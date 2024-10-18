

A **High-Performance Computing (HPC) cluster** consists of multiple high-speed computer servers networked together via a fast interconnect. This architecture allows for parallel computing workloads managed by a centralized scheduler. The typical components of an HPC cluster include:

## Components of an HPC Cluster

1. **Master Node (or Login Node)**:
   - The entry point for users to log in and submit jobs. Users interact with the master node to initiate and manage their computational tasks.

2. **Scheduler**:
   - Responsible for managing the distribution of computational tasks across the available resources, such as nodes, GPUs, and storage, upon job submission. Common examples of schedulers used in HPC environments include:
     - **SLURM**
     - **PBS (Portable Batch System)**
     - **LSF (Load Sharing Facility)**

3. **Compute Nodes**:
   - All computational tasks are executed on these nodes. Each node is equipped with:
     - Processors (CPUs or GPUs)
     - Local memory (RAM)
     - Storage (SSD/HDD)
   - The nodes work collaboratively to perform the computations required for job completion.

4. **Storage System**:
   - HPC clusters often include a dedicated physical storage system designed to handle the large volumes of data generated or required during computations. Storage options can vary, including:
     - **Parallel File Systems**: Allow multiple nodes to access data concurrently.
     - **Storage Area Networks (SAN)**: A dedicated high-speed network that provides access to consolidated block-level storage.
     - **Network-Attached Storage (NAS)**: Typically used for simpler file storage needs. Data is stored on a dedicated server or device connected to the network and accessed using standard network protocols such as:
       - **Network File System (NFS)**
       - **Server Message Block (SMB)**

5. **Network**:
   - The cluster is connected through a high-speed interconnect, enabling efficient data transfer between nodes during computations. **InfiniBand** is a common high-speed network standard used in many HPC clusters. An **InfiniBand switch** manages the routing and switching of data packets, facilitating high-speed communication across the nodes.

# Kuria Cluster

Kuria Cluster is a high-performance computing cluster with a capacity of 40 teraFLOPS, hosted in the [Department of Physics, CUSAT](https://physics.cusat.ac.in/).

<figure style="text-align: center;">
    <img src="./_resources/schematics_of_kuria_cluster.png"
         alt="CLI" style="width:35%" >
    <figcaption>  Figure: Schematics of Kuria Cluster
</figcaption></figure>



## Resource Specifications

The resource specifications of the Kuria Cluster are as follows:

| Resource                                                                                           | Details                                               |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------|
| **PowerEdge R6625 Compute Nodes**                                                                  | 10 (960 CPU cores)                                   |
| **Compute Node Memory (Total)**                                                                    | 2560 GB (256 GB x 10)                               |
| **PowerEdge R7625 GPU Nodes**                                                                      | 1 (96 CPU cores)                                     |
| **A30 - 24GB Tensor Core GPUs**                                                                     | 2                                                    |
| **Total GPU Memory**                                                                                | 256 GB                                              |
| **Dell EMC S5212F-ON Switch with 600 Gb/s Aggregate Switch Throughput**                          | 15 ports (12 x 25GbE SFP28 + 3 x 100GbE QSFP28)     |
| **Dell EMC S5212F-ON Switch (Secondary Interconnect Switch – Standby)**                           | 15 ports (12 x 25GbE SFP28 + 3 x 100GbE QSFP28)     |
| **NFS-Based Storage (Dell ME5012)**                                                                | 123.84 TB (10 x 12TB NLSAS + 2 x 1.92TB SSD)         |

<div align="center">  

### Virtual Machine Nodes
</div>

| Component                     | Details                                                              |
|-------------------------------|----------------------------------------------------------------------|
| **Total VM Nodes**            | 3                                                                    |
| **Test Nodes (2 nodes)**      | 32 CPU cores (total)                                               |
| **Data Transfer Node (1 node)**| 8 CPU cores                                                         |
| **Aggregate CPU Cores from VMs**| 40 CPU cores                                                       |

<div align="center">  

### Compute Node Resources
</div>

| Component                  | Specification                                                                              |
|----------------------------|--------------------------------------------------------------------------------------------|
| **CPU**                    | AMD EPYC 9254                                                                              |
| **CPU Cores**              | 48 Cores (Dual Socket, each with 24 Cores) [96 cores with Hyper-threading]                |
| **L3 Cache**               | 128 MB                                                                                     |
| **System Memory (RAM)**    | 256 GB                                                                                     |
| **Local Storage**          | Diskless Configuration                                                                     |
| **GPU**                    | No                                                                                         |
| **GPU Memory**             | Nil                                                                                        |
| **Total No. of GPUs per node** | 0                                                                                      |
| **Network**                | Broadcom 57414 Dual Port 10/25GbE & Broadcom 5720 Dual Port 1GbE (additional network card) |

<div align="center">  

### GPU Node Resources
</div>

| Component                  | Specification                                                              |
|----------------------------|----------------------------------------------------------------------------|
| **CPU**                    | AMD EPYC 9254                                                              |
| **CPU Cores**              | 48 Cores (Dual Socket, each with 24 Cores) [96 cores with Hyper-threading] |
| **L3 Cache**               | 128 MB                                                                     |
| **System Memory (RAM)**    | 256 GB                                                                      |
| **Local Storage**          | 480 GB SSD                                                                 |
| **GPU**                    | NVIDIA A30 - PCIe                                                          |
| **GPU Memory**             | 24 GB                                                                      |
| **Total No. of GPUs per node** | 2                                                                          |
| **Network**                | Broadcom 57414 Dual Port 10/25GbE                                          |

## Software Stack of Kuria Cluster

<figure style="text-align: center;">
    <img src="./_resources/software%20stack.png"
         alt="CLI" style="width:100%" >
    <figcaption>  Figure: Software Stack of Kuria Cluster
</figcaption></figure>
