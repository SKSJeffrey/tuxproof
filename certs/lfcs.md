## Archiving and Compressing Files and Directories
```bash
$ tar -cvf data_backup.tar data_backup/
   # compress all files into one backup file

$ tar -tvf data_backup.tar | less
   # list the contents of a tar file

$ gzip data_backup.tar
   # creates data_backup.tar.gz

$ tar -zcvf data_backup.tgz data_backup/
   # compress and gzip in one command

$ tar -zxvf data_backup.tgz new_folder/
   # unzipping and extract all files

$ tar -zcvf data_without_test.tgz --exclude=test.txt data/
   # create a compressed and zipped archive without the test.txt file
```

## Assembling Partitions as LVM Devices


## Creating Backups
dd - convert and copy a file
```bash
$ dd if=/dev/sdb1 of=/root/sdb1.img
   if: input file
   of: output file
```

Make an ISO copy of the CDROM
```bash
$ dd if=/dev/sr0 of=/root/vmtools.iso
```

## Creating Local User Groups

```bash
$ groupadd group-one
# Same as
$ addgroup group-two
```

```bash
$ cat /etc/group | grep 'group-one\|group-two'
   group-one:x:1002:
   group-two:x:1002:
```

To add user **linuxuser** to `group-one` and `group-two`
```bash
$ vim /etc/group
   group-one:x:1002:linuxuser
   group-two:x:1002:linuxuser
```

If we want to be able to change into a group so all subsequent files/directories created are under said group, we need to set a password for said group
```bash
$ gpasswd group-one
   # enter password
$ gpasswd group-two
   # enter password
   
# change into the group
$ newgrp group-one
   # type in group-one's password
```

All files/directories created now will be created as
* user: linuxuser
* group: group-one

To change the group ownership of a file/directory to `group-two` without logging into `group-two`
```bash
$ chgrp group-two file_name.txt
```

## Managing File Permissions
```bash
$ chmod a+rwx file_name.txt

   a: all
   u: user
   g: group
   o: other

   r: read: 4
   w: write: 2
   x: execute: 1
```

## Managing FSTAB Entries
```bash
//192.168.1.60/news   /mnt/tmp    cifs    credentials=/mnt/.smbcredentials,defaults   0   0
```

## Managing Local User Accounts
```bash
$ useradd -d /home/testuser testuser
   -d: designate home directory$
$ passwd testuser
   <type password>
```

```bash
$ adduser testuser
   # spits out more information than useradd and contains more functionality
```

```bash
$ userdel testuser
   # does not automatically delete the user's home directory
$ userdel -r testuser
   # -r: remove the user's home directory
```

## Managing the Startup Process and Related Services
```bash
# Ubuntu 14.04
$ cd /etc/init.d
$ status ssh
$ start/stop/restart ssh
$ echo "manual" > /etc/init.d/sshd.override
   # this will disbale sshd on startup; to re-enable, just remove the override file
   
# CentOS 7.0
$ cd /etc/rc.d
$ systemctl status sshd
$ systemctl start/stop/restart sshd
$ systemctl enable/disable sshd
```

## Managing User Accounts
Creating a linux user manually without using the `useradd` or `adduser` utilities
```bash
$ vipw
   # VI the password file to add a new user
testuser:x:1002:1002:,,,:/home/testuser:/bin/bash

$ vigr
   # VI the group file to add a new gruop
testuser:x:1002:

$ mkdir /home/testuser
$ cp -rf /etc/skel/* /home/testuser
$ chown -R testuser:testuser /home/testuser

$ passwd testuser
   # type password
```

## Managing User Account Attributes
```bash
$ chfn
   # Changing the full name of the user
$ chsh /bin/dash
```

Prevent a user from logging in and provide a message to them
```bash
$ chsh /bin/false
   # can still 'su - testuser'
   # cannot SSH into testuser
$ chsh /sbin/nologin
   # cannot 'su - testuser
   # cannot SSH into testuser
```

## Managing User Processes
```bash
$ top 
   # NICE: 20 = lowest priority
   # NICE: -20 = highest priority
$ htop
   # much nicer view of system processes
$ ps
   # show processes running for current user
$ ps aux
   # show all running processes for all users
```

Find a process based on the name
```bash
$ pgrep bash
   # returns a PID of 1794
$ ps aux | grep 1794
   # verifies that bash has a PID of 1794
```

Kill the PID of cron
```bash
$ ps aux | grep cron
   # returns the PID of cron - 1292 that is owned by root
$ kill 1292
   # the kill command must be run by root
$ kill -KILL 1292
   # send a signal to the OS kernel to shutdown the process
$ kill -15 or -9
$ kill -HUP 1292
$ kill -l
   # shows all signals that can be sent to the application or the OS
```

To re-priotize the application that is running, we can `nice` or `renice` the PID of the program
```bash
$ renice 10 1292
   # lower the priority of PID 1292 to 10
$ nice -n 20 1310
   # make the default value of 1310 start at priority of 20
```

## Restoring Backed Up Data
```bash
dd if=/root/sdb1.img of=/dev/sdc1
mount -o loop /root/vmtool.iso /mnt/tmp
```

## Accessing the Root Account
```bash
$ su - 
```

## Using SUDO to Manage Access to the Root Account
```bash
$ visudo
   # change the sudoers file
$ sudo vim /etc/sudoers
   # do not use the above method because you can bork the file
```

## Basic Bash Shell Scripting: Basic Setup and Execution
```bash
$ export PATH=$PATH:/root/scripts
   # add the PATH /root/scripts to my $PATH so we can execute the script from anywhere
```

## Basic Bash Shell Scripting: Conditionals and Loops
```bash
#!/bin/bash

DIRECTORY="/root/scripts/test"

if [ -d "$DIRECTORY" ]; then
	echo "This directory exists"
else
	echo "This directory does not exist"
fi

for count in 1 2 3 4 5
do
	echo "This is line # $count"
done

while read HOST; do
	ping -c 3 $HOST
done < myhosts
# a file called 'myhosts' contains 2 lines:
#   8.8.8.8
#   8.8.4.4
```
