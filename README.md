# Proxmox Homelab



## Proxmox Server Stack Architecture

```mermaid
graph TD
    Proxmox["Proxmox VE Host"]

    %% PCIe Passthrough Guests
    subgraph PCIe_Passthrough["PCIe Passthrough Guests"]
        Win11["Windows 11 VM (2070 Super GPU)"]
        Kubuntu["Kubuntu VM (2070 Super GPU)"]
    end


    %% GPU Allocation
    subgraph GPUs["GPU Allocation"]
        GPU1["1050 Ti"]
        GPU2["2070 Super"]
        Custodian["Dedicated Compute GPU"]
    end

    %% Headless Guests
    subgraph Headless["Headless Guests"]
        DockerHost["Ubuntu Docker Host (Services & Containers)"]
        IntelliJ["Remote IDE Service (Jetbrains Gateway, VS Code Server)"]
        WRAP["WRAP Server (Private Client service)"]
    end

    %% Connections

    GPU2 --> PCIe_Passthrough
    GPU1 --> Custodian

    Proxmox --> PCIe_Passthrough
    Proxmox --> Headless
```

## Infrastructure Roadmap

## Infrastructure Roadmap Timeline

```mermaid
timeline
    title Proxmox Server Stack Roadmap

    "Baseline Setup" : "Proxmox VE host with PCIe passthrough guests (Windows 11, Kubuntu) and headless guests (Docker Host, Remote Dev VM, ETC...)"
    "Monitoring Layer" : "Add Prometheus + Grafana for metrics and basic alerting"
    "Planned NAS Storage" : "Deploy NAS appliance for shared storage access across VMs and containers"
    "Infrastructure as Code" : "Use Terraform to declaratively define VMs in Proxmox"
    "Container Orchestration" : "Optionally deploy Kubernetes cluster for container orchestration"
    "Expansion / Experiments" : "GPU-accelerated workloads on 1050 Ti, ML/AI experiments, optional Ceph/ZFS, improve security/backups"
