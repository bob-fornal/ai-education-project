# Security Diagnostics

[Back to the Scientific Method](README.md)

Security work has a different shape from the other playbooks. You're often proving a *negative* ("this isn't exploitable," "no data left the system"), working against an adversary who adapts, and handling evidence that may matter legally. The scientific method fits well because it forces you to separate what you know from what you fear, and to write both down.

> **Authorization first.** Only test, scan, or probe systems you own or have written permission to test. During a suspected incident, follow your organization's incident response plan. Preserve evidence before changing anything, and involve your security team early.

## Typical situations

- A scanner reports hundreds of vulnerabilities and you need to know which ones matter
- A dependency has a newly published CVE
- An alert fires for suspicious logins, unusual API usage, or data transfer
- A pentest or bug bounty report claims a vulnerability
- You need to prove a control actually works (MFA enforced, encryption at rest, least privilege)
- A secret may have been committed or leaked
- AI-generated code needs a security review before merge

## Observation tooling

| Need | Tools |
|---|---|
| Static analysis of your code (SAST) | Semgrep, CodeQL, SonarQube, language linters with security rules (Bandit, ESLint security plugins, gosec) |
| Dynamic testing of running apps (DAST) | OWASP ZAP, Burp Suite, Nuclei (authorized targets only) |
| Dependency vulnerabilities (SCA) | Dependabot, Renovate, Snyk, `npm audit`, `pip-audit`, OWASP Dependency-Check, OSV-Scanner |
| Container and IaC scanning | Trivy, Grype, Checkov, tfsec, kube-bench |
| Secrets in code and history | gitleaks, TruffleHog, GitHub secret scanning / push protection |
| Deciding what matters | CVSS for severity, **EPSS** for exploit likelihood, the **CISA KEV** catalog for known active exploitation, and reachability analysis from your SCA tool |
| Event correlation and hunting | SIEM tools such as Splunk, Elastic Security, Microsoft Sentinel, or Google SecOps |
| Cloud activity | CloudTrail, Azure Activity Log / Entra ID sign-in logs, GCP Audit Logs, GuardDuty, Defender for Cloud, Security Command Center |
| Endpoint and host evidence | Your EDR platform, `auditd`, `journalctl`, osquery |
| Threat modeling | STRIDE, OWASP Threat Dragon, Microsoft Threat Modeling Tool, attack trees |
| Verifying headers and TLS | `curl -I`, securityheaders.com, SSL Labs, `testssl.sh` |

## Common hypotheses and how to tell them apart

| Situation | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Critical CVE in a dependency | (a) Vulnerable code path is reachable from your inputs (b) Package is present but the vulnerable function isn't used (c) Only in dev or test dependencies (d) Already mitigated by configuration or a WAF | Check reachability in the SCA tool, or trace call sites to the vulnerable function. Confirm whether the package ships in the production artifact (`npm ls --omit=dev`, SBOM). Write a proof-of-concept test *in a safe environment* that exercises the vulnerable input through your real entry point. |
| Suspicious login alert | (a) Credential stuffing or account takeover (b) Legitimate user on VPN or travel (c) Misconfigured automation using a human account (d) Detection rule false positive | Correlate IP, ASN, user agent, and device ID with the user's history. Check for MFA challenges passed or failed. Look for other accounts hit from the same source. Contact the user through a known channel. |
| Pentest reports SQL injection | (a) Real injection through string concatenation (b) Input reflected in errors but parameterized in the query (c) Scanner false positive | Reproduce with the reported payload in a test environment. Read the code path. Check database logs for the query as executed. Parameterized queries show bound values, not concatenated SQL. |
| Secret committed to a repo | (a) Secret is live and was exposed publicly (b) Secret is live but the repo is private and access-limited (c) Secret was already rotated or is a test value | **Rotate first, investigate second.** Then check the provider's audit log for use of the credential since the commit timestamp. Check repo visibility history and fork/clone logs. |
| Is MFA really enforced? | (a) Enforced everywhere (b) Legacy auth protocols bypass it (c) Some accounts or apps are excluded | Try signing in to a test account over each protocol and app in scope. Query sign-in logs for successful sign-ins without MFA. Review conditional access exclusions. |

## Worked example: triaging a "critical" dependency CVE

1. **Observe.** The SCA tool flags a CVSS 9.8 deserialization vulnerability in a JSON library used by the billing service. The advisory says exploitation requires deserializing untrusted input with polymorphic type handling enabled.
2. **Question.** Can an attacker reach the vulnerable deserialization path in the billing service with input they control?
3. **Research.** EPSS is 0.4 (high), and the CVE is in CISA KEV, so it's being exploited in the wild. The library is a direct production dependency. The advisory names the specific setting that must be on for the vulnerability to apply.
4. **Hypothesize.**
   - H1: Billing enables polymorphic typing somewhere and deserializes request bodies with it. Exploitable.
   - H2: The library is present, but polymorphic typing is never enabled. Not exploitable as-is.
   - H3: Polymorphic typing is enabled only for an internal queue consumer whose messages come from a trusted producer. Exploitable only if the queue is compromised.
5. **Predict.** H1: code search finds the setting on an object mapper used by an HTTP controller, and a PoC payload sent to that endpoint in a test environment triggers the gadget class load. H2: no code or config enables it. H3: the setting appears only in the queue consumer's configuration.
6. **Experiment.** Search the code and config for the enabling setting and annotations. Trace which mapper instances are used where. In an isolated test environment, send a benign PoC payload (one that tries to load a harmless class and logs it) to each entry point that uses an affected mapper.
7. **Analyze.** The setting is enabled on a mapper in `LegacyInvoiceImportController`, which accepts uploaded JSON from customer admins. The PoC payload caused the test class to load. H1 is supported. H3 is also partly true: the queue consumer enables it too.
8. **Conclude.** Treated as an emergency. Upgraded the library, removed polymorphic typing from both mappers, and added an allowlist-based type validator where polymorphism was genuinely needed. Checked logs back to the library's introduction for payloads matching the gadget pattern and found none. Added a Semgrep rule that fails CI if the setting is re-enabled. Recorded the reachability evidence in the ticket so auditors can see *why* it was handled as it was.

## Experiment techniques

- **Prove reachability, not just presence.** Most scanner findings aren't exploitable in context. Reachability evidence is what lets you prioritize the few that are, and defend the decision later.
- **Use benign proofs of concept.** Show that a path is reachable with a payload that logs or loads a harmless class. You don't need a working exploit to prove the risk.
- **Preserve before you change.** During an incident, snapshot disks, export logs, and record timestamps *before* remediation destroys evidence. Coordinate with your security or legal team.
- **Build the timeline from several sources.** Attackers can tamper with one log. Correlate application logs, cloud audit logs, identity provider logs, and network flow logs.
- **Test controls from the attacker's side.** A policy document says MFA is enforced. A login attempt over IMAP tells you whether it is.

## Reviewing AI-generated code for security

AI-generated code has well-documented, repeatable blind spots. Check these on every AI-assisted change:

- [ ] Input validation and output encoding (injection, XSS)
- [ ] Parameterized queries, never string-built SQL
- [ ] Authorization checks on every endpoint and object access, not just authentication
- [ ] No hardcoded secrets, keys, or example credentials left in
- [ ] Safe deserialization and file handling (path traversal, unrestricted upload)
- [ ] Cryptography uses vetted libraries and modern algorithms; no homemade crypto, no MD5/SHA-1 for passwords
- [ ] Dependencies it added actually exist, are maintained, and are the package you think they are (watch for typosquatting and hallucinated package names)
- [ ] Error messages don't leak stack traces or internal details

## AI prompts that help

- "Here's the advisory text and the three places we use this library. For each, tell me whether the vulnerable condition applies and what evidence would confirm it."
- "Review this diff for OWASP Top 10 issues. For each finding, cite the line and explain the exploit path. Say 'none found' for categories that don't apply."
- "Write a benign proof-of-concept test that shows whether this endpoint deserializes attacker-controlled type information, without executing anything harmful."

**Pitfall:** Never paste secrets, customer data, incident evidence, or unreleased vulnerability details into an AI tool that isn't approved for that data. And AI "no issues found" is not a security review. It's one more scanner, with its own false negatives.

## Theory behind the playbook

- [CS15 Computer & Network Security](../computer-science-in-ai-curriculum/talks/15-computer-and-network-security.md)
- [CS16 Cryptography](../computer-science-in-ai-curriculum/talks/16-cryptography.md): RSA, SHA-256, and why homemade crypto fails
- [CS27 Computing Ethics, Policy & Society](../computer-science-in-ai-curriculum/talks/27-computing-ethics-policy-and-society.md): disclosure and responsibility
- [SDA28 Cloud Security Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-28--cloud-security-patterns/README.md): gatekeeper, valet key, federated identity
- [SDA32 Security for Architects](../computer-science-software-design-and-architecture/curriculum/part-8/part-8-32--security-for-architects/README.md): hashing, PKI, OWASP Top 10, auth strategies
