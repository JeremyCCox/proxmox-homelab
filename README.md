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
