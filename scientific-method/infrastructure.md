# Infrastructure Diagnostics

[Back to the Scientific Method](README.md)

Infrastructure is everything your software runs on: hosts, virtual machines, containers, orchestrators, networks, DNS, load balancers, certificates, and storage. It's also where the USE method (Utilization, Saturation, Errors) earns its keep. Check every resource against all three, and most infrastructure problems show themselves.

For provider-managed services, quotas, IAM, and billing, see [Cloud](cloud.md).

## Typical symptoms

- A host or node is slow, unresponsive, or keeps restarting
- Containers are `OOMKilled` or stuck in `CrashLoopBackOff`
- Connection timeouts or resets between services
- Name resolution fails or resolves to the wrong address
- TLS handshake errors or expired-certificate warnings
- The disk fills up, or I/O latency climbs
- Load balancer health checks flap

## Observation tooling

| Need | Tools |
|---|---|
| CPU, memory, load at a glance | `top` / `htop`, `uptime`, `vmstat 1`, `mpstat -P ALL 1`, `free -m` |
| Disk I/O and space | `iostat -xz 1`, `df -h`, `df -i` (inodes), `du -sh`, `ncdu`, `lsof +L1` (deleted-but-open files) |
| Kernel and service logs | `dmesg -T` (OOM killer, hardware errors), `journalctl -u <service> --since` |
| What a process is doing | `strace -f -p <pid>`, `perf top`, `perf record`, `lsof -p <pid>`, `/proc/<pid>/status` |
| History of resource use | `sar` (sysstat), Prometheus `node_exporter` + Grafana, your monitoring agent |
| Sockets and connections | `ss -tanp` (state counts, e.g. `TIME_WAIT`, `CLOSE_WAIT`), `netstat` on older systems |
| Packets on the wire | `tcpdump`, Wireshark, `tshark` |
| Path and reachability | `ping`, `mtr`, `traceroute`, `nc -zv host port`, `curl -v --connect-timeout` |
| DNS | `dig +trace`, `dig @<specific-resolver>`, `nslookup`, `/etc/resolv.conf`, CoreDNS logs in Kubernetes |
| TLS | `openssl s_client -connect host:443 -servername host`, `openssl x509 -noout -dates -in cert.pem`, `curl -v`, SSL Labs for public endpoints |
| Containers | `docker stats`, `docker inspect`, `docker logs`, `crictl` |
| Kubernetes | `kubectl describe pod`, `kubectl logs --previous`, `kubectl get events --sort-by=.lastTimestamp`, `kubectl top`, `kubectl debug` (ephemeral debug containers) |

Brendan Gregg's "Linux Performance Analysis in 60,000 Milliseconds" checklist is a good first pass: `uptime`, `dmesg | tail`, `vmstat 1`, `mpstat -P ALL 1`, `pidstat 1`, `iostat -xz 1`, `free -m`, `sar -n DEV 1`, `sar -n TCP,ETCP 1`, `top`.

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Container `OOMKilled` | (a) Memory limit set below real working set (b) Application memory leak (c) Runtime not container-aware (heap sized to host memory) (d) Page cache or tmpfs counted against the limit | Graph container memory over its lifetime. A plateau above the limit means (a), steady growth means (b). Check runtime flags (for example, JVM `-XX:MaxRAMPercentage`). Inspect `memory.stat` in the cgroup for cache vs. RSS. |
| Intermittent connection timeouts | (a) Ephemeral port or conntrack table exhaustion (b) Packet loss on a path (c) Server-side backlog full (d) Idle connections closed by a middlebox or load balancer | `ss -s` and `conntrack -S` during the incident. `mtr` to the target. Check listen queue overflows (`netstat -s \| grep -i listen`). Compare timeout timing to LB idle timeout. |
| DNS fails intermittently | (a) Resolver overloaded or rate-limited (b) Kubernetes `ndots:5` search-path amplification (c) Stale cached record after a change (d) UDP packet loss or conntrack race | Time lookups in a loop from the affected pod. Count queries per lookup in CoreDNS logs. Query the authoritative server directly with `dig +trace`. Test with a fully qualified name (trailing dot). |
| TLS handshake fails for some clients | (a) Expired or not-yet-valid cert (b) Missing intermediate certificate (c) Protocol or cipher mismatch (d) SNI not sent or wrong | `openssl s_client -showcerts` to see the chain served. Test with `-tls1_2` / `-tls1_3`. Compare with and without `-servername`. |
| Disk full but `du` doesn't add up | (a) Deleted files held open by a process (b) Inodes exhausted (c) Filesystem reserved blocks (d) A different mount than you think | `lsof +L1` for (a). `df -i` for (b). `tune2fs -l` for (c). `findmnt` for (d). |
| Health checks flap | (a) Check timeout shorter than a slow startup or GC pause (b) Check hits a dependency that's degraded (c) Resource saturation on the node | Log check latency. Make the liveness check local-only and compare. Correlate with node CPU steal, memory pressure, or I/O wait. |

## Worked example: pods OOMKilled after a base image upgrade

1. **Observe.** After moving the invoice service to a new base image, pods restart every 40–90 minutes with `OOMKilled`. The limit is 1 GiB. Before the upgrade, pods ran for weeks.
2. **Question.** Why do invoice-service pods exceed their 1 GiB memory limit since the base image upgrade?
3. **Research.** The new image moves the JVM from 11 to 17. The Dockerfile sets `-Xmx768m`. Nothing else changed in the application.
4. **Hypothesize.**
   - H1: Non-heap memory (metaspace, thread stacks, direct buffers, code cache) grew under the new JVM, and heap plus non-heap now exceeds 1 GiB.
   - H2: An application leak that existed before is now exposed by different GC behavior.
   - H3: The new image writes logs to a tmpfs that counts against the memory limit.
5. **Predict.** H1: JVM Native Memory Tracking shows total committed memory near 1 GiB with heap stable under 768 MB. H2: heap usage after full GC climbs steadily. H3: `memory.stat` shows large `shmem` or tmpfs usage.
6. **Experiment.** Run one pod with `-XX:NativeMemoryTracking=summary` and take `jcmd <pid> VM.native_memory summary` every 10 minutes. Scrape heap-after-GC metrics. Read the cgroup's `memory.stat`.
7. **Analyze.** Heap after GC is flat at ~420 MB, so H2 is refuted. `shmem` is negligible, so H3 is refuted. NMT shows heap committed at 768 MB plus ~310 MB of non-heap (thread stacks for 200 threads, metaspace, code cache, direct buffers), rising as the thread pool fills up. H1 is supported. The new image's default thread pool size also doubled, adding stack memory.
8. **Conclude.** Replaced `-Xmx` with `-XX:MaxRAMPercentage=65` so heap scales with the limit. Capped the thread pool at its old size. Raised the limit to 1.25 GiB after measuring real peak usage. Added an alert when container memory exceeds 85% of its limit for 10 minutes.

## Experiment techniques

- **Use the USE method, resource by resource.** For CPU, memory, disk, network, and any pools: how busy (utilization), how much is queuing (saturation), how many errors. Write it down in a grid.
- **Isolate with a debug pod.** `kubectl debug` or a sidecar with network tools lets you test DNS, routes, and ports from *exactly* where the application runs.
- **Compare a healthy node to a sick one.** Differential diagnosis works well here. Diff kernel versions, sysctls, mounted volumes, and node labels.
- **Capture during the event.** Many infrastructure problems are transient. Set up a triggered capture (a `tcpdump` ring buffer, periodic `ss -s` snapshots) so the next occurrence leaves evidence.
- **Change limits one at a time.** Memory limit, CPU limit, and thread pool size interact. Adjusting all three at once hides which one mattered.

## AI prompts that help

- "Here's the output of `vmstat 1`, `iostat -xz 1`, and `ss -s` from a sick node and a healthy node. Apply the USE method and tell me which resource looks saturated."
- "Here's `kubectl describe pod` and the previous container's logs. List the possible reasons for `CrashLoopBackOff` in order of likelihood, and what to check for each."
- "Here's the `openssl s_client -showcerts` output. Is the chain complete, and what would fail for a client with an older trust store?"

**Pitfall:** AI often suggests raising the memory or CPU limit. That can be the right answer, but only after you know whether usage is a plateau (limit too low) or a slope (leak). Raising the limit on a leak just changes how often it crashes.

## Theory behind the playbook

- [CS08 Operating Systems](../computer-science-in-ai-curriculum/talks/08-operating-systems.md): processes, memory management, and file systems
- [CS09 Computer Networks](../computer-science-in-ai-curriculum/talks/09-computer-networks.md): TCP, DNS, and routing
- [CS06 Computer Organization & Architecture](../computer-science-in-ai-curriculum/talks/06-computer-organization-and-architecture.md): caches, memory hierarchy, and I/O
- [CS16 Cryptography](../computer-science-in-ai-curriculum/talks/16-cryptography.md): certificates and TLS
- [SDA16 DNS, CDNs & Load Balancers](../computer-science-software-design-and-architecture/curriculum/part-4/part-4-16--dns-cdns-load-balancers/README.md)
- [SDA17 Scaling Applications](../computer-science-software-design-and-architecture/curriculum/part-4/part-4-17--scaling-applications/README.md)
- [SDA33 Operations & DevOps Knowledge](../computer-science-software-design-and-architecture/curriculum/part-8/part-8-33--operations-devops-knowledge/README.md): containers and Linux fundamentals
