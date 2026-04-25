# Table of Contents

- [Linux Fundamentals](#linux-fundamentals)
  - [Essesntials tools](#essesntials-tools)
  - [Essentials File Management](#essentials-file-management)
  - [Advance file Management Tools](#advance-file-management-tools)
  - [Working with text file](#working-with-text-file)
  - [Advance text file Processing](#advance-text-file-processing)
  - [connecting to a server](#connecting-to-a-server)
  - [Working with the bash shell](#working-with-the-bash-shell)
  - [User and Group Management](#user-and-group-management)
  - [Permission Management](#permission-management)
  - [storage management essentials](#storage-management-essentials)
  - [managing networking](#managing-networking)
  - [Working with Systemd](#working-with-systemd)
  - [Manging Software](#manging-software)
  - [Managing SSH](#managing-ssh)
  - [Managing time](#managing-time)
  - [Process Management](#process-management)
  - [Scheduling Tasks](#scheduling-tasks)
  - [Reading Log Files](#reading-log-files)
  - [Container and Linux](#container-and-linux)
  - [Managing the Kernel](#managing-the-kernel)
  - [Managing the hw](#managing-the-hw)
  - [Linux Security](#linux-security)
  - [Regular Expression](#regular-expression)
  - [Command Basics](#command-basics)

---

# Linux Fundamentals

## Essesntials tools
- `uname -a`: print system info
- `sudo`: allows authorized user to run tasks with esclated priv
-  to allow `sudo` usage user must be member of th sudo group
- `sudo` configuration can be created to allow users to perform specific tasks only
- `whoami`: curent user name
- `id`: list of group that the user is member of
- commands used with long (--) and short option (-) and short option can be combine ls -lrt
- `passwd`: change curent user paassword
- 'ip a': show ip interfaces , `lo` interface loopback and used for internal comm
- `man [command]`: command documentation
- `man -k [search_string] or apropos`: search all the command with the search string
- `mandb`: manual databases 
- `info`: usage information of some commands
- `command --help`
- `/usr/share/doc/` contains limited additional help for some commands
- `touch`: create the empty file
- GNOME is mostly the graphical display manager in most of the distribution
- GNU - two distribution
	- debian
	- red hat

## Essentials File Management
- `cp -a`: copy all file `cp /tmp/file /xzy`: it will create the file /xzy and copy the contents of /tmp/file into it
- `mkdir -p`: create the directories with path
- `rmdir': remove empty directories
- `mv src dst`: rename or move the file

## Advance file Management Tools
- **Hard Link***: pointng to the same inode on the same file system
	- ln [file] [new_file]
- **Symbolic Link**: it is shortcuts , can exist on a directory and cross device also exists
	- ln -s [file] [new_file] 
- `find`
    - find / [-name][-user][-size][-perm]
    - ex 
	- `find / -size +2G`, `find / -user shubhamkantd`, `find / -perm /4000`, `find / -name "*.log" -exec rm {} \;` etc.
- `-exec`: runs a command on items using {} as placeholder; \; executes per item, + batches multiple items (faster).
-  Most commands work with groups and ranges
    - touch file{1..100}
    - mkdir /data/{sales, account}
- `find /etc -type f -exec grep -l shubhamkantd {} \; -exec cp {} ./ \;` 2>/dev/null
- `find /etc/ -name "*" -type f | xargs grep "127.0.0.0"`
- `find /etc/ -name "*in" -print '%s, %p\n' | sort -nr

### tar
- archives the file and also do the compression
- `tar -cvf my_home.tar /home`
- `tar -xvf my_home.tar -C /home/tmp`
- with compresssio -cjvf (bz) -czvf (gz) -cJvf (xz)
- `file my_home.tar`: tell type of file

### mounting
- lsblk, df -h, mount | grep '^/', findmnt

## Working with text file
-`tail -<n> /etc/passwd` or `head -<n> <file>`
- `cat file.txt`
	- -A show all non-printable characters
	- -b number lines
	- -n number lines but not empy lines 
	- -s supressed repeated empty lines
- `tac` rverse order of cat

## Advance text file Processing
- `cut` is an easy command that filter out columns from text files
- `cut -f 3 /etc/passwd`
- `cut -d: -f 3 /etc/passwd`
- `sort -n` sort in numeric order
- `tr` translate lower to upper or upper to lower
- `sed -n 5p /etc/passwd`
- `sed -i -e '2d' regex` delte line two

## connecting to a server
- su: switch user
- root user is a linux kernel space user that always exists
- su <user>
- su : open a root shell
- `su -`: open a login: environment variable are set as the target user
- as a sudo-enabled user, use **sudo command** to run command with escalted privileges
- the default "admin" user has **sudo** priveleges by default
    - on ubuntu because of membeship of the group sudo
- `sudo -i` to open a root shell
- `sudo apt install openssh-server` and enable and start `sudo systemctl enable --now sshd`


## Working with the bash shell
- `sudo sh -c "echo hello > /root/file.txt"` → runs the whole command as root, so the file is created in /root without permission errors.
- `sudo echo hello > /root/file.txt` fails because > runs as your user, not root, so it lacks permission to write to /root
- standard input(0): < , `wc -l < /etc/passwd` or `wc -l /etc/passwd` later one works because it as agument
- `history -c` clear the current history
- Variables
    - system variables contain default setting like $PATH and $SHELL
    - environment variables `varname=value` use export if you want to see it in subshell
- /etc/environment contains a list of variables and first file processed while starting bash
- /etc/profile is executed while user log in
   - `/etc/profile.d` is used as drop-in directory that contains additional configuration
   - `~/.bash_profile` can be used as a user specific version
   - `~/.bash_logout` is porcessed when user logs out
- `/etc/bashrc` is processed every time a subshell is started
   - a user specific `~/.bashrc` file

## User and Group Management
- every linux user is part of atleast one group
- while creating the file that group will become group owner
- most linux distributions are using "private groups" which means that while creating a user, a group with the anme of the user is created and the user is the only member of that group
- `useradd` and `useradd -m` create the user later one with home directoy
- `/etc/login.defs` setting from this file applied when user created , we can change thiis file befor creating the user , it wont change the current user settings
- `useradd -D` to get an overview of currently effective default setting
- to create a group `groupadd groupname`
- add users to a group as secondary group using `usermod -aG groupname username`
- users and their properties are stored in /etc/passwd
- use `detent passwd username` to list the current propertiesof user
- to change the properties use commands like `usermod` and `passwd`
- groups are stored in `/etc/group`
- `/etc/skel` contents is copied to user home directory upon uset creation
- `passwd` and `chage` use to manage the properties of user
- password setting are written to `/etc/shadow`
- `who` and `w` show who is currently logged in
- `loginctl` for current session management

## Permission Management
- permission determine what user can do on a file or directory
- Read(4) write (2) aand execute (1), these are octal values, directory also require execute to do the `cd`
- permission are assigned to the user, group and other file owners
- standard linux permission allow for one user-owner and one group owner for each file
- use `chown` to change the user ownership
    - `chown anna myfile`
    - `chown anna:sales /data/sales` group owner and user owner 
- use `chgrp` to change group ownership
    - `chgrp sales /data/sales`
- use `chmod` is used to manage file permission
    - `chmod 750 myfile`
    - `chmod u+w myfile`
    - `chmod +x myscript`
- Advance linux permission
   - set user id suid (4): the file runs with the permissions of the file owner, not the user executing it
   - set group id (sgid) (2):On files → runs with the group’s permissions, on directories newly created items as the group owner of that directoy
   - sticky bit (1): Used on directories → only the file owner can delete their files, even if others have write permission.
> Who is allowed to run the file? With whose permissions does it run?
- the `umask` is a shell setting that defines a mask that will be subtracted from the default permissions, present i /etc/profile or /etc/bashrc

## storage management essentials
- block device
	- /dev/sda : first scsi hard disk
	- /dev/sda: second scsi hard disk
	- /dev/vda: kvm hard disk
	- /dev/nvme01 first nvme hard disk, 0 means connected to bus 0 and node 1
	- /dev/sr0: optical drive
- command `lsblk`
- there are several utility to create the partitions like `fdisk /dev/nvme01` it will open prompt use n, p and w command
- to use a partition, a filesystem must be created on top of it
- generic file system are xfs ext4
- `mkfs.xfs` or `mkfs.ext4` to create these filesystems
- `vfat` is used for the uefi system partition, which is required while booting to get essential boot information from disk
- list all filesystem of each partition `sudo mount | grep '^/'`
- mounting filesystem
	- to use a partition containing filesystem, it must be mounted
- `mount /dev/sdb1 /mnt` and `unmount /mnt`
- `lsof` to find out which process are keeping the device busy
- to verify current mount
	- `mount`
	- `lsblk`
	- `df -h`
	- `findmnt`
- make mount persistent
	- /etc/fstab file is used to make the mounts persisten
	- `blkid` to list the uuid of partition
	- add this in the fstab file

## managing networking
- comman command `ip a, ip route show, ping, ss, dig, nmap, hostname, hostnamectl`


## Working with Systemd
- it is the first thing that is started after the linux kernel
- the main function is to manage services
- it is event driven, which means that it can react to specific events
- the items that are managed by systemd are called units
	- `systemctl cat name.service`: reads current unit configuration
	- `systemctl show`: show all available configuration parameters
	- `systemctl edit name.service`: allow you to edit servie configuration
	- `systemctl daemon-reload`: after modifying the service configuration
	- `systemctl restart name.service`: to restart the service
	- `systemctl -t help`
	- `systemctl list-unit-files`
	- ex `systemctl status httpd`
- a target is group of services
	- some target are isolatable, which means that you can use them as a state your system should be in
	- emergency.target
	- graphical.target
	- multi-user.target
- systemctl get-default: get the default target
- `systemctl list-dependencies name.target` to see the contents and dependencies of a systemd target

## Manging Software
- `dpkg`: list of package installed in ubuntu
- `ldd`: list library dependencies
- first make sure that the local list of packages is updated to the most recent version of packages using `apt update` 
- `apt search` : to search packages
- `apt isntall`: install packages
- `apt upgrade`: upgrade packages
- `apt autoremove`: remove
- `sudo apt-cache search filename` : to search for packages containing a specific file

- snap is an advanced package manager that applies container technology to run package in isolation

## Managing SSH
- ssh servers that offer access on port 22
- can modify a few parameters in /etc/ssh/sshd_config to make it more secure
	- port: defines port on which ssh is listening
	- permitRootLogin: consider disabling root login
	- few more like this parameter for configuration
- to connect to an non default port use -p (ssh) or -P (scp)
	- ssh lisa@webhost -p 2022
- when first login in to an ssh server the server host key is cached in the file ~/.ssh/known_hosts
- ssh-keygen
- ssh-copy-id sshserver
- use ssh-agent /bin/bash to run the ssh agent, which can cache the passphrase for the session duration

## Managing time
- hw time sync with ntp(network time protocol)
- ubuntu uses `systemd- timesyncd` to synchronize time
	- command `timedatectl`
	- configuration is written to the /etc/systemd/timesyncd.conf configuration file
## Process Management
- anything run on linux runs as a prcess
- jobs are user process that have been started from a specific shell
- all process originate as children from systemd
- `kill` use SIGTERM - 15, SIGKILL 9
- `ps faux` : process forest, show parent child relation between processes
- `ps aux` : show all process with information
- `ps aux --sort pmem`: sort process by memory usage
- `ps -ef`: system v way of doing it 
- `ctrl+z` and then `bg`: to move process / jobs to foreground
- `jobs` list the job `fg <job number>`move to foreground
- process priorities
	- adjust prior `nice` to start process
		- non priv users can only run process with a lower priority
		- priv user can increase and decrease prior
	- range -20 to 19 lowest
	- to adjust prioirty for running process use renice
- `killall` kill based on process name


## Scheduling Tasks
- `crond.service` schedule task
- `ls /etc/cron*/`
- `systemctl -t timer list-units`
- `systemctl -t timer list-units-files`
- `vim skd.service`
- `vim skd.timer`
- `sudo systmctl start skd.timer`
- `sudo systmctl daemon-reload`
- `systmctl status skd.service`
- `sudo systmctl status atd`
- echo "hello logger" | at teatime tomorrow
- `systmctl cat fstrim.timer / systmctl edit fstrim.timer`

## Reading Log Files
- syslog: legacy logging
- syslog - rsyslogd
- systmd-journald is a systemd-integrated log service
- `journalctl` show complete journal
- `journalctl -u <units>`
- `journalctl --dmesg`: show kernel messages
-  by default systemd journal is not persistent, make persistent using Storage=auto in /etc/systemd/journald.conf and start using systmctl start journal-flush, and directory will be created /var/log/jouran
- journalctl -u corntd -p info 
- journalctl -xb <unit> : boot messages with explanation  
- journalctl -p error : error message 
- rsyslog
   - sudo vim /etc/rsyslog.conf
   - sudo systemctl satus rsyslog.service
   - /var/log/message
   - cd /etc/rsyslog.d/ : dropin files for configuration if not present in /etc/rsyslog.conf
- `logger` : write directly to logging entity
## Container and Linux
- what is container: a container is an application that is started in an envirronment that is isolated by linux kernel namesapaces from a container image
    - the kernel is provided by the host operating system, no need a kernel inside a container
    - the host that runs a container has the container runtime and the container manage software
    - container runtimes are containerd and cri-o
    - common container managers are Docker and Podman
    - container technology is standardized by the open containers initiative
    - means same container image can be used any environment like cloud, distributed and stand alone
- docker run -d nginx
- docker run -d docker.io/library/nginx

## Managing the Kernel
- initial ram drive (initrd) or initial ram filesyste(initramfs) loaded by the kernel while booting and contains the essential drivers for essential hardware
- other hw init by systemd-udevd while booitng or when the hw is connected
- modprobe can be use to load the kernel modules
- `lsmod` shows currently loaded kernel modules
- `lspci -l`: shows pci devices and kernel modules loaded for the specific devices
- `lsusb`: show usb devices 
-/proc filesystem is a pseudo filesystem that provides access to kernel settings and parameters
    - /proc/meminfo : detailed insights of the kernel
    - /proc/sys contains kernel tunables
    - /proc/<pid>  is directory structure that has detailed information about process running on linux

## Managing the hw
- systemd-udevd is a standard component on linux distributions
- it ensures that hw that is plugged in is enabled with the appropriate properties
    - user modification are in /etc/udev/rules.d
- to monitor hw initialization use `usdevadm monitor` while plugging in or removing a device
- `lshw`: gives an overview of all currently installed hw
- `lscpu`: lists cpus and their properties
- `lsmem`:list memory devices
- `lsinitrd`: shows what is inside the initrd file
- hardware propertis can be monitored through the /sys pseudo filesystem
- `udevadm info` to get the details about the devices:
   - udevadm info /dev/sdb
- `modinfo sr0` to show properties
- `arch`
- `uname -r`

## Linux Security
- mandatory access control is an optional security systems that uses the linux kernel security modules to restric access based on security policies and profiles
- mandatory access control is independent of the system of linux permissions
- it is manily used as a solution that locks down a system for types of acess that haven't specifiacally allowed
- selinux or redhat and apparmor on unbuntu
- `firewall-cmd, ufw, getenforce, setenforce, sealert, restorecon, aa-status, apparmor_parser aa-complain, aa-enforce`

## Overview of Regular Expression

### About Character Classes
- `\d`: a single digit (0-9)
- `\w`: word characters (letters, digits, underscore)
- `\s`: whitespace chars (space, tab, newline)
- `.`: match any single character
> Uppercase \D, \W, and \S negate their lowercase versions. (The dot has no negated form.)
### About Quantifiers
- `*`: zero or more of the preceding character
- `+`: one or more of the preceding character
- `?`: zero or one(only) of the preceding character
- {M, N}: repetition count. M = minimum, N = maximum (inclusive) ex {5,} means 5 or more
   - ex search ipv4 string `^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$`, Anchors (^ and $) force full-string match
### Custom Character Classes
- go beyond single characters
   - `[A-Z]`       : define a range, ex all uppercase A to Z
   - `[A-LN-Z]`    : all capitals except M
   - `[\da-fA-F]`  : hex digits 0-9 and a-f
   - `[^a-f]`      : caret negates the class, here anything except lowercase a-f
- ex to match hexa number `0x[\da-fA-F]+`

### About Alternation (Branching)
- match among multiple options, like if/else, with vertical pipe `|`
   - ex match bin or hex number `0x[\da-fA-F]+|0b[\d]+`

### Understanding Groups
- parentheses around part of your regex form a group, e.g. /my_(regex)_here/ – (regex) is a group.

#### About Sub-Expressions
- Wrap part of your regex in parentheses to form a group that acts as its own sub-expression.
    - ex `(shubham\.)?kant\.dubey` and  `shubham\.|kant\.dubey`
	```
	shubham.kant.dubey
	kant.dubey
	```
> with alternation it will do the extra match of `kant.dubey`
> Groups with quantifiers: Quantifiers apply to the full group in parentheses, same as for character classes—[a-z]+ repeats any char in the set, and (abc)+ repeats the full sequence abc.
-   every match has groups and numbering follows the order of opening parenthesis — first `(` is group 1, second is group 2, etc...

#### How Capturing Groups Work
- Text captured by a group is saved and can be recalled with $#, where # is the group number.
```
import re

string = "username_123"

match = re.search(r"username(_\d+)", string)
if match:
    suffix = match.group(1)  # group(1) is the first capturing group
    print(f"Suffix is: {suffix}")
```
>  (?1) refers back to capture group #1
- ex `(column(\d|A)+) = (?1)` it will matches the both why - because `(?1)` → recursive pattern match of Group 1

```
 column0555AA = column0AA
 column0AA = column0AA
```
> Note: (\d|A)+ is like [0-9A]+

#### About Non-Capturing Groups
- Prefix a group with `?:` ex `(?:pattern)` to make it non-capturing. The engine treats it as one unit but won’t save it for backreferences.

### Understanding Anchors
- Anchors match positions in a string rather than characters. They pin a pattern to a specific spot

- `^`: beginning of a line 
- `$`: end of a line
- `\b`: word boundary — transition between word and non-word character, or start/end of a word.
- `\B`: non-word-boundary, the inverse of \b

```
# Match words starting with 'c'
echo "cat catalog concatenate" | grep -oP '\bc\w+'
# Output:
# cat
# catalog
# concatenate

# Match 'at' inside words, not at the start or end
echo "cat bat matx at" | grep -oP '\Bat\B'
# Output:
# matx
```

### About Non-Greedy Matching
- The * wildcard is greedy and grabs as much as it can, but appending ? makes it non-greedy, halting at the first match.
```
echo '"apple" "banana" "cherry"' | grep -oP '".*?"'
# Output:
# "apple"
# "banana"
# "cherry"
```

### How to Escape Characters
- To match a special character literally (like ( or )), escape it with a backslash. Without escaping, regex treats it as a group, which can produce unwanted captures.

## Overview of Command Basics

### Using man
- `man <command> | vim` -: read in vim

### About Command History
- `history`: show history of command
    - `!!` press enter will run the most recent command
    - `!<#>` run the specific command where `#` is the command number
    - `!<text>` where `<text>` is the few chars of command it will match with the recent one

### Output Redirection
- `>`: will redirect to file
    - `command > file.log` redirect stdout to file log
    - `command &> file.log` or `command > file.log 2>&1` will redirect both stdout and stderr
        > `>>` use for append instead of overwrite
- `|`: will redirect (pipe) to the next command
    - `command1 | command2`: command1 pass its stdout to command2 stdin
    - `command1 |& command2`: also redirect its stderr along with stdout

### About Chaining Commands
- `;`: run command in order regardless of whether previous command is successful
- `&&`: run the command in order will stop as soon as previous command encountered (non-zero exit code) error

### Using Aliases
- `$HOME/.aliases`
    - alias '<name>' <command>

### About Glob Patterns
- Wildcard matching for filenames
    - `*`: match anything
    - `?`: match a single character
        > unlike regex `?` (0 or 1), glob `?` matches exactly 1 character
    - `[...]`: match characters in the class (works like regex character class)
        > `[!a-h]`: negate with !
    - `{A,B,..}`: multiple options to match. Works like regex alternation `|`
        - ex `*.log{.gz,*}` expand to `*.log.gz` and `*.log*`


### Overview of grep / zgrep
- zgrep is a variant of grep that can search compressed gzipped(*gz) files directly

#### Commonly Used Flags
- `-i`: case insensitive search
- `-E`: use extended regex (no escaping needed for special chars)
    > these two are equivalent `grep '^\(a\|b\)'` and `grep -E '^(a|b)'`
- `-v`: invert — only show lines that don't match
- `--color`: highlight matched portion
- `-m <n>`: stop after n matching lines
- `-n`: show line numbers
- `-r` or `-R`: recursive search; `-r` may skip symlinks
- `-l`: show only filenames that match
- `-I`: skip binary files
- `-A <n>, -B <n>, -C <n>`: n lines after, before, and around the match
- `--exclude <GLOB>` - skip files matching the Glob
    - `--exclude '*.log'`
- `--exclude-dir <GLOB>`: skip directories matching Glob
    - `--exclude-dir '*Linux_x86*'`
- `--include <GLOB>`: only search files matching the Glob

- ex
```bash
grep -Ei 'nbytes=(0x[0-9a-fA-F]+).*free=\1&' ~/sample.txt
```

### Overview of find
- `find <path1 path2 ...> <pattern> <additional options>`
    - `-name <pattern>, -iname <pattern>`: match by filename (basename)
    - `-path <pattern>, -ipath <pattern>`: match on the full path with filename
    - `-regex <pattern>, -iregex <pattern>`: apply a regex for matching
        > works on the entire path with implied ^ and $ anchors, so for /path/abc.xy `".*abc.*"` matches but `"abc.*"` does not

#### Combining Multiple Patterns
- `-o` (or) and `-a` (and) operators to combine conditions
- `-not`: negate a condition
    - `find . \( -name "Makefile" -o -name "*.mk" \) -a -not -path "*.make*"`

#### Running Commands on Results
- `-exec`: run a command on each result `find . -name "*.log" -exec ls -l {} \;`
- `-ok`: like exec but prompts before each run `find . -name "*.log" -ok ls -l {} \;`

- `-type` <type>
    - f: file
    - d: directory
- `-maxdepth <n>`: limit search depth; n=0 means only the starting directory

### Overview of sed
- stream editor, used to apply operations on text streams

#### How Substitution Works
- `s/REGEX/replacement/`
    - `sed 's/addr=[0-9a-f]\+/addr=XX/g' skd.txt` : with + use escape otherwise it will treat as plain text and g is for global
        - `sed -n 's/addr=[0-9a-f]\+/addr=XX/g' skd.txt`
            - `-n` : suppress output, makes sed quiet (also --quiet or --silent)
        - `sed -n 's/addr=[0-9a-f]\+/addr=XX/g p' skd.txt`
            - `p`: tells sed to print only matched lines; without it all file content is printed
> sed doesn't support [\d] shorthand, use POSIX classes instead like [:digit:], [:alpha:] `sed -n 's/addr=[[:digit:]a-f]\+/addr=XX/g p' skd.txt`

#### About Delimiters
- `sed -n 's/\/usr\/local\/sim\/[0-9A-Z\.\-]\+\/*run\/logs\///g p' skd.txt`
- `sed -n 's#/usr/local/sim/[0-9A-Z\.\-]\+/*run/logs/##g p' skd.txt`: the char right after `s` becomes the delimiter, here `#` is used

#### Using Extended Regex
- use `-r` (sed's equivalent of grep `-E`), these two are equivalent
    - `sed 's/[0-9]\+/NUM/g' skd.txt`
    - `sed -r 's/[0-9]+/NUM/g' skd.txt`

#### Working with Match Groups
- `sed -r -n 's/src_ip=([0-9.]+), dst_ip=([0-9.]+)/route=\1->\2/g p' skd.txt` : \1 and \2 reference captured group 1 and 2

#### Chaining Multiple Commands
- pass multiple commands with the -e flag (short for --expression).
- `sed -r -n -e 's/src_ip=([0-9.]+), dst_ip=([0-9.]+)/route=\1->\2/g p' -e 's/[0-9]+/NN/g p' skd.txt`

#### Restricting Match Range
- `sed -r -n '10,18 s/[0-9]+/NN/g p' skd.txt`
    - Restrict substitution to lines 10-18 (inclusive) of the file
- `sed -r -n '1,/END_BLOCK/ s/[0-9]+/NN/g p' skd.txt`
    - Restrict substitution from line 1 to the first line matching pattern END_BLOCK (inclusive).

#### Targeting Specific Occurrences
- `sed -r -n 's/[0-9]+/NN/2 p' skd.txt` : only replace the 2nd occurrence with NN
- `sed -r -n 's/[0-9]+/NN/2g p' skd.txt`: replace from the 2nd occurrence onward with NN (2g combined)

#### In-Place File Editing
- the `-i` option makes sed edit the file in place
- `sed -r -i.orig 's/[0-9]+/NN/g' skd.txt`: -i.orig creates a backup with .orig extension. Without the extension the original gets overwritten
    > Be careful with -n or p flags — only what would appear on screen gets written to the file. When patching ensure neither flag is set.

### Overview of xargs
- xargs (extended arguments) reads input from stdin and runs a command on each token (default separator is space). Think of it as a for loop
```
foreach arg in stdin.split(' '):
  <cmd> arg
```

- `-d 'delimiter'` : set a custom delimiter
- `-t`: display each command before running it
- `-p`: like -t but prompts y/n before each run

> By default xargs appends the token at the end of the command. When you need the argument placed elsewhere, use `-I <symbol>` (capital i) to give the token a placeholder name.
    - `find . -iname "*.mk" | xargs -I % mv % ../tmp/`

### Overview of awk
- Awk is a very powerful utility for processing lines of text.
    - `awk '/regex to match/ { action }'`
- Fields
    - Every line is divided into **fields**
    - Default separator = **space**
    - Use `-F` or `FS` to change the separator


- Field References
    - `$0` → full line
    - `$1` → first field
    - `$2` → second field
    - `$NF` → last field
    - `NF` → total field count
- Examples
    - `awk '/WARN.*timeout/ { print NF, $2; }' skd.txt`
    - `awk '/WARN.*timeout/ { print $2; }' skd.txt`
    - `awk -F: '/WARN.*timeout/ { print $1; }' skd.txt`

- Custom Variable
    - `awk '/WARN.*timeout/ { val=$2; print val; }' skd.txt`
