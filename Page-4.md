# SLURM Basics

Simple Linux Utility for Resource Management (SLURM) is an open-source job scheduler designed to manage and allocate resources in HPC clusters. It helps distribute computational workloads efficiently across nodes in a cluster by managing job queuing, resource allocation, and job execution.

SLURM is composed of 4 components:
1. **slurmctld** (Controller Daemon) - Execute on cluster's master node to manage workload. It controls the overall operations of SLURM.
2. **slurmd** (Node Daemon) - Worker Daemon that runs on each compute node in the cluster. Responsible for launching jobs on the node, communicating with a central controller, job execution and resource allocation.
3. **slurmbd** (Database Daemon) - Manage storage and retrieval of information related to user accounts during job submissions, user and group usage and historical job data.
4. **User commands** - Commands that users and administrators use to submit, manage and monitor jobs, as well as configure the cluster.
<figure style="display: flex; flex-direction: column; align-items: center;">
    <img src="./images/slurm.gif"
         alt="CLI" style="width:60%" >
    <figcaption> 
 Figure: Command Line Interface 
(<a href="https://slurm.schedmd.com/quickstart.html" target="_blank">source</a>)
</figcaption></figure>

## User Commands

Users can interact with SLURM using following commands:

- `sbatch` -  To submit a job to the queue.
- `srun` - To launch tasks in parallel.
- `salloc` - To obtain a job allocation and to use the cluster in an interactive manner.
- `scancel` - To cancel a Job.
- `squeue` - To check the status of jobs in the queue.
- `sinfo` - To view information about the nodes and partitions.

Another important SLURM command is `scontrol`, which is primarily used for administrative tasks. It allows administrators to manage and configure jobs, nodes, partitions, and SLURM daemons. Through scontrol, administrators can modify job parameters, change node states, and query system configurations. More details about the important SLURM commands are at the [official slurm website](https://slurm.schedmd.com/pdfs/summary.pdf).

## SLURM Partitions & User Types
SLURM Partitions are the logical grouping of computing nodes that allows for different access policies, resource limits & priority settings. Each partition is configured with specific node access and time limits, enabling efficient resource management. Below is the summary of the SLURM partitons available in the DOP-HPC cluster:

| Name of Partition              | Time Limit | Node Access |
|--------------------------------|------------|-------------|
| normal                         | 1 day      | Nodes 01-11 |
| gpu                            | infinite   | Node 11     |
| test                           | 1 hr       | Node 12-13  |
| data  (**available at request**) | infinite   | Node 14     |
| faculty                        | 14 days    | Nodes 01-11 |
| phd                            | 7 days     | Nodes 01-11 |

There are three types of users in the DOP-HPC Cluster: Faculty User, PhD User and Student User. Each user type has access to different SLURM partitions and specific resource limitations. 

| User-type |     Partitions available     | Storage Limit |
|:---------:|:----------------------------:|:-------------:|
|  Faculty  | normal, gpu, test, faculty   |     500 GB    |
|    PhD    |    normal, gpu, test, phd    |     300 GB    |
|  Student  |       normal, gpu, test      |     200 GB    |

Faculty users have the broadest access, including partitions like faculty with extended time limits and larger storage. PhD users have access to the phd partition, while students are limited to standard partitions like normal, gpu, and test.  Each user type also has a defined storage limit, ensuring fair resource allocation across the system. Further, users are grouped based on their affiliation with the research groups. Each research group is assigned three user IDs, corresponding to one from each user type: Faculty, PhD, and Student. This ensures that the research group has access to resources and partitions according to the role and level of the user within the group.
