# Logging into Cluster

SSH, or Secure Shell, is a protocol that provides secure remote login from one computer to another. It offers a secure alternative to non-protected protocols like telnet and rlogin, and insecure file transfer methods such as FTP. With SSH, you can remotely run commands, manage files, and perform administrative tasks on a remote machine.

To connect to a remote server or cluster, you can use the following command:

```
# Connecting to cluster using SSH
ssh username@remote_server_ip
```

SSH also supports X11 forwarding, allowing you to run graphical applications remotely while displaying their graphical user interface (GUI) on your local machine. Here's how to enable X11 forwarding:


```
# Basic x11 forwarding
ssh -X username@remote_server_ip

# Trusted x11 forwarding
ssh -Y username@remote_server
```

# Sending Files to and From Cluster

### SCP (Secure Copy)

SCP command is used to transfer files between local host and remote host or betwen two remote hosts.  The general syntax for SCP is

```
scp [options] [source] [destination]
```

Some of the common use cases are:

```
# copy a file from your local system to a remote system
scp /path/to/local/file username@remote_host:/path/to/remote/directory

# copy a file from a remote system to your local system
scp username@remote_host:/path/to/remote/file /path/to/local/directory

# copy a directory recursively from your local system to a remote system
scp -r /path/to/local/directory username@remote_host:/path/to/remote/directory

# copy a file from one remote system to another remote system
scp username1@remote_host1:/path/to/remote/file username2@remote_host2:/path/to/remote/directory
```

### SFTP (Secure File Transfer Protocol)

SFTP is another method for securely transferring files. Its syntax is similar to the ssh command:
```
sftp username@remote_server
```

Once connected to the remote server, you'll be prompted to enter a password. After that, you can use various commands to navigate and transfer files. The commands useful during `sftp` session is given as follows;

```
# connect to the remote server
sftp user@192.XXX.XXX.XXX

# after login, list files on the remote server
sftp> ls

# change directory on remote server
sftp> cd /remote/directory

# change local directory
sftp> lcd /local/directory

# download a file from remote server to local machine
sftp> get remote_file.txt

# upload a file from local machine to remote server
sftp> put local_file.txt

# exit the SFTP session
sftp> bye
```

 Most systems have built-in SFTP support via the command line. You can also use graphical SFTP clients like **FileZilla** or **WinSCP**.
 
 # Environment Modules

Managing dependencies in a cluster environment is crucial for ensuring that applications run smoothly and consistently across various compute nodes. In Kuria cluster we uses environment modules to manage the dependencies.

**Environment Modules** is a tool used to dynamically modify the user environment in a UNIX-like operating system. It allows users to easily load, unload, and switch between different software packages and their associated environment variables (like PATH, LD_LIBRARY_PATH, etc.). 

Below the list of different environment modules that are currently available in KURIA cluster:

```

----------------------- /opt/ohpc/pub/moduledeps/gnu12 ------------------------
   mpich/3.4.3-ofi    openmpi4/4.1.6 (L)

-------------------------- /opt/ohpc/pub/modulefiles --------------------------
   Compilers/Intel/2024.1                Libraries/hdf5/1.14.3           (D)
   Compilers/clang/16.0.6                Softwares/Gadget-4/4.0
   Languages/R/2024.2.6                  Softwares/Geant4/11.2.1
   Languages/julia/1.10.0                Softwares/Mitmeep/1.29.0
   Languages/python/2.7.16               Softwares/Netgen/6.2.2
   Languages/python/3.8.16               Softwares/QuantumESPRESSO/7.3
   Languages/python/3.10.5               Softwares/QuantumESPRESSO/7.3.1 (D)
   Languages/python/3.11.6               Softwares/Sagemath/10.2
   Languages/python/3.12.1        (D)    Softwares/Singular/4.3.2
   Libraries/fftw/2.1.5                  Softwares/Ubermag/2023.11
   Libraries/fftw/3.3.5                  Softwares/gap/4.12.2
   Libraries/fftw/3.3.8                  autotools                       (L)
   Libraries/fftw/3.3.10          (D)    cmake/3.24.2
   Libraries/fgsl/1.3.0                  gnu12/12.3.0                    (L)
   Libraries/fgsl/1.5.0           (D)    hwloc/2.7.2                     (L)
   Libraries/gsl/2.4                     libfabric/1.19.0                (L)
   Libraries/gsl/2.5                     ohpc                            (L)
   Libraries/gsl/2.6              (D)    os
   Libraries/hdf5/1.12.3.parallel        pmix/4.2.9
   Libraries/hdf5/1.12.3                 prun/2.2                        (L)
   Libraries/hdf5/1.14.3.parallel        ucx/1.15.0                      (L)

  Where:
   D:  Default Module
   L:  Module is loaded
```
In the Kuria cluster, modules are organized into various categories, including Compilers, Languages, Libraries, and Software. Each category encompasses specific versions of tools, programming languages, and libraries tailored to different tasks and applications. When a module is loaded, the user environment is dynamically modified, allowing applications to run smoothly without conflicts. Here are some essential commands to manage modules:

```
# Listing Available Modules
module avail

# Loading a Module
module load Languages/python/3.12.1

# Unloading a Module
module unload Languages/python/3.12.1

# Displaying Loaded Modules
module list

#  Purging All Loaded Modules
module purge

# Displaying Module Information
module show Languages/python/3.12.1

# Searching for Modules
module spider <keyword>
```