---
layout: single
title: What is systemd and Why Should I Care?
date: 2014-10-21 12:00:00
tags: [unix, Linux, OS]
---

While systemd has been around for a few years, it is gaining more and more
attention as more Linux distributions switch to it. Lots of developers are
extremely opposed to systemd as it goes against the Unix philosophy. Most Linux
users, however, have no idea what it is or why it is a good or bad thing. Here
is a basic overview of init systems and systemd. 

## What is it?

When you boot a Linux machine, once the kernel has finished getting itself ready a new 
process is launched which is termed the init process. This process is a
[daemon](https://en.wikipedia.org/wiki/Daemon_%28computing%29) and its responsibilities 
are to load and manage the other daemons on the system. By this, I mean that
once your kernel has booted, the init process gets your computer into the much
more usable, familiar state that you are used to. Essentially the init process
establishes and operates the entire user space. 

Some of the more typical tasks which fall to the init process are starting any
user services (That game server you like to run on start up? Probably is launched
through the init process.), loading a desktop manager such as GNOME or KDE, and
even checking and mounting file systems. 

The init process is generally started with what is known as a run level. This
level defines what should and should not be loaded (and unloaded). Each
operating system uses different run levels for different things but they tend to
have some things in common. Generally there are 7 of these run levels, numbered
0-6 and some are fairly common. Some of the more common ones are referred to by
name since they don't always have the same run level. Examples of this are
"single user mode", "multi user mode without network services", and "system 
reboot". While there is no guarantee, of consistency between systems, run level 
0 is generally a halt which shuts down the system and level 6 is generally
reboot. 

When the init system is loaded with a run level, it executes all the processes
for that run level and that run level only. For instance if it starts with run
level 3 (which is a fairly typical selection for normal use), it will load all
the regular user processes including your desktop manager and networking. The
scripts and instructions for all other run levels are ignored. When
you decide to shutdown or reboot your machine you must change the run level.
This change in run level is achieved through a separate binary. Upon the change,
the init system will execute the scripts and instructions for that run level.
These run levels are generally only seen in System-V style init methods (this
will be explained later). 

The idea of this init process was originally found in Unix, with the most well
known model coming from System-V. The init method here really set the foundation
of having one daemon manage and load all of the others. This method involved
executing a series of scripts in turn when initially launched or when the run
level changed. This method is used today in many Linux distributions which use
System-V style init systems. So lets look at the various current options for
Linux. 

### Init
Often just named init, also known as sysvinit, this was one of the most popular
init systems found on Linux. It's main configuration file is found in 
`/etc/inittab` which is also where the run level is specified for launch. This
init method follows the principle outlined above, where a scripts are executed
in series until the system is ready. These scripts tend be found in directories
named `/etc/rc.******`. Once all processes are loaded it waits for one of three 
events: The parent process dies, a power failure signal or a request via 
`/sbin/telinit` to change the run level (for example, on shutdown).

### upstart
Upstart is a replacement for sysvinit. It is fairly well known due to its use in
Ubuntu. Created by former Canonical engineer [Scott James
Remnant](https://en.wikipedia.org/wiki/Scott_James_Remnant), it was designed to
try and remove as much of the serial architecture of sysvinit as possible. In
sysvinit, if something is loaded from HDD then the entire system had to wait for
this to finish. IO takes a huge amount of time relatively speaking, and so the
process was rather wasteful. Upstart is asynchronous, which allows it to load
multiple things at the same time, reducing the time spent waiting on IO. Further
more, it is also able to respond to events which allows it to gracefully handle
actions such as inserting a USB drive. One of the greatest strengths of upstart
is that it is also backwards compatible with sysvinit scripts. 

### systemd
Just like upstart, systemd is a replacement for sysvinit, but includes a lot 
more software with it such as systemd, journald and logind. It was also designed 
to be more efficient than sysvinit, but works slightly differently. The idea is
that you can clearly express the dependencies on different daemons and that way
systemd can solve the problem and load the various daemons concurrently as
efficiently as possible. Instead of the standard scripts used for sysvinit and
upstart, systemd configuration files are declarative plain text files. 


So basically, what we have seen is that each of these pieces of software
successfully does its job of initialising and monitoring daemons. The difference
is that upstart and systemd parallelise this task and so they load everything
much faster than sysvinit. 


## Why should I care?

So this is where things get more complicated. Basically, the Unix philosophy is
that each piece of software should do one thing and do it well. The idea means
that we create a bunch of extremely powerful, modular components which can be
stacked together to solve problems. This is most apparent with simple tools used
on the command line where you pipe output from one program into another. This
applies throughout Linux.

Systemd goes against this, according to some people, as it tries to do too much.
Systemd refers not just to an init process, but an entire software bundle along
with the init process. These other applications are used for various things from
new login managers to logging managers. However, while it may be bundled with 
lots of other functionality, this functionality is all separated into different 
binaries which interact with each other, thus keeping the modularity.

So does it really matter? Well that depends. While systemd has achieved its goal 
of being a better init system, it has continued to grow and take over functions 
which should be left to other software where that is the software's only 
responsibility (remember that Unix philosophy). This presents a some problems. 

The first problem is that it means that systemd is a single point of failure. 
The entire user space is dependent on it, and without it, the machine is 
useless to most people. If there is a bug in the code, its required position
due to many dependencies, will make it a prime target for attackers. The Linux
kernel is already a huge target so why give attackers another? 

The second problem is the fact that systemd is very tightly coupled with the 
Linux kernel and therefore it is very easy to introduce incompatibilities. 
Software which runs in user space should have minimal dependencies on the 
kernel. With these dependencies, it makes it very difficult to ensure that 
systems can remain stable as a software update is much more likely to require
a kernel update. 

The third problem stems from the tight coupling with the Linux kernel. It means
that systemd itself can't be ported to other platforms. You might think that
this isn't a huge issue, and normally you would be right. However, lots of
software is starting to list systemd as a dependency. If it has systemd as a
dependency then it means that software also can only run on Linux. As an example
of how serious this is, both KDE and GNOME have a dependency on systemd. For the
time being, this is only for "non-basic functionality", but that means things
like power management, which one might argue, is pretty important. It seems to
only be a matter of time before the dependencies grow. 

The list goes on. Personally, the biggest warning sign to me is how they handle
logging. No longer are text files used for logging, instead a binary format is
used. Furthermore, this format does not have ACID compliant transactions, which
means that your logs can easily become corrupted. When this happens, the software
simple ignores it, rotates the file out, and then continues on as though nothing
happened. The best part of this one is that the developers have stated that it
is unlikely that this will be fixed in the near future.[^journald] If such a
critical component can't get logging right, what chance do they have for
everything else?

I set out when writing this post to try and give a balanced view on systemd and
I have to admit that I have completely failed so far. Almost everything I've 
said so far has been negative. So let's look at some of the good parts of
systemd:

* It has a much more efficient init process than sysvinit. 
* It tracks processes using cgroups rather than PIDs which helps with process
management. 
* In theory, it is easier to launch a daemon as you no longer have to write
a bash script.
* The support for D-Bus and sockets makes inter-daemon communication easier.


Basically, you have to make your own decision, but hopefully you can see that
this is an extremely important issue and you most definitely should care. 



[^journald]:<https://bugs.freedesktop.org/show_bug.cgi?id=64116>
