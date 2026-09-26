---
plate: /assets/img/nitrile/code/baremetalvmm.webp
plate_alt: Pen and dot-screen drawing of an open rack chassis on a workbench, a row of identical sealed modules slotted inside and one pulled halfway out, a screwdriver beside it.
title: 'BareMetalVMM'
caption: Spin up Firecracker microVMs like containers, and keep the isolation of a VM
description: >
  A command-line tool for running lightweight Firecracker microVMs and Kubernetes clusters on bare-metal Linux hosts.
  VMs boot in seconds with their own kernel, can be built from any Docker image, and are reachable over SSH or a web console.
language: Go
licence: MIT
started: 2026
usage:
  - sudo vmm create myvm --cpus 2 --memory 1024
  - sudo vmm start myvm
  - vmm ssh myvm
links:
  - title: Documentation
    url: https://raesene.github.io/baremetalvmm/
  - title: Source on GitHub
    url: https://github.com/raesene/baremetalvmm
sitemap: false
---

I wanted something that feels like Docker for day-to-day test environments, but with more isolation than a container gives, and room for lower-level work that doesn't suit Docker well: custom kernels, kernel debugging builds, whole Kubernetes clusters.

## What it does

- Creates, starts, stops and snapshots Firecracker microVMs, each with a dedicated kernel.
- Builds a VM root filesystem from a Docker image, so a custom VM is a Dockerfile away.
- Brings up multi-node Kubernetes clusters with kubeadm and Cilium, or single-node MicroShift.
- Handles bridge networking, NAT, port forwarding and host directory mounts.
- Ships an optional web UI with a browser terminal and a JSON API.
- Supports security-research kernels, including KASAN builds, for vulnerability and exploit testing.

It is personal software, built largely with [Claude Code](https://github.com/anthropics/claude-code) and tested on Ubuntu 24.04 with KVM. It works for me; other environments may need some care.
