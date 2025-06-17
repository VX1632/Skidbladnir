# ⚙️ Self-Hosted Infrastructure Roadmap (MicroK8s-based)

## 🌐 Infrastructure Overview

| Node      | Role(s)                                                                 |
|-----------|-------------------------------------------------------------------------|
| Odin      | Control Plane, Longhorn, AI Datasets, Logging                           |
| Thor      | Control Plane, Traefik, Loki, GitOps Agent                              |
| Sindri    | Control Plane, Backups, GitOps Replica                                  |
| Pi 5s     | Workload Nodes, DNS, App Servers, Data Preprocessing                    |
| Protectli | Firewall, DNS via Unbound, VLAN Routing, VPN Entrypoint                |

---

## ✅ Installation Roadmap

### Phase 1: Bootstrap Cluster
- [x] MicroK8s (HA, control plane with Odin, Thor, Sindri)
- [ ] Netmaker (Overlay Network + WireGuard)
- [ ] Ansible (Node provisioning, cluster bootstrap)
- [ ] GitOps (FluxCD + Kustomize integration)

### Phase 2: Networking & Ingress
- [ ] Traefik (Ingress Controller)
- [ ] Cert-Manager (TLS automation)
- [ ] AdGuard or Pi-hole (DNS filtering)
- [ ] Gravity Sync (for DNS redundancy)
- [ ] Authelia or Keycloak (MFA, Identity Layer)

### Phase 3: Monitoring & Observability
- [ ] Prometheus + Grafana
- [ ] Loki + Fluent Bit
- [ ] Node Exporter + kube-state-metrics
- [ ] Suricata / Snort / Falco (Security Monitoring)

### Phase 4: Core Services
- [ ] GitLab (CI, DevOps)
- [ ] JupyterLab (Notebook environment)
- [ ] Matrix + Element (Chat system)
- [ ] Jellyfin / MangaDB / Minecraft / TF2
- [ ] Personal Search Engine (Whoogle / Aleph)
- [ ] Wiki (XOWA / Wiki.js)

### Phase 5: AI & Data Processing
- [ ] Deploy Ollama or LocalAI
- [ ] Test model inference on Odin and Pi 5s
- [ ] Mount object storage (MinIO) and shared datasets

---

## 📦 Storage Strategy
- MinIO for object storage (S3)
- Longhorn for block storage (stateful apps)
- TrueNAS for shared NFS/iSCSI/SMB (cold storage, backups)

---

## 🔁 Backup Strategy
- Velero for cluster resource snapshots
- Restic/Borg for app-level and data backups
- Longhorn snapshot scheduling
- Offsite cold storage (Dell server or HDD mirror)

---

## 🧠 Notes
- Entire system orchestrated using GitOps via FluxCD
- Focused on YAML-defined infrastructure (declarative, reproducible)
- Stateless-first services; stateful workloads live on Odin and Sindri
