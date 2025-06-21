# Node Provisioning Ansible Bootstrap

This document outlines the public-facing Ansible automation process used to configure `k8s-worker`, a Kubernetes node in the MicroK8s high-availability (HA) cluster.

## Overview

This node is bootstrapped using a structured, role-based Ansible setup with focus on:

- Hardened SSH configuration
- Secure UFW rules
- Disabling swap for kubelet compatibility
- Installing MicroK8s 1.32 via Snap
- User and permission setup
- Optional cluster joining logic (as worker or control-plane)

## Inventory Configuration

`inventory.yaml` defines the node groupings and host parameters.

```yaml
all:
  hosts:
    k8s-worker:
      ansible_host: 10.10.10.10
      cluster_join_mode: worker
```
