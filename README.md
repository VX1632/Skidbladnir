# Skidbladnir

**Skidbladnir** is a modular Kubernetes deployment framework built for **resilience** and **adaptability**.  
Inspired by Norse mythology’s legendary foldable ship, it delivers infrastructure that **travels light but carries heavy** — optimized for both agility and power.  

---

## Mission Statement

This project explores the design and operation of a **high-availability Kubernetes microservices cluster** built on **heterogeneous hardware** (x86_64 and ARM64), with planned expansion to Apple silicon.  

The goals are:  
- To create a **learning platform** for secure cloud- and edge-oriented infrastructure.  
- To provide a **practical reference** for others building MicroK8s clusters across mixed architectures.  

---

## Purpose and Objectives

- **Multi-Architecture Enablement**  
  Develop and document strategies for running MicroK8s on a cluster that combines x86_64 and ARM64 nodes. Address unique challenges such as provisioning, distributed backups, and container image compatibility.  

- **AI/ML Readiness**  
  Build a stable and efficient foundation for running AI/ML workloads, with careful attention to Ray-on-Kubernetes deployment, resource allocation, and jemalloc page-size limitations.  

- **Infrastructure as Code**  
  Use Ansible, Helm, and Kustomize to provision, configure, and manage the cluster reproducibly. Maintain version-controlled workflows for lifecycle management.  

- **Networking and Security**  
  Integrate HTTPS, DNS, and firewall controls into the cluster fabric. Explore traffic management, monitoring, logging, and security tooling to balance **protection** with **performance**.  

- **Knowledge Sharing**  
  Document lessons learned — including missteps, workarounds, and iterative improvements — so others can replicate, adapt, or extend this work.  

---

## Why This Project Matters

Building secure, resilient, and flexible clusters requires more than simply “getting Kubernetes running.” By experimenting with:  

- **Multi-architecture deployments**  
- **Edge/cloud design patterns**  
- **Defensive security practices**  

This project highlights both the **opportunities and pitfalls** faced by:  
- Builders and hobbyists experimenting with Kubernetes at home  
- System maintainers responsible for multi-node, multi-arch infrastructure  
- Cybersecurity professionals implementing defenses in cloud-native environments  

The long-term vision is to establish a **robust platform for AI/ML research** while also serving as a **public reference architecture** for secure, real-world Kubernetes deployments.  

---

## About Naglfar

**Note:** Documentation for **Naglfar** — the hardened, autonomous Kubernetes cluster paired with Skidbladnir — is **unreleased** and not yet available for public review.  

When published, Naglfar will showcase advanced features such as:  
- Zero-trust network segmentation   

Until then, Skidbladnir remains the **primary public-facing framework**.  

---

## Project Notes

- Built and maintained as part of a **self-hosted homelab**, simulating production-grade orchestration, networking, and security workflows.  
- Originally deployed in a **hybrid cluster** using Raspberry Pi 5 (ARM64) and x86 infrastructure.  
- Provisioning strategies are documented for **multi-arch image builds** and **distributed backup systems**.  

[View the full architecture diagram with codenames](diagram/skidbladnir-architecture.md)  Diagram is not final, I am still reworking the underlying pipelines and services.

---

## Tags

`#homelab` `#kubernetes` `#raspberrypi` `#infrastructure-as-code`  
