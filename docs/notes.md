RP sits in DMZ

6.6.2025
Kubernetes 1.31 fully linked and ready
9.6.2025
discovered HA was not enabled
10.6.2025
disassembled kubernetes 1.31 and upgraded to 1.32
HA was enabled
swap was turned off, directory ~/scrips a scp looping script was made to turn swap off on all remaining nodes
15.6.2025
Added a new Kubernetes Cluster node: UI (Pi 4 with a touchscreen GUI)
Created microk8s dashboard-proxy and sucessfully tested dashboard-proxy locally as well as remotely, documentation for access added to docs
16.6.2025
Researching kubernetes application and created roadmap for self-hosted services
17.6.2025
Updated inconsistency with hostnames on Odrerir, a Pi 4 node. relabelled roles on kubernetes cluster

Inventory
Dell
24 TB 
64 gb ECC ram

# 📘 Cluster Infrastructure - Phase 1 Notes & Rationale

These are the foundational components for building, managing, and protecting the Kubernetes-based self-hosted AI system, with support for mixed ARM64 and x86_64 architecture. This GitOps-first system is designed to be resilient, secure, and repeatable.

---

## 🧠 1. Core Cluster Management & GitOps

| Tool             | Purpose                            | Architecture     | Install on              | Rationale |
|------------------|-------------------------------------|------------------|--------------------------|-----------|
| **MicroK8s**      | Lightweight Kubernetes distribution | ARM64 + x86_64   | All nodes                | Simple, modular, and supports high-availability out of the box; great for hybrid node setups. |
| **FluxCD / ArgoCD** | Declarative app management         | ARM64 + x86_64   | Master (x86 preferred)   | Enables GitOps—cluster and app state is driven by Git. ArgoCD for visual UI; FluxCD for lightweight installs. |
| **Sealed Secrets** | Encrypt secrets for GitOps         | ARM64 + x86_64   | Master (x86) or shared   | Keeps secrets safe in Git; decryption only possible inside the cluster. |
| **KubeStateMetrics** | Export Kubernetes state metrics    | ARM64 + x86_64   | Master (x86)             | Enables detailed cluster visibility via Prometheus and Grafana. |

---

## 🧱 2. Storage & Backup

| Tool             | Purpose                            | Architecture     | Install on              | Rationale |
|------------------|-------------------------------------|------------------|--------------------------|-----------|
| **OpenEBS**       | CSI volume provisioning (ZFS/ext4) | ARM64 + x86_64   | ZFS on Master, hostpath on Pi 5s | Allows dynamic and static volume provisioning with support for both Pi and ZFS-capable nodes. |
| **Velero**        | Kubernetes-native backup/restore   | ARM64 + x86_64   | Backup Pi node (preferred), Master (x86) | Ensures full cluster snapshot and restore. Can target cloud/S3 or local PVCs. |
| **Restic / Kopia**| File-level PVC backups             | ARM64 + x86_64   | Used by Velero           | Adds finer-grained backups of individual files or apps, integrates with Velero. |
| **ZFS**           | Native volume + snapshot system    | x86_64 only      | Master (x86) only        | Supports efficient snapshots, replication, and data integrity; ideal for Odin and long-term data. |
| **CSI snapshot CRDs** | Enables snapshot management     | All              | All nodes                | Required for CSI drivers to support Kubernetes-native volume snapshotting. |
| **MinIO (optional)** | S3-compatible object storage     | ARM64 + x86_64   | Master (x86) or Backup Pi | Enables remote backups in S3 format—self-hosted, great for redundancy and air-gapped clusters. |

---

## 📡 3. Observability & Monitoring

| Tool             | Purpose                            | Architecture     | Install on              | Rationale |
|------------------|-------------------------------------|------------------|--------------------------|-----------|
| **Prometheus**    | Metrics & cluster health            | ✅                | Master (x86)             | Foundation of observability stack; scrapes data from exporters for alerting and dashboards. |
| **Grafana**       | Dashboards                          | ✅                | Master (x86)             | Visualizes metrics from Prometheus and logs from Loki; extensible with plugins. |
| **Node Exporter** | Node-level metrics                  | ✅                | All nodes                | Provides CPU, memory, disk, and other system-level metrics to Prometheus. |
| **Alertmanager**  | Send alerts                         | ✅                | Master (x86)             | Trigger notifications (email, Discord, Slack, etc.) based on Prometheus alert rules. |
| **KubeLens / Lens (optional)** | GUI for cluster         | ✅                | Local dev machine        | Optional developer-friendly GUI for visualizing pods, logs, and metrics. |

---

## 🔁 4. Job Scheduling & Automation

| Tool             | Purpose                            | Architecture     | Install on              | Rationale |
|------------------|-------------------------------------|------------------|--------------------------|-----------|
| **CronJobs**      | Schedule GC, snapshots, syncs       | ✅                | Backup Pi & Master (x86) | Useful for regular automation—e.g., cleanups, snapshot rotation, model syncs. |
| **Rsync**         | Copy snapshots to Odin              | ✅                | Backup Pi & Master (x86) | Reliable tool for synchronizing files from PVCs or datasets to persistent ZFS-backed storage. |
| **Kustomize**     | Declarative overlay management      | ✅                | Dev workstation          | Enables layering and patching Kubernetes manifests; GitOps-friendly. |
| **Helm**          | App install & config templating     | ✅                | Master (x86)             | Preferred tool for deploying complex charts like Loki, Wazuh, and Grafana with parameterization. |

---

## 🌐 5. Access & Control

| Tool             | Purpose                            | Architecture     | Install on              | Rationale |
|------------------|-------------------------------------|------------------|--------------------------|-----------|
| **Traefik / NGINX Ingress** | Routing apps            | ✅                | Master (x86)             | Handles Ingress routing, TLS termination, load balancing, and route protections. |
| **Cert-Manager**  | TLS certificate automation          | ✅                | Master (x86)             | Automatically provisions and renews TLS certificates (Let’s Encrypt, CA, etc.). |
| **OAuth2 Proxy**  | Auth proxy for apps                 | ✅                | Master (x86)             | Protects internal dashboards (Grafana, ArgoCD) with Google, GitHub, or OIDC logins. |

---

## 📌 Phase 1 Strategy

- Focus on building **resilient infrastructure** before deploying advanced workloads (like Ollama or data pipelines).
- Embrace **GitOps**: every service should be deployed via FluxCD or ArgoCD with Helm/Kustomize.
- Prioritize **data safety**: use ZFS snapshots and Velero for cluster backups, especially for AI-generated content.

---

> _Built to be modular, self-healing, and secure—from Odin to Gjallarhorn._
