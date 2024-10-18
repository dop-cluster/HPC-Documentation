# Parallel Programming in HPC clusters
In the realm of high-performance computing (HPC), the efficient utilization of multi-core processors is essential for accelerating computational tasks and optimizing resource usage. As scientific and engineering applications increasingly demand greater processing power, parallel programming emerges as a critical solution to enhance performance and reduce runtime. By leveraging threading and message passing, developers can distribute workloads across multiple CPU cores or nodes, enabling simultaneous execution of tasks. This approach not only improves computational efficiency but also facilitates the handling of complex problems that would otherwise be intractable in a sequential environment. This document outlines the techniques for submitting threaded jobs using Python's multiprocessing module and implementing distributed computing with the Message Passing Interface (MPI) using mpi4py, providing practical examples and SLURM job submission scripts to help you effectively run parallel jobs on HPC clusters.

## Submitting Threaded Jobs to the Cluster

<!-- In high-performance computing (HPC), taking advantage of multi-core processors is essential for efficient parallel execution of programs. Many scientific and computational workloads can benefit from running multiple tasks concurrently using threading. When submitting a threaded job, the goal is to distribute the workload across the cores available on a node, making the program more efficient by reducing runtime.

One common way to achieve parallelism in Python is through the `multiprocessing` module, which enables parallel execution of functions across multiple CPU cores. This is useful for CPU-bound tasks, where you want to divide work and compute results simultaneously across different cores of the same node. 
Let’s consider an example where we want to submit a Python script that tries to find the Fibonacci series, and it is using `multiprocessing` module to speed up the computation.-->

In high-performance computing (HPC), efficiently utilizing multi-core processors is crucial for reducing runtime and improving the performance of scientific and computational workloads. Many tasks, such as simulations, data processing, and mathematical computations, can benefit from running multiple tasks concurrently using threading. The goal of submitting a threaded job is to distribute the workload across the cores available on a node, ensuring better resource utilization and faster execution.

Python offers several ways to implement parallelism, one of which is the multiprocessing module. This module allows parallel execution of functions across multiple CPU cores, making it particularly useful for CPU-bound tasks where computations need to be divided across different cores to achieve higher efficiency.

### Example: Using Python’s multiprocessing Module for Fibonacci Calculation
As an example, let's consider a Python script that computes Fibonacci numbers using the multiprocessing module to speed up the computation. The script creates multiple processes to parallelize the work across available CPU cores.


```
# Python script to find Fibonacci numbers from 0 to 40.

import multiprocessing

def fib(n): # function to find fibbonaci numbers
    return n if n < 2 else fib(n - 2) + fib(n - 1)

if __name__ == "__main__":
    with multiprocessing.Pool(processes=4) as pool: # creates 4 process
        results = pool.map(fib, range(40))
        for i, result in enumerate(results):
            print(f"fib({i}) = {result}")		
```

<!-- The Python script is saved as `job.py`. Now, you'll need to create a job submission script (slurm script) that requests the necessary resources and runs the `job.py` script. Let us create a job submission script named `slurm_script.sh`. -->

The script calculates upto 35th Fibonacci number using multiple processes. It determines the number of available CPU cores and creates that many parallel processes, each performing the same Fibonacci calculation.

The Python script is saved as `job.py`. Next, you need to create a job submission script (`slurm_script.sh`) to request the necessary resources and submit the job to the cluster using a job scheduler, such as SLURM.

### SLURM Job Submission Script
Here is the SLURM submission script that requests resources and runs the `job.py` script:

```
#!/bin/bash
#SBATCH --job-name=parallel_job       # Job name
#SBATCH --output=parallel_job_%j.out  # Output and error log
#SBATCH --error=parallel_job_%j.err   # Error log (optional)
#SBATCH --ntasks=1                    # Number of tasks (processes)
#SBATCH --cpus-per-task=4             # Number of CPU cores per task
#SBATCH --time=00:10:00               # Time limit (hh:mm:ss)(optional)
#SBATCH --partition=normal            # Partition name (adjust as needed)
#SBATCH --mem=4G                      # Memory per node (adjust as needed)

# Load necessary modules
module load Languages/python/3.12.1  # Adjust Python version as needed

# Activate virtual environment if needed
# source /path/to/venv/bin/activate

# Run the Python script using srun
srun python3 job.py
```
In this script:

`--ntasks=1` : Specifies that only one task (one job) will be run.
`--cpus-per-task=4` : Requests 4 CPU cores for the task.
`--time=00:10:00` : Sets a time limit of 10 minutes for the job (optional).
`--partition=normal` : Specifies the partition to which the job should be submitted.
`--mem=4G` : Allocates 4GB of memory for the job.
`module load Languages/python/3.12.1` : Loads Python version 3.12.1 on the cluster (adjust based on your cluster's configuration).
`srun python3 job.py` : Executes the job.py Python script using `srun`. Here `srun` is used to launch tasks parallely.

### Submitting the Job
Once the SLURM script is ready, you can submit the job to the SLURM scheduler with the following command:

```
sbatch slurm_script.sh
```

This command submits the job to the cluster, and SLURM will schedule it for execution based on the available resources. The output of the job will be stored in the file `parallel_job_<job_id>.out`, where `<job_id>` is the job identifier assigned by SLURM.

## Submitting MPI Jobs to the Cluster

In high-performance computing (HPC), Message Passing Interface (MPI) is widely used to enable communication between multiple processes running on different nodes or cores. MPI is particularly useful for large-scale distributed computing tasks where processes need to exchange data frequently. Python’s mpi4py library provides bindings for MPI, allowing you to write parallel programs in Python that can run on multiple nodes and communicate with each other using MPI primitives.

To demonstrate how to use MPI (Message Passing Interface) with Python using the mpi4py module to compute the Fibonacci series, we will break down the task into smaller pieces that can be executed in parallel across multiple processes.

### Example: Using `mpi4py` for Fibonacci Numbers

One approach to parallelize this problem is to distribute the computation of different Fibonacci numbers to different MPI processes. Each process can compute a specific Fibonacci number, and then the results can be gathered. While this won't speed up the computation of a single Fibonacci number, it can compute multiple Fibonacci numbers in parallel, which might be useful if you need a series of Fibonacci numbers.

Here’s an example of how you could use `mpi4py` to compute different Fibonacci numbers in parallel:

```
# Python script to calculate Fibonacci numbers using mpi4py

from mpi4py import MPI

def fib(n):
    """Function to compute Fibonacci numbers"""
    return n if n < 2 else fib(n - 2) + fib(n - 1)

# Initialize MPI
comm = MPI.COMM_WORLD
rank = comm.Get_rank()  # Get the rank of the process
size = comm.Get_size()  # Get the number of processes

# Define the range of Fibonacci numbers to compute (let's say from 0 to 40)
fib_numbers = list(range(40))

# Scatter work to different processes (each process gets a subset of Fibonacci numbers to compute)
chunk_size = len(fib_numbers) // size
start = rank * chunk_size
end = start + chunk_size if rank != size - 1 else len(fib_numbers)  # Last process may get more work
work = fib_numbers[start:end]

# Each process computes the Fibonacci numbers assigned to it
results = {n: fib(n) for n in work}
print(f"Process {rank} computed Fibonacci numbers: {results}")

# Gather all results to the root process (rank 0)
all_results = comm.gather(results, root=0)

# Root process consolidates and prints the final result
if rank == 0:
    fib_dict = {}
    for result in all_results:
        fib_dict.update(result)
    print(f"Final Fibonacci numbers: {fib_dict}")
```

In this example, each MPI process is responsible for calculating a subset of Fibonacci numbers. The `fib_numbers` array contains the Fibonacci indices from 0 to 40, which need to be computed. To distribute the work evenly, the total number of Fibonacci numbers is divided by the number of available processes using `chunk_size`. Each MPI process gets a specific range of Fibonacci indices based on its rank and calculates the corresponding Fibonacci numbers for that range. After each process completes its calculations, the results are sent back to the root process (rank 0) using `comm.gather()`. The root process gathers the results from all processes, combines the calculated Fibonacci numbers, and prints the final set of Fibonacci numbers. This method efficiently distributes computational tasks and allows for parallel execution. The script can be saved as `mpi_fib.py`.

### SLURM Job Submission Script

To run this MPI-based Fibonacci calculation on the cluster, you’ll need a SLURM submission script (`slurm_mpi_fib.sh`):

```bash
#!/bin/bash
#SBATCH --job-name=mpi_fibonacci         # Job name
#SBATCH --output=mpi_fibonacci_%j.out    # Output and error log
#SBATCH --ntasks=4                       # Number of tasks (processes)
#SBATCH --cpus-per-task=1                # Number of CPU cores per task
#SBATCH --time=00:10:00                  # Time limit (hh:mm:ss)
#SBATCH --partition=normal               # Partition name
#SBATCH --mem=2G                         # Memory per node

# Load necessary modules
module load Languages/python/3.12.1       # Adjust Python version as needed
module load MPI/OpenMPI/4.1.1             # Load MPI module (adjust version as needed)

# Run the Python script using mpirun
mpirun -np 4 python3 mpi_fib.py
```
In this script:

- `--ntasks=4`: Specifies that the job will run 4 MPI tasks (processes).
- `--cpus-per-task=1`: Each task uses 1 CPU core.
- `mpirun -np 4 python3 mpi_fib.py`: Runs the `mpi_fib.py` script with 4 MPI processes.

### Submitting the Job
Submit the job to SLURM using the following command:
```
sbatch slurm_mpi_fib.sh
```

This will distribute the calculation of the Fibonacci numbers across the 4 processes. Each process computes a subset of the numbers in parallel, and the results are gathered at the root process.

> **NOTE** 
> Fibonacci is a recursive problem, and not all parts of the calculation can be easily parallelized. Thus, using MPI to run such problem leads to decline in performance because of large communication overhead of MPI programs.


## Difference Between `srun` and `mpirun`

Both `srun` and `mpirun` are used to run parallel jobs, but they serve slightly different purposes, especially in the context of SLURM and MPI environments. Here's a breakdown of their differences:

###  srun (SLURM run)

- *Primary Use* : `srun` is the native command for running parallel jobs in the SLURM workload manager. It is generally used to execute tasks in parallel within a SLURM job allocation.
- *Integration* : It works seamlessly with SLURM, automatically inheriting all resource allocations (like number of nodes, tasks, memory, etc.) specified in the SLURM job script.
- *Task Launching* : `srun` can be used for both MPI and non-MPI jobs. When running an MPI job, it acts similarly to `mpirun` by launching tasks across nodes.
- *SLURM Aware* : Since `srun` is a SLURM tool, it communicates directly with the SLURM controller. It handles resource management, task distribution, and output collection across multiple nodes automatically. Thus  When you use `srun` within your `sbatch` script, it will automatically launch the number of tasks specified by `--ntasks`.

###  mpirun (MPI run)

- *Primary Use* : `mpirun` is the command used to launch MPI jobs across multiple processes. It comes from the MPI implementation (like OpenMPI or MPICH) and is specifically designed to run MPI applications.
- *MPI-Specific* : Unlike `srun`, `mpirun` is specific to MPI applications. It initializes the MPI environment, coordinates communication between processes, and handles MPI task launching.
- *External to SLURM* : `mpirun` does not have direct knowledge of SLURM. It needs to be explicitly told which resources (e.g., number of nodes, number of tasks) are available. It works independently of the SLURM scheduler and requires the user to manually manage resource allocation when run within a SLURM job.
- *Compatibility* : In many SLURM configurations, `mpirun` can be used within a job allocation (after using `sbatch` or `salloc`). 
<!-- However, in some setups, it might conflict with SLURM’s resource management, which is why `srun` is often preferred. -->



#### References to do parallel programming in other lanuages
- [OpenMP and MPI programming for C/C++](https://compphysics.github.io/ComputationalPhysics2/doc/LectureNotes/_build/html/parallelization.html#)
- [OpenMP and MPI programming for Fortran](https://hpc-wiki.info/mediawiki/hpc_images/a/a5/OpenMP_and_MPI_for_Dummies-Fortran.pdf)
