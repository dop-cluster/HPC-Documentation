# Parallel Programming 101

Parallel programming involves dividing a computational task into smaller sub-tasks that can be executed simultaneously, thereby reducing the overall execution time. This approach is particularly beneficial in HPC environments, where large-scale computations are common.

There are mainly two models of parallel computing:

(1) *Shared Memory Parallelism* -
A model of parallel computing where computational units communicates with each other through same memory space. The advantage is that communication between the subunits does need to be take cared explicitly as they read and write from shared memory. But race conditions can arise when two subunits try to access same resources and such situations should be avoided using synchronization techniques. 

Threads are one-way to achieve shared memory parallelism. In simple words, thread represent a sequence of instructions that cpus can execute independently. A thread have its own program counter that keeps track of instructions to be executed, system register to hold current working variables, and stack containing the execution history. A thread shares with its peer threads few information like code segment, data segment and open files. When one thread alters a code segment memory item, all other threads see that.

Common Libraries that supports multithreading are: 
* **OpenMP** -  Used in C/C++ and Fortran.
* **Threading** module in Pthon.
*  **Multiprocessing** module in Python.
<figure style="display: flex; flex-direction: column; align-items: center;">
    <img src="./images/concept_of_threads.png"
         alt="slurm components" style="width:80%" >
    <figcaption>  Figure: Concept of threads
		<a href="https://www.geeksforgeeks.org/multithreading-python-set-1/" target="_blank"> (source) </a> 
</figcaption></figure>




(2) *Distributed Memory Parallelism* -  Mode of parallelism where each processor or computational unit has its own private memory, and data is exchanged between units via a communication mechanism (e.g., message passing). In contrast, in shared memory parallelism computational units share common memory space.

Distributed memory parallelism is achieved by means of process. A process is an independent instance of a program in execution. It includes the program's code, its own memory space, and system resources, such as CPU time and I/O access. A process operates in isolation from other processes, meaning it has its own address space (memory) and cannot each other's memory directly.  Message Passing Interface (MPI) is a specialized and widely used framework for parallel computing in distributed memory systems, enabling processes to efficiently communicate and synchronize across nodes.

> Intuitatively, a process is like a full program running in its own memory space, a thread is more like a smaller task or part of that program

Common libraries that support MPI programming:
* **OPENMPI** - Supports C,C++ and Fortran.
*  **MPICH** - Supports C,C++ and Fortran.
*  **mpi4py** - Library that provides bindings for MPI in Python. It allows programs to ultilize OPENMPI and MPICH functionalities.


## Global Interpreter Lock (GIL) in Python

The Global Interpreter Lock (GIL) is a mechanism in Python that limits its interperter to use only single thread at a time during execution of Python bytecode, even in multithreaded programs. As a result, Python's multithreading is not ideal for CPU-bound tasks, which require significant processing power, since the GIL effectively serializes the execution of threads, turning the program into a single-threaded one. However, multithreading in Python can still be useful for I/O-bound tasks, such as reading from files or making network requests, where the CPU spends time waiting for external resources. Hence, using the `threading` module in Python would not guarntee improved performance in all tasks.

On the other hand modules like `multiprocessing` in python provides ways to bypass GIL, by using process instead of thread. Thus, a task is parallelised using child processes, each with their own memory space and resource that they do not share with other processes. Each process communicate via Inter-Process Communication (IPC), which is much more genralized scheme than MPI for communicating between process. MPI is designed particularly for dist
ributed memory parallelism, i.e, to run programs in multiple nodes whereas IPC can be used both for communication within a single node or across multiple nodes. Further, they way processes communicate in both mechanism is fundamentally different. There are other modules in python such as `cython`, `numba` etc that also bypasses GIL and a detailed tutorial on using these modules to obtain shared memory parallelism is available [here](https://realpython.com/python-parallel-processing/#make-python-threads-run-in-parallel).
