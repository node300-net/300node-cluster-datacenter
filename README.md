#!/usr/bin/env bash
set -euo pipefail

PROJECT_DIR="${1:-$HOME/node300-cluster-datacenter}"
REPO_URL="https://github.com/node300-net/300node-cluster-datacenter.git"

echo "Creating project directory: $PROJECT_DIR"
mkdir -p "$PROJECT_DIR"/docs "$PROJECT_DIR"/configs "$PROJECT_DIR"/ansible/group_vars "$PROJECT_DIR"/ansible/templates "$PROJECT_DIR"/ansible/roles/common/tasks "$PROJECT_DIR"/ansible/roles/proxmox/tasks "$PROJECT_DIR"/ansible/roles/zfs/tasks "$PROJECT_DIR"/ansible/roles/cluster/tasks "$PROJECT_DIR"/ansible/roles/pbs/tasks "$PROJECT_DIR"/playbooks "$PROJECT_DIR"/scripts "$PROJECT_DIR"/lab-notes

cd "$PROJECT_DIR"

cat > README.md <<'EOF'
# The Sovereign Datacenter
## Why Proxmox VE & Recycled Enterprise Hardware Obliterate Consumer Tech & The Cloud Trap

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Platform: Proxmox VE](https://img.shields.io/badge/Platform-Proxmox%20VE-blue.svg)](https://www.proxmox.com/)

> “Sovereignty is not an ideology. It is the practical ability to own your infrastructure, your data, your recovery path, and your failure modes.”

This repository is a practical, deeply technical reference for building a sovereign datacenter on Proxmox VE using recycled enterprise hardware. It combines architecture, storage design, network design, GPU passthrough, backup strategy, and security principles into a coherent model.

## Why Proxmox VE?
- Debian Linux base
- KVM full virtualization
- LXC lightweight containerization
- native ZFS integration
- Corosync + pmxcfs cluster control
- API-driven automation and IaC

## Why Enterprise Hardware?
- ECC memory
- hot-swap redundancy
- higher MTBF
- superior I/O
- lower TCO over time
- better performance per euro in real-world workloads

## Reference topology
```mermaid
flowchart TB
    subgraph Fabric["Cluster Fabric / Management"]
        Core["Huawei S220S-24LP4JX\n10GbE Core Interconnect"]
        Mgmt["Zyxel GS1900-24E v3\nMgmt / OOB"]
        FW["Firewalla Gold Pro\nPerimeter / Firewall"]
    end

    subgraph PVE["Proxmox VE Cluster"]
        N0["NODE 0\nSEC-CORE\nFirewall & VPC Gateway"]
        N1["NODE 1\nDEFENSE\nHoneypot & Deception"]
        N2["NODE 2\nANALYTICS\nML & Anomaly Engine"]
        N3["NODE 3\nMONITOR\nSuricata IDS / Logging"]
    end

    Internet --> FW
    FW --> Core
    Core --> N0
    Core --> N1
    Core --> N2
    Core --> N3

    Mgmt --> N0
    Mgmt --> N1
    Mgmt --> N2
    Mgmt --> N3# 300node-cluster-datacenter
