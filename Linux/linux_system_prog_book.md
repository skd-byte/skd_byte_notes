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

### Intro 
- kernel keep per process opened list of file -> file table, indexed by descriptor, 0, 1, 2 opened by default
Redirection via fork() + exec() separation
> The gap between fork() and exec() lets the shell manipulate file descriptors before the new program runs.
```
// Shell handling: wc p3.c > newfile.txt

pid = fork();
if (pid == 0) {
    // Child — before exec():
    close(STDOUT_FILENO);                          // close stdout (fd 1)
    open("newfile.txt", O_CREAT|O_WRONLY|O_TRUNC, 0644); // opens as fd 1
    exec("wc", "wc", "p3.c", NULL);               // wc writes to fd 1 → file
}

```
> - Why it works: open() always takes the lowest available fd. After closing fd 1, the new file gets fd 1 — so wc thinks it's writing to stdout, but it's actually writing to the file.

> - The child inherits the parent's fd table after fork(), and exec() preserves it — so the redirected fd survives the program switch.

### Opening Files
- if a file is read-only to a given user, then a process owned by that user can open that file O_RDONLY but not O_WRONLY or O_RDWR.
- O_CLOEXEC, O_NONBLOCK

#### Owners of New File
- uid of the file owner is the effective uid of the process which is creating the file
- file’s group is set to the gid of the parent directory

### Permission of New file
- mode argument with complement of the user's file creation mask (umask)

### Return Values and Error Codes - open and create
- set errno and return -1 mostly
- typical response prompting user with suggestion with different file names or simply terminating the program

### Reading via read()
- regular files read advanced by how many bytes read, that is return value
- char device file which does not support the seeking that will alsway start from the current position


### Read Return Value
- success will return with number of byte reads
- eof will return 0
- no data availble vs eof, no data avialable make sense in case of socket, pipe or device file
- read interrupted by singal it return -1, with errrno EINTR

### Write
- special file always write from head
- partial write not likely happen on the regular files
- partial write is possible in socket and special files, so generally we dont do write in the loop but if first write fail then second write will return proper error

#### Append
- write alway in end of file, instead of the position always write end, open file in this mode(O_APPEND) in case of multiple process writing to same file example - logs
- all writes will get append

####  Noon blocking writes
- there is generally no non blocking writes in regular file, it always write when return

#### Behaviour of Write
- wrtie call not gurantee data written to disk, kernel copy the data into kernel buffer from supplied buffer
- read call might return the data from these buffer if those write buffer still dirty
- kernel maintain the dirty buffer and flush them later, control from this /proc/sys/vm/dirty_expire`


### fysnc, sync, fdatasync
- fsync- metadata and data of desccriptor
- fdatasync- only data
- sync - all the buffer, as per the requirement only sync to initaite the process to flush the data but not gurantee before return all the data flushed, but linux guranteed it
- read always synchronize and writes not

### Direct I/O 
- like modern operating system, linux implement layer of caching, buffering, and i/o management between application and devices
- O_DIRECT flag in open tell the kernel to minimize presence of IO mangement, mostly required high performance devices
- all i/o will be synchronous

### Closing File
- closing the file would not flush to disk, so flush before closing the file
- unlink opened file then it will only be removed from disk once it is closed


### seeking with lseek
- SEEK_CURR, SEEK_END, SEEK_SET
- lseek return the updated file position
- seeking end of file, padded with zeros, called sparse files, means it has holes
- unseekable objects: pipes, fifo and socket etc

