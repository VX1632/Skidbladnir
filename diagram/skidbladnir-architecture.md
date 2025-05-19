# Skidbladnir Public-Facing Architecture

**Codenamed Layers:**
- **Ghost Ship**: Hardened reverse proxy with TLS, ACL, and MFA handling
- **Paper Boat**: Ingress layer (e.g., NGINX, Traefik) routing traffic to internal services
- **Skidbladnir**: Modular Kubernetes layer exposed to the public
- **Naglfar**: (Not shown) Private backend, hardened and autonomous

---

## Diagram

```
             ┌──────────────────────┐
             │    Public Internet   │
             └────────┬─────────────┘
                      │
                      ▼
          ┌──────────────────────────┐
          │     "Ghost Ship"         │   ◀─ (Reverse Proxy)
          │  - TLS Termination       │
          │  - IP Filtering / ACLs   │
          │  - Rate Limiting         │
          │  - OAuth / MFA Gateway   │
          └────────┬─────────────────┘
                   │
          ┌────────▼────────┐
          │   "Paper Boat"  │   ◀─ Ingress Layer (NGINX / Traefik)
          │   Service Router│
          └────────┬────────┘
                   │
 ┌─────────────────┼─────────────────────┐
 ▼                 ▼                     ▼
┌────────────┐ ┌────────────┐ ┌────────────┐
│ Web Front  │ │  API Layer │ │ GitOps / CI│
│ (React/UI) │ │ (REST/gRPC)│ │ Controller │
└────────────┘ └────┬───────┘ └────────────┘
                    │               
                    ▼               
          ┌────────────┐
          │   DB Pod   │
          │ (Postgres) │
          └────────────┘
```

## Notes

- This diagram represents a simplified, public-facing, modular portion of the overall architecture.
- Real networking details, IP addresses, hostnames, and internal secrets are intentionally abstracted and will vary based on client needs and deployment context.
- The system is designed and deployed in a hybrid homelab environment using Raspberry Pi 5 and x86 infrastructure.
- It simulates production-grade orchestration, networking, and service behavior in low-trust or adversarial environments.

---

## Keywords

#homelab, #kubernetes, #zero_trust, #reverse_proxy, #ingress, #raspberry_pi, #architecture, #container_orchestration, #infrastructure_as_code, #iaas
