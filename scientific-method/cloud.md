# Cloud Diagnostics

[Back to the Scientific Method](README.md)

Cloud problems come from running on someone else's platform. You get managed services, elastic capacity, and global reach, and in exchange you get quotas you didn't know existed, permission models with many layers, bills that react to bugs, and failure modes you can't see inside. Diagnosis here leans heavily on the provider's own telemetry and audit logs.

This playbook covers the managed-platform layer. For the hosts, containers, and networks you run on top of it, see [Infrastructure](infrastructure.md).

## Typical symptoms

- Requests fail with throttling errors (HTTP 429, `ThrottlingException`, `TooManyRequests`)
- `AccessDenied` / `403` / `AuthorizationFailed` for something that "should" be allowed
- The bill spikes with no matching traffic increase
- A serverless function is slow on some requests (cold starts) or times out
- A managed service behaves differently than the docs suggest
- Problems in one region or zone only
- Scaling doesn't kick in, or kicks in too late

## Observation tooling

| Need | AWS | Azure | Google Cloud |
|---|---|---|---|
| Metrics and dashboards | CloudWatch Metrics | Azure Monitor Metrics | Cloud Monitoring |
| Logs | CloudWatch Logs, Logs Insights | Log Analytics (KQL) | Cloud Logging |
| Distributed tracing | X-Ray, or OpenTelemetry via ADOT | Application Insights | Cloud Trace |
| Who changed what | **CloudTrail** | **Activity Log** | **Cloud Audit Logs** |
| Permission debugging | IAM Policy Simulator, IAM Access Analyzer, CloudTrail `errorCode` | Access control (IAM) "Check access", Activity Log | Policy Troubleshooter, Policy Analyzer |
| Quotas and limits | Service Quotas console, Trusted Advisor | Usage + quotas blade | IAM & Admin → Quotas |
| Cost | Cost Explorer, Cost and Usage Report, Cost Anomaly Detection | Cost Management + Billing, cost alerts | Billing reports, BigQuery billing export |
| Provider-side incidents | AWS Health Dashboard | Azure Service Health | Google Cloud Service Health |

For edge and serverless platforms, use the platform's own tooling. On Cloudflare Workers, for example, `wrangler tail` streams live logs and the dashboard shows per-Worker CPU time and errors.

Also useful across providers: infrastructure-as-code state (`terraform state show`, `pulumi stack`), tagging reports for cost allocation, and the CLI (`aws`, `az`, `gcloud`) with `--debug` to see raw API calls and responses.

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Throttling errors | (a) Account or regional service quota reached (b) Per-resource limit (partition, shard, or table throughput) (c) Burst credits exhausted (d) Your own retry loop multiplying calls | Compare the request rate to the documented and actual quota. Check per-partition metrics for hot keys in (b). Look at credit balance metrics in (c). Graph *attempted* calls vs. *unique* operations for (d). |
| Access denied | (a) Missing permission in the identity policy (b) Explicit deny in a resource policy, SCP, or management-group policy (c) Wrong identity in use (d) Condition key mismatch (IP, VPC endpoint, tags, MFA) | Find the denied call in the audit log. It shows the principal, action, resource, and often the policy type that denied it. Run the policy simulator or troubleshooter with those exact values. Print the caller identity (`aws sts get-caller-identity`, `az account show`, `gcloud auth list`). |
| Cost spike | (a) Runaway loop (function invoking itself, recursive trigger) (b) Log or data egress explosion (c) Resources left running (d) Pricing-tier change or an expired commitment | Break cost down by service, then by resource tag, then by day. Correlate with invocation counts and egress metrics. Check for recent resource creation in the audit log. |
| Serverless latency spikes | (a) Cold starts (b) Concurrency limit reached, requests queued or throttled (c) Downstream connections re-created on every invoke (d) Oversized package or memory setting | Split latency by "init" vs. "invoke" (for example, `Init Duration` in AWS Lambda logs). Check concurrency and throttle metrics. Time connection setup inside the handler. Rerun with a different memory setting. |
| One region is failing | (a) Provider incident (b) Regional quota or capacity (c) Config differs between regions (d) Data replication lag into that region | Check the provider health dashboard. Diff IaC outputs between regions. Measure replication lag. Fail a small percentage of traffic over and compare. |

## Worked example: a 6x jump in the monthly storage bill

1. **Observe.** Cost anomaly detection flags object storage costs up from ~$1,200/month to a projected ~$7,400. Request counts to the app are flat. The increase started nine days ago.
2. **Question.** What's driving the object storage cost increase that started nine days ago, given flat app traffic?
3. **Research.** Breaking cost down by usage type shows the growth is almost all in `PUT`/`COPY`/`POST`/`LIST` request charges, not storage volume or egress. Nine days ago a thumbnail-generation function was deployed. It's triggered by object-created events on the bucket.
4. **Hypothesize.**
   - H1: The thumbnail function writes its output to the same bucket and prefix it listens on, so it triggers itself recursively.
   - H2: A lifecycle rule change is copying objects between storage classes repeatedly.
   - H3: A client retry bug is re-uploading files many times.
5. **Predict.** H1: function invocations far exceed uploads, and object keys show nested patterns like `thumb_thumb_thumb_...`. H2: the audit log shows a lifecycle config change around day 0, and request charges are transition requests. H3: application logs show repeated uploads with the same content hash from clients.
6. **Experiment.** Compare function invocation count to user uploads for one day. List the bucket for keys matching `thumb_thumb_`. Search the audit log for lifecycle configuration changes. Group upload logs by content hash.
7. **Analyze.** 4,100 user uploads and 3.9 million function invocations in one day. Tens of thousands of keys like `thumb_thumb_thumb_...jpg`. No lifecycle changes. No duplicate client uploads. H1 is supported. The recursion stops only when key names hit the maximum length, which is why it didn't run forever.
8. **Conclude.** Moved thumbnail output to a separate bucket. Added an event filter so the function only fires on the `uploads/` prefix. Cleaned up the recursive objects with a scripted, reviewed batch delete. Added a budget alert per service and an alarm when function invocations exceed 3x uploads.

## Experiment techniques

- **Start with the audit log.** For permission errors and "what changed" questions, the audit log usually answers the question outright, and it's faster than reasoning about policy documents.
- **Break costs down along several dimensions.** Service → usage type → resource/tag → day. Cost spikes almost always resolve to one line item once you slice finely enough.
- **Test in a sandbox account or subscription.** Quotas, IAM, and service behavior can be tested safely in an isolated account with the same organization policies.
- **Read quotas, don't assume them.** Default quotas vary by account age, region, and service. Query the current values rather than trusting documentation or memory.
- **Include provider status in every timeline.** Check it early. It rules a whole class of hypotheses in or out quickly. A green status page isn't proof of health, though. Small incidents are often not posted.

## AI prompts that help

- "Here's the CloudTrail event for the denied call and the three policies attached to this role. Walk through policy evaluation logic and tell me which statement causes the deny."
- "Here's a cost breakdown by usage type for the last 30 days. What patterns suggest a runaway process versus organic growth?"
- "Write a CloudWatch Logs Insights (or KQL) query that shows p50/p95/p99 duration split by cold start vs. warm invoke, per hour."

**Pitfall:** AI-suggested IAM fixes often grant `*` actions or resources to make an error go away. That resolves the symptom and opens a security hole. Grant the specific action on the specific resource the audit log names.

## Theory behind the playbook

- [CS10 Distributed Systems](../computer-science-in-ai-curriculum/talks/10-distributed-systems.md): partial failure, replication, and regions
- [CS15 Computer & Network Security](../computer-science-in-ai-curriculum/talks/15-computer-and-network-security.md): least privilege
- [SDA25 Cloud Messaging Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-25--cloud-messaging-patterns/README.md): queue-based load leveling and competing consumers
- [SDA26 Cloud Data Management Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-26--cloud-data-management-patterns/README.md)
- [SDA27 Reliability & Resiliency Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-27--reliability-resiliency-patterns/README.md): throttling, deployment stamps, geodes
- [SDA28 Cloud Security Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-28--cloud-security-patterns/README.md): federated identity, gatekeeper, valet key
