## Chapter 1. Introduction and Essential Concepts

### API and ABI
- API: defines interface between two sw components
- ABI: defines interface bw software on a particular hw

### System vs Application Programming
- System Programming: mostly interact with kernel and systemcall and low level libs
    - it contain three parts, system calls, the c library and the compiler
- application Programming: mostly interact with high level libraries

### Standards
- SUS: single Unix Specification
- Posix: Portable Operating Systtem Interface

### Regular Files
- Regular Files: it stored bytes as linear stream of data
    - it represent using inode in filesystem

### Special Files
- Special files: char device files, block device files, named pipes and unix network sockets
- char device files: it access as linear queue of bytes
- block device file: it access as an array of bytes
-  Named pipes: act like regular pipes but are accessed via a file, called a FIFO special file


### Directories
- directory is like a regular file which keep the inode and file name mapping which kernel use to resolve the path
- entries inside the directory file called it dentry
- name and inode pair called ***link***


### Hard Links
- hard link should be in the same filesystem, so same inode with different filename
- deleting a file unlinking from the directory , removing the pair once, ecah inode contain the link count
- once each link count reaches zero then kernel removes the inode and it assocated data from filesystem


### Symbolic link 
- it is regular file, and has its inode and data chunk, which contain the pathname to the linked file


### Filesystem
- Linux, like unix usage unified and global namespace of files and directories
- Filesystem collection of files and directories, can be added and reomved from the global namespace call mounting and unnmounting
- filesystem added in the root **/** of the namespace is called root filesystem
- smallest addresable in block -> sector
- block is logical addresable unit
- block > sector
- block <= page size (memory mangement smallest unit), 512, 1kb, 
- linux support per process namespace, process will unique view of system files and directories hiearchy

### Process and threads
- linux tread thread as process with some shared resources

#### Process Heiarchy
- Linux follow process tree, first process is init with pid 1, 
- creating a process using the fork() systemcall
- if child process terminated, kernel does not remove it resources from the memory, so it parent job to inquire about the child terminated process called **waiting**, then only fully destroyed, if parent does not wait then it called **zombie process**

### User and Groups
- every process has user id , numeric value, it inherit from the parent
- which identifies the user running the process and that is process real uid.
- login program spawn the shell as per the login user that gives uid to that login shell, username and id mapping present in /etc/passwd
- along with real uid, there are also ***effective uid, saved uid and filessystem uid***
- Each user belongs to one or more groups, including a primary or login group, listed in /etc/passwd, and possibly a number of supplemental groups, listed in /etc/group.

### Permission
- Directory Read Permission - list the content
- Directory Write permission - create links and new files
- Directory Execute permission- entered in directory and used in pathname

### Signals
Signals are a mechanism for one-way asynchronous notifications. A signal may be sent from the kernel to a process, from a process to another process, or from a process to itself.

### IPC
IPC mechanisms supported by Linux include pipes, named pipes, semaphores, message queues, shared memory, and futexes.

### Error Handling
- `perror`, `sterror`, `sterror_r`,
- errno is global variable, in multithreaded program it store per thread

## Chapter 2: File I/O

### 