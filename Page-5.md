# Submitting a Job to the Cluster

When using a cluster, jobs can be submitted in two main ways: as batch jobs or interactive jobs. In the following sections, we are going to discuss about both modes, their differences, and how to submit jobs effectively.

### Batch Job
A batch job allows you to submit a job script that will run without direct user interaction. You specify the required resources (like CPU, memory, and time), and the job is queued until those resources become available. Steps to perform a batch job are as follows:

1. #### Write a SLURM Job Script

A job script is a shell script that includes both SLURM directives and the commands you want to execute. Below is a basic example of submitting a python code as a batch job.
```
!/bin/bash
#SBATCH --job-name=batch_job        # Job name
#SBATCH --output=output_%j.log      # Standard output and error log (%j is the job ID)
#SBATCH --error=error_%j.log        # Create a log specifically to display error in running jobs
#SBATCH --ntasks=1                  # Number of tasks (1 task = 1 process)
#SBATCH --cpus-per-task=4           # Number of CPU cores per task
#SBATCH --mem=8G                    # Memory per node -- optional
#SBATCH --time=02:00:00             # Time limit (hh:mm:ss) -- optional
#SBATCH --partition=normal          # Partition (queue) to submit the job

# Load necessary modules
module load Languages/python/3.11.2

# Run the application or script
python my_script.py
```

The above job script instructs SLURM to run the Python program `myscript.py` as a single task utilizing 4 CPU cores. The job is named `batch_job`, and the output and error logs will be generated in the directory where `myscript.py` is located. These logs will be saved as `output_<job_id>.log` and `error_<job_id>.log`, where `<job_id>` is the unique identifier assigned to the job.

More details about SLURM Directives and Environment variables that can be used in SLURM script is available at their [documentation](https://slurm.schedmd.com/sbatch.html).

2. #### Submiting a job to a cluster

Once you’ve written your job script, use the sbatch command to submit it to the cluster.

```
sbatch my_job_script.sh
```
SLURM will queue the job and execute it when resources become available.

3.  #### Monitor the Job

You can monitor the job’s progress using the squeue command:
```
squeue -u your_username
```

**Advantages of using Batch Jobs:**
- *Automated*: Once submitted, the job runs without further interaction.
- *Scheduled*: Jobs wait in a queue until the cluster allocates the requested resources.
- *Ideal for long runs*: Jobs that take hours or days can be easily handled in batch mode.


> **SLURM JOB ARRAY**
> A job array allows you to submit a large number of jobs as a single entity, simplifying the submission and management process. Each job in the array can be accessed using an index (e.g., `job_array[0]`, `job_array[1]`, etc.).
> Example of submitting a Job Array:  ` sbatch --array=0-9 my_script.sh`

### Interactive Job

An interactive job allows you to work directly on the compute nodes of a cluster in real time. This is useful for testing code, debugging, or running jobs that require user input or observation. While you can use both `salloc` and `srun` to run interactive jobs, salloc is generally the preferred method, as it provides more flexibility and control over resource allocation.

#### Using `salloc` for interactive Jobs

The `salloc` command is used to request resources from SLURM and grants you an interactive shell on the allocated compute node. From this shell, you can run your tasks manually, providing a real-time environment for experimentation or debugging.

Example of using `salloc` in interactive jobs:

```
salloc --ntasks=1 --cpus-per-task=4 --mem=8G --time=02:00:00 --partition=normal
```

The above block of code requests for **4 CPU cores** and **8 GB of memory** and **2 hours of time** from any node in normal partition **to run a single task**.  

Once the resources are allocated, you will be placed in an interactive shell on the allocated node. From here, you can load any required modules, execute commands, or run scripts.

More options of `salloc` command can be seen in their [documentation](https://slurm.schedmd.com/salloc.html).

#### Using ```srun``` for Interactive Jobs

The srun command is commonly used for running individual tasks within a job (allocated using ```sbatch``` and ```salloc```). But using a Psuedo-Terminal, `srun` command can also be used run jobs interactively.

Example of using `srun` in interactive jobs:

```
srun --ntasks=1 --cpus-per-task=4 --mem=8G --time=02:00:00 --partition=general --pty bash
```

- ```--pty```: This tells srun to open a pseudo-terminal, allowing you to run an interactive session on the allocated node.
- ```bash```: This runs an interactive shell (you can also use other shells like ```sh``` or ```zsh```).

> **NOTE** 
> One can also use srun command without --pty option to directly run commands in the compute node
> ```srun --ntasks=1 --cpus-per-task=4 --mem=8G --time=02:00:00 --partition=general python3 my_script.py```
> 
> In this approach job scheduled will not have interactive nature rather resembles to a batch job.

More options of `srun` command can be seen in their [documentation](https://slurm.schedmd.com/srun.html).