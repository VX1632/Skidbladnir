# SECURITY_ALERTS

A concise, living list of recent security incidents and supply-chain events relevant to this project. Each entry includes a short summary and recommended next steps for maintainers running mixed-architecture Kubernetes clusters.

---

## Table of contents
- [Recent supply-chain incidents](#recent-supply-chain-incidents)  
- [Kubernetes-specific risks & resources](#kubernetes-specific-risks--resources)  
- [Immediate actions & mitigations](#immediate-actions--mitigations)  
- [Notes](#notes)

---

## Recent supply-chain incidents

### 1. "The Rise of Non-Ransomware Attacks on AWS S3 Data"  
**Source:** Thales CPL — analysis of attacks that abuse S3 server-side encryption with customer-provided keys (SSE-C). [Thales Blog](https://cpl.thalesgroup.com/blog/data-security/the-rise-of-non-ransomware-attacks-on-aws-s3-data)
**Summary:** Attackers can leverage misconfigured S3 encryption and key-management processes to hold data hostage or exfiltrate data without using classic ransomware payloads. This type of attack highlights the need for rigorous key management and CloudTrail monitoring of unusual SSE-C parameters. :contentReference[oaicite:0]{index=0}  
**Short action:** Audit S3 buckets and SSE usage, enforce KMS-based encryption policies (SSE-KMS), enable S3 protection in cloud threat detection, and review CloudTrail for anomalous `x-amz-server-side-encryption-customer-algorithm` events. :contentReference[oaicite:1]{index=1}

### 2. Widespread NPM supply-chain attack — debug / chalk (npm)  
**Source:** Wiz analysis of the debug/chalk incident.  [Wiz Blog](https://www.wiz.io/blog/widespread-npm-supply-chain-attack-breaking-down-impact-scope-across-debug-chalk)
**Summary:** Malicious updates to widely-used npm packages resulted in wallet-hijacking code and rapid propagation across dependent projects. The incident shows how quickly supply-chain compromises can spread and why publish-time protections are critical. :contentReference[oaicite:2]{index=2}  
**Short action:** Pin dependencies where possible, validate package checksums (SLSA/SBOM workflows), perform dependency scanning, and remove affected versions from registries or CI caches.

### 3. "s1ngularity" / Nx supply-chain attack (Aikido / reporting)  
**Source:** Aikido Security / multiple reports — Nx packages compromised, attackers exfiltrated tokens and secrets.  [Aikido Blog](https://www.aikido.dev/blog/s1ngularity-nx-attackers-strike-again) 
**Summary:** Malicious Nx package versions were used to harvest developer secrets (GitHub tokens, SSH keys, etc.), leaking thousands of credentials and potentially exposing cloud resources. This demonstrates the downstream risk to CI/CD and Kubernetes deploy pipelines. :contentReference[oaicite:3]{index=3}  
**Short action:** Rotate exposed tokens, audit CI/CD secrets, enforce least-privilege tokens, and add repository-level secret scanning and ephemeral credentials for automation.

### 4. Rapid targeting of new clusters / attack surface of fresh deployments  
**Source:** Wiz — Kubernetes Security Report (2025): new clusters get probed within minutes.  [Wiz Kubernetes Security Report](https://www.wiz.io/blog/kubernetes-security-report-2025)
**Summary:** Freshly-created clusters are scanned and targeted quickly; public exposure of control planes or misconfigured RBAC vastly increases risk. Harden cluster bootstrap processes and avoid publicly exposing the API server. :contentReference[oaicite:4]{index=4}  
**Short action:** Harden kube-apiserver accessibility, enable network policies, and delay publishing cluster endpoints until hardened.

### 5. Most common Kubernetes misconfigurations (RBAC, privileged containers, etc.)  
**Source:** Picus / OWASP Kubernetes cheatsheet — lists top misconfigurations and hardening guidance. 
- [Picus: Most Common Kubernetes Security Misconfigurations](https://www.picussecurity.com/resource/blog/most-common-kubernetes-security-misconfigurations)  
- [OWASP Kubernetes Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)  
**Summary:** Overly permissive RBAC, missing network policies, running containers as root, and insecure admission/controller settings are persistently common and exploitable. Regular misconfiguration scanning should be part of CI. :contentReference[oaicite:5]{index=5}  
**Short action:** Audit RBAC, block privileged escalation, enforce Pod Security Standards, and run periodic configuration checks.

### 6. Container/image supply-chain & registry hygiene  
**Sources:** Industry reporting on container supply-chain trends and The Hacker News / TechRadar coverage of npm/container compromises.  
- [The Hacker News: NPM Supply-Chain Attack Coverage](https://thehackernews.com/)  
- [TechRadar: Container Security Trends](https://www.techradar.com/) 
**Summary:** Image or registry compromises allow poisoned images to be deployed to clusters. Protect registry access, require image signing (cosign) where feasible, and implement runtime image allow-lists or admission controllers that validate provenance. :contentReference[oaicite:6]{index=6}  
**Short action:** Use signed images, restrict registry push permissions, and scan images for malware and known-vulnerable packages before deployment.

---

## Immediate actions & mitigations (checklist)

1. **Dependency & image hygiene**
   - Pin and audit third-party dependencies.  
   - Enable SBOM generation and verify package checksums. :contentReference[oaicite:7]{index=7}

2. **Secrets & tokens**
   - Rotate service and CI tokens that might have been exposed.  
   - Enforce short-lived credentials and use OIDC / workload identities for cloud API access. :contentReference[oaicite:8]{index=8}

3. **Registry & image provenance**
   - Require image signing (cosign) and validate in admission controllers.  
   - Scan images on push and before deploy. :contentReference[oaicite:9]{index=9}

4. **Cluster hardening**
   - Lock down kube-apiserver access (IP allowlist / private endpoint).  
   - Enforce least-privilege RBAC and Pod Security Standards. :contentReference[oaicite:10]{index=10}

5. **Monitoring & detection**
   - Enable CloudTrail-like audit logging for control-plane/cloud calls and monitor for unusual SSE-C or encryption key activity. :contentReference[oaicite:11]{index=11}

6. **CI/CD hygiene**
   - Revoke and rotate compromised CI tokens, enable repo secret scanning, and avoid long-lived tokens in pipelines. :contentReference[oaicite:12]{index=12}

7. **Incident playbooks**
   - Maintain a short incident runbook: revoke keys, isolate workloads, rotate secrets, scan images and nodes, and restore from verified backups.


---

## Notes
- This file is intended to be short and action-oriented. Add entries as new incidents appear, keep links to primary reporting, and record any cluster-specific remediation steps taken (dates, PR/issue numbers, rotated keys).  
- Refer to docs/feeds.yml. :contentReference[oaicite:13]{index=13}

---

