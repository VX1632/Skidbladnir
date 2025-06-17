# 🚀 GitOps, FluxCD, Kustomize, and Helm — Overview

## What is GitOps?
GitOps is the practice of using Git as the single source of truth for your infrastructure. It allows you to:
- Version control all deployments
- Automatically sync your cluster to Git
- Use pull requests to approve changes

## Key Components

### FluxCD
- Operator that watches your Git repository
- Applies changes to your Kubernetes cluster
- Works natively with Kustomize and Helm

### Kustomize
- Template-free way to customize Kubernetes YAML
- Useful for layering configurations (dev/staging/prod)
- Built into FluxCD

### Helm
- Package manager for Kubernetes (like apt for clusters)
- Supports complex apps (Grafana, GitLab, Loki, etc.)
- Helm Charts can be deployed via `HelmRelease` objects in Flux

## How They Work Together

```text
Git Repo
 ├── flux-system/
 ├── apps/
 │   └── traefik/
 │       ├── kustomization.yaml
 │       └── helmrelease.yaml
 ↓ Flux Watches
Flux Controller → Applies → K8s Cluster
