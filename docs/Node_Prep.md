# 🛠️ Node Preparation Guide for MicroK8s Kubernetes Cluster

This guide covers the secure setup and configuration of Ubuntu-based nodes before joining them to a MicroK8s-managed Kubernetes cluster.

---

## 📋 Prerequisites

- Ubuntu 24.04+ LTS
- Static IP or DHCP reservation
- SSH access to the node
- Internet access for package updates

---

## 🔐 Secure Network Configuration (Before Init)

Ensure the following ports are open on your firewall/router **before initializing the cluster**:

| Port(s)                | Purpose                                                 |
|------------------------|---------------------------------------------------------|
| 2379-2380              | etcd client and peer communication                      |
| 10250                  | kubelet API                                             |
| 10255                  | Read-only kubelet port (optional, can disable)          |
| 10257-10259            | Control plane (controller-manager, scheduler)           |
| 6443                   | Kubernetes API server (required)                        |
| 179                    | BGP (if using Calico with BGP)                          |
| 4789/UDP or 51820/UDP  | Container networking (Calico VXLAN / WireGuard)         |

---

## 🔑 SSH Configuration (Safe Defaults)

Set up SSH securely on the node:

```bash
sudo apt update
sudo apt install openssh-server
sudo nano /etc/ssh/sshd_config
```

🚧 Initial sshd_config (Bootstrap Phase)

Use this temporary config to enable password authentication just long enough to install your public SSH key.

# /etc/ssh/sshd_config (initial)

```bash
Port <your-port>                # Use a non-standard port for obscurity
PermitRootLogin no              # Disable root login
PasswordAuthentication yes      # TEMPORARY: enable for key transfer
PubkeyAuthentication no         # Disabled during bootstrapping
AuthorizedKeysFile .ssh/authorized_keys
AllowUsers vxadmin              # Replace with your admin username
MaxSessions 2                   # Limit concurrent SSH sessions
MaxAuthTries 3                  # Reduce brute-force attack attempts
LoginGraceTime 30m              # Allow time for login (helps automation)
PermitEmptyPasswords no         # Disallow blank passwords
```
    ✅ After successful key deployment, switch to the hardened config below.

🛡️ Hardened sshd_config (Post-Key Setup)

Once your public key is confirmed working:

# /etc/ssh/sshd_config (hardened)

```bash
Port <your-port>                # Keep non-standard port
PermitRootLogin no              # Still block root login
PasswordAuthentication no       # Disable password-based login
PubkeyAuthentication yes        # Use key-based authentication only
AuthorizedKeysFile .ssh/authorized_keys
AllowUsers <username>           # Ensure only authorized users connect
MaxSessions 2                   # Maintain session limits
MaxAuthTries 3                  # Still resist brute-force
LoginGraceTime 30m              # Keep grace period (or shorten if desired)
PermitEmptyPasswords no         # Block empty passwords
```

🔁 Reload SSH Configuration

```bash
sudo systemctl reload ssh
```
Or:
```bash
sudo systemctl restart ssh
```

🔁 Post-Install Setup

```bash
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
newgrp microk8s
microk8s status --wait-ready
```
