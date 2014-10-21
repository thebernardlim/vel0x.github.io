---
layout: post
title: What is systemd and why should I care?
date: 2014-10-21 12:00:00
tags: [unix, linux, OS]
---

##What is it?

* All linux machines use an init process
* The init process is responsible for loading all userspace functions once the
kernel is running. It also generally monitors and manages these functions once
they have been loaded. It essentially establishes and operates the entire user space.
** Checking and mounting file systems
** Starting user services
** Loading desktop environments
* Init is executed with a run level which defines what should and should not be
loaded. This applies to System V-style initialisation only. 
** Generally 7 run levels:
*** Single user mode
*** Multi user without network services
*** Multi user with network services
*** System shutdown
*** System reboot
** Varies from OS to OS but generally follows patterns such as 0 being a half.
Low levels generally are for repairs or maintenance and don't have networking.
** Check here: https://en.wikipedia.org/wiki/Runlevel#Linux
* The standard init process came from System V which was Unix but most linux distros
had a compatible version which was called "System V-style" init.
* Init
** Also known as sysvinit
** Main configuration is in `/etc/inittab` which is where the run level is
specified.
** Loads things sequentially in `/etc/rc.....`
** Once all processes are loaded it waits for one of three events. The parent
process dies, a power failure signal or a request via `/sbin/telinit` to change
the run level (for example, on shutdown).
* upstart
** A fairly well know init process which is used in Ubuntu.
** Designed to deal with events rather than having to wait and do things
synchronously. (For example, connecting a USD pen while the machine is running)
** Backwards compatible with sysvinit
* systemd
** Is a replacement for sysvinit, but includes a lot more software with it.
*** Examples being systemd, journald, logind, networkd, etc.
** Designed to be more efficient.
*** Make it easier to express dependencies so more can be loaded concurrently
** Uses declarative configuration files instead of scripts for launching
daemons.
** Gives daemons access to unix domain sockets and D-Bus
** Also includes a job scheduler similar to cron named "systemd Calendar Timers"
and an event logging subsystem called journald (which uses binary files).
* So basically init, upstart and systemd all do the same thing and that is load
userspace daemons.
* Systemd includes a lot of other stuff with it.
* As an init system, systemd and upstart are better because they can parallelise
the loading which makes it faster.

##Why should I care?

So this is where things get more complicated. Basically, the Unix philosophy is
that each piece of software should do one thing and do it well. The idea means
that we create a bunch of extremely powerful, modular components which can be
stacked together to solve problems. This is most apparent with simple tools used
on the command line where you pipe output from one program into another. This
applies throughout linux.

Systemd goes against this, according to some people, as it tries to do too much.
Systemd refers to both the init process and the entire software bundle which
comes with it. However, while it may bundle lots of other functionality, this
functionality is all separated into different binaries which interact with each
other, thus keeping the modularity.

So does it really matter? Well that depends. While systemd has achieved its goal 
of being a better init system, it has continued to grow and take over functions 
which should be left to other software where that is the softwares only 
responsibility (remember that Unix philosophy). This presents a some problems. 

The first problem is that it means that systemd is a single point of failure. 
The entire userspace is dependent on it, and without it, the machine is 
useless to most people. If there is a bug in the code, its required position
due to many dependencies, will make it a prime target for attackers. The linux
kernel is already a huge target so why give attackers another? 

The second problem is the fact that systemd is very tightly coupled with the 
linux kernel and therefore it is very easy to introduce incompatibilties. 
Software which runs in userspace should have minimal dependencies on the 
kernal. With these dependencies, it makes it very difficult to ensure that 
systems can remain stable as a software update is much more likely to require
a kernel update. 

The list goes on. Personally, the biggest warning sign to me is how they handle
logging. No longer are text files used for logging, instead a binary format is
used. Furthermore, this format does not have ACID compliant transactions, which
means that your logs can easily become corruped. When this happens, the software
simple ignores it, rotates the file out, and then continues on as though nothing
happened. The best part of this one is that the developers have stated that it
is unlikely that this will be fixed in the near future.[^journald]

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
