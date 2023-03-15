---
title: "Linux for DevOps and Developers"
datePublished: Mon Dec 12 2022 19:13:32 GMT+0000 (Coordinated Universal Time)
cuid: clbl6akm0000008l29rxq5xbk
slug: linux-for-devops-and-developers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1670872378696/wAy40Mapt.webp
tags: programming-blogs, linux, docker, kubernetes, devops

---

# History of Linux

In 1964, Bell laboratory started a project at New Jersey.
Project was to create a multitasking and multiuser operating system And in 1969 they withdrawed that project.
But Dennis Ritchie(developer of C language) and Ken Thompson did not give up on this project. This project was UNIX.

In 1969, Dennis Ritchie and Ken Thompson started on this project again. They made a OS called UNICS.(Uniplexed Information & computing services)

After sometime it called as UNIX. This was open source project.

In 1975, a new version of unix introduced- UNIX version 6. This was so popular at that time. So many companies made their versions using this open source unix operating system.

Examples:
#### Flavours of UNIX

<li>IBM- AIX
<li>SUN Solaris
<li>Apple- MacOS
<li>HP- UX

### <li>Linux

In 1991, There was a student Linus Torvalds.
Linux began in 1991 as a personal project by Finnish student Linus Torvalds: to create a new free operating system kernel. The resulting Linux kernel has been marked by constant growth throughout its history. Since the initial release of its source code in 1991, it has grown from a small number of C files under a license prohibiting commercial distribution to the 4.15 version in 2018 with more than 23.3 million lines of source code,

Linus Torvalds studies UNIX OS and MINIX(developer by Andrew Tenenbaum) and developed a kernel called LINUX.

In between 1991 and 1995 there was a movement going on Free software movement- GNU.

>Linux is a kernel.

As we know an OS is a mix of kernel and softwares. So OS was developed using linux kernel and GNU softwares.

Many companies created their own version of OS using linux kernel.

Examples:

<li>RHEL(Red Hat Enterprise Linux)
<li>Fedora
<li>Debian
<li>Others- Ubuntu, CentOS, Amazon Linux, Kali linux


> Linux is a kernel not O.S
> Linux is not a Unix derivative, it was written from scrath.
> A linux distribution is the linux kernel and a collection of softwares that together create an OS.

### Linux features
<li>Open Source
<li>Secure
<li>Simplified updates for all installed softwares
<li>Light weight
<li>Multiuser - multitask
<li>Multiple distribution- Redhat, debian, fedora



## File  System Hierarchy

<li><b>/</b><br> Top level root directory<br>
<ul>
<li>/root<br>
Home directory for root user.
<li>/home<br>
Home directory for other users.
<li>/boot<br>
Contains bootable files for linux.

>POST: Power on Self Test


<li>/etc<br>
Contains all configuration files.
<li>/usr<br>
by default softwares are installed in this directory.
<li>/bin<br>
(binary)Contains commands used by all users.
<li>/sbin<br>
(system binary)Contains commands used by only root user.
<li>/opt<br>
Optional application software packages.
<li>/dev<br>
Essential device files, include terminal devices, usb or any deviice attached to the system.
</ul>

# Linux basic commands

<li>Creating a file in linux

<ol>
<li>cat :- we can create or see the content but can't edit the file.
<li>touch :- create empty file,create multiple empty files.
<li>vi/vim :- it's an editor
<li>nano :- it's also an editor
</ol>

```bash
$ cat > file.txt
>ctrl+d to exit

$ touch file1.txt file2.txt file3
$ vi files.txt
$ nano file5.txt
```


<li>Switch to root user

```bash
$ sudo su
```

> super user do switch user


### <li><b>cat Command</b>
The cat command is one of the most universal tools, yet all it does is copy standard input to standard output.

we can make use of cat command for following tasks:

<ul>
<li>create file- creating single file

```bash
$ cat > file.txt
# check content of file
$ cat file.txt
```

<li>Concatenate file- to add more than one file into a single file(We can't edit file but we can concatenate something in existing file)

```bash
$ cat >> file.txt
```

<li>Copy files- To copy the content of file1 into file2

```bash
$ cat file1 file2 > all.txt
```

</ul>

### <li><b>Touch Command</b>

<ul>
<li>Create an empty file

```bash
$ touch file.txt
```

<li>create multiple empty files

```bash
$ touch file1 file2 file3
```

<li>Change all timestamp of a file

```bash
$ touch file.txt
```

<li>Update only access time of a file, modify time of file

```bash
$ touch -a file.txt #change access time
$ touch -m file.txt #change modify time
```

</ul>

#### Time Stamp

<ol>
<li>Access Time(last time when a file was accessed)
<li>Modify Time(last time when file was modified)
<li>Change Time(last time when file metadata was changed)
</ol>

How to check time stamp of file

```bash
$ stat file.txt
```

### <li>Vi Editor
<ul>
<li>A programmer's text editor
<li>It can be used to edit all kinds of plain text, it is specially useful for editing programs. Mainly used for unix programs.

```bash
$ vi file.txt
```

<ul>
<li>:w -- To save
<li>:wq or :x -- To save & quit
<li>:q -- To quit
<li>:q! -- force quit not save
</ul>
</ul>

### <li>Nano editor

```bash
$ nano file.txt
```

For quit - ctrl+X

### <li>How to create a directory

```bash
$ mkdir dir1
# Create directory inside a directory
$ mkdir dir1/dir2/dir3
```

### <li>How to list files and directories

```bash
$ ls
# also hidden files
$ ls -a
```

### <li>Change Directory

```bash
$ cd dir1
# go back parent dir
$ cd ..
```

### <li>Check current directory 

```bash
$ pwd
# print working directory
```

### <li>How to Copy a file

```bash
$ cp file1 file2
$ cp file.txt dir1/dir2/
```

### <li>How to cut and paste file

```bash
$ mv file dir1
```

### <li>How to rename a file

```bash
$ mv file1 file3
```

### <li>How to create hidden file or directory

```bash
$ mkdir .dir1
$ touch .file.txt
```

### <li>How to remove file or directory

```bash
# remove file
$ remove file.txt
# remove the specified dir
$ rmdir dir1
# remove both parents and child dir
$ rmdir -p
# remove all the parents and subdirectories along with verbrox
$ rmdir -pv
# remove even non-empty file and directory
$ rm -rf
# Remove non-empty dir including parents and subdirectory
$ rm -rp
#  remove empty directories
$ rm -r
```

### <li>Some other commands-

```bash
# to see head content of any file
$ head file.txt
# last content of file
$ tail file.txt
# more content
$ more file.txt
# Check history
$ history
```
# Some advance commands

- hostname
- ifconfig, cat /etc/os-release
- yum install httpd
- yum remove httpd
- yum update httpd
- service httpd start
- service httpd status
- chkconfig httpd on
- chkconfig httpd off
- which
- whoami
- echo
- yum list installed

## <li>hostname command

```bash
$ hostname
```

## <li>ifconfig, cat /etc/os-release


```bash
$ ifconfig # check ip address
$ cat /etc/os-release # which os version
```

## <li>yum install httpd
yum is a package, yellowdog updatter modified used to install softwares or uninstall softwares or packages.
httpd will install apache also

```bash
$ yum install httpd
```

## <li>yum remove httpd

```bash
# remove package
$ yum remove httpd

```

## <li>yum update httpd

```bash
# update package
$ yum update httpd
```

## <li>service httpd start

```bash
$ service httpd start

```

## <li>service httpd status

```bash
$ service httpd status

```

## <li>chkconfig httpd on

```bash
# automatically start httpd when open computer
$ chkconfig httpd on
```

## <li>chkconfig httpd off

```bash
$ chkconfig httpd off
```

## <li>which

```bash
# find location of package, installed or not
$ which terraform
```

## <li>whoami


```bash
$ whoami
```

## <li>echo

```bash
# print on screen
$ echo Hello
```

## <li> grep command
- for finding specific content

```sh
# find root in /etc/passwd directory
grep root /etc/passwd
```

## <li> Sort command
- sort file content or data in alphabetical sorting format

```sh
sort filename.txt
```

<hr>