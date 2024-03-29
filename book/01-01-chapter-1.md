# Chapter 1 : What is Linux ? {#chapter-1}

Usually, when we talk about Linux, we talk about a Linux Distribution. But actually, Linux itself is just one component of an operating system (ok, a very central one). Linux is a kernel[^kernel], a core computer program that manages the entire operating system. It's the interface between the hardware and the software : CPU access, Memory access, Filesystem access, devices access, all of this is made through the kernel. By misuse of langage, we assimilate Linux to an operating system but it's not exactly accurate because the OS is the integration of the Linux Kernel and various other softwares (we will detail this in the "What is a Linux Distribution" part).

This distinction is not specific to Linux, even in Microsoft's world you have it, but it's less confusing as the software is managed by a unique company unlike a Linux Distribution which integrates various projects together. Actually, Microsoft Windows 11 or 2022 are both operating systems based on the Windows NT Kernel[^winntkernel], the core program of Windows' architecture. In Apple's world, the kernel running behind every systems edited by the company is named XNU[^XNU]. Since these two companies are providing a comprehensive product, these details are less advertised.

Linux is distributed under a free and open-source license, the General Public License 2.0, allowing anybody to use it, modify it, distribute it, and integrate it in other works. This is one of the many reasons that made the Linux kernel very popular for Servers systems, embedded systems, mobile devices (Android[^android]) is based on Linux too), and even mainframes and supercomputers. If Linux was born for Intel x86 architecture at the beginning, the kernel now supports a multitude of platforms : ARM, PowerPC, etc.

## Unix and Linux history {#chapter-1-unix-and-linux-history}

In August 2021, Linux celebrated its 30th birthday[^30thbirthday]. In this article, in French (I can translate it later if requested), I've made a little history about the origins of the Linux Kernel. Linux is what we commonly call a "Unix-Like" or a "POSIX compatible" kernel. But, what are "Unix" and "POSIX" ?

Back in the mid-1960, the MIT, Bell Labs and General Electric were developing an operating system for a mainframe computer. This OS, named *Multics[^multics]*, was innovative (with the idea of sharing the computer resources for multiple users, hierarchical filesystem, etc) but very difficult to develop and the project was eventually abandoned. The experimentation was reimplemented into a smaller scale project for a single-tasking system, *Unics*, later spelled *Unix[^unix]* for reasons that seems to have been forgotten. The first version was released in 1970 after a development started in 1969, a date well known in the Information Technologies sector because it's what we call the Unix epoch[^unixepoch], a date and time representation using the number or seconds elapsed since the 1st January 1970 00:00:00 UTC. The Unix epoch timestamp is widely used in programmation to manipulate date and time. Also another very noticeable fact related to Unix is the creation of the C programming language[^cprogramminglanguage] at the same moment. Unix itself has been rewritten in C with the release of its version 4 in 1973.

Unix has been later selected as the basis for a new operating system standard written in 1980 by the IEEE Computer society, the Portable Operating System Interface[^posix], or POSIX. POSIX is a standard describing both the system and user-level application programming interfaces (APIs) with command line shells and various utilities. The reason of this standard creation is mainly because Unix has been progressively copied, adapted and fragmented into various Unix-like systems developed by several companies that were often mutually-incompatible (HP-UX from HP, SunOS/Solaris from Sun Microsystems, AIX from IBM, etc).

Today, the Single Unix Specification is still evolving and the operating system based on the POSIX standard can be certified for using the commercial brand Unix. This is why Linux is described as a "Unix-like" system, because it was developed as a compatible POSIX kernel, but is not certified.

Linux itself is inspired by Minix[^minix], another Unix-like system used by students. Created in August 1991 by Linus Torvalds, a 21 years old Finnish student, the source code of Linux has been submitted to the Minix mailing-list community. A couple of months later, the first "official" version of Linux able to run Bash, GCC, and some other GNU tools, was released. Despite its various limitations at this time, Linux has been adopted by numerous developers and later integrated by the GNU Project for its free and open source operating system, the GNU Operating System. The GNU Project was also developing its own kernel, GNU Hurd, since the mid-1980s.

In 1992, Linus adopts the GPLv2 license for the Linux source code and distributes it publicly, including the drivers code, in contrast of Unix which was mainly a proprietary system. Linux would eventually support the POSIX APIs and being able to run software and applications developed for Unix. The version 0.92 of Linux was able to run the X Window System, allowing to start graphic applications. The first suitable production version of Linux, tagged 1.0.0, has been released in 1994 containing 176 250 lines of code.

Nowadays, Linux is present in various embedded, servers, mainframes, supercomputers, mobile systems, and is powering a big part of the Cloud Computing.

## What is a Linux Distribution {#chapter-1-what-is-a-linux-distribution}

As we said at the beginning, Linux itself is a kernel. Alone, its capabilities to interact with the user are limited because it needs other programs to perform tasks. In order to being useful, Linux needs a bundle of softwares and applications to use the computing resources distributed with it : that's the Linux Distribution.

A Linux Distribution (or Distro) is an operating system including a packaged Linux Kernel and a software collection. These packages are often maintained by a package manager, a tool that automates the installation, update, and removal of softwares packages from the system… Including the Kernel itself since it's new versions are also managed by the package manager.

Here is a simplified diagram about the architecture of a Linux Distribution with the relation with the components.

![The common architecture of a Linux distribution.](img/linux-distro-architecture.png)

At the beginning, Linux was only distributed as source code to compile. Later, a set of two downloadable floppy disks was provided with one containing a bootable compiled Linux kernel, and a second with a set of the GNU utilities for setting up a filesystem. These floppy disks images are the very first Linux Distributions.

Today, the oldest still maintained Linux Distributions are Slackware[^slackware] and Debian[^debian]. And don't imagine it took time to commercialize Linux, the first commercial distribution was Yggdrasil Linux/GNU/X released in December 1992 !

Yggdrasil Linux's full name is a good example about a controversy[^controversy] running since ... A lot of time about how to name a Linux Distribution. Because Linux is usually distributed with a big part of the GNU tools developed by the Free Software Foundation's GNU Project[^gnuproject], its founder Richard Stallman proposed the term "GNU/Linux" to describe a Linux operating system. It's a very old controversy because not everyone accepted the idea as it would create an unnecessary confusion for the user and "Linux" became *de facto* the common name for a Linux-based OS. And today it's still matter to debate. I won't go deeper in this topic, just remember that you can see this naming convention along with "Linux" and "Linux Distribution".

So, we have the basis, now let's drop some weird names you may had already seen : Slackware, Debian, Ubuntu, Red Hat Enterprise Linux, Fedora, Arch, Gentoo, etc, all of them are Linux Distributions. And Linux Distros, there are a lot of them ... I mean, a freaking bazillions lot of them[^distrolist].

To remain simple, a Linux Distribution is a software project that integrates the Linux Kernel, usually shipped with the GNU tool set (but there are some alternatives), a boot loader (optional too), and various software packages like a Window Manager for graphical Desktop environment, or other applications like a Web Browser or an office software suite (LibreOffice, etc).

They are a complete operating system defined by a philosophy and an architecture which are usually their own. These projects have often a specific goals or philosophies : being stable and reliable, providing only Free software packages (like Debian), being user-friendly (like Ubuntu or Manjaro), highly customizable targeting experimented users (like Arch or Gentoo), integrating cutting-edge technology (Fedora), security oriented (like Kali Linux), network oriented (like Pfsense), virtualization oriented (ProxMox), lightweight (Alpine Linux), etc.

[^30thbirthday]: Linux a 30 ans - https://blog.zedas.fr/posts/linux-a-30-ans/
[^kernel]: Kernel (Operating System) https://en.wikipedia.org/wiki/Kernel_(operating_system)
[^winntkernel]: Architecture of Windows NT -  https://en.wikipedia.org/wiki/Architecture_of_Windows_NT
[^XNU]: XNU https://en.wikipedia.org/wiki/XNU
[^android]: Android (Operating System) https://en.wikipedia.org/wiki/Android_(operating_system
[^multics]: Multics https://en.wikipedia.org/wiki/Multics
[^unix]: Unix https://en.wikipedia.org/wiki/Unix
[^unixepoch]: Unix Time https://en.wikipedia.org/wiki/Unix_time
[^cprogramminglanguage]: C (Programming Language) https://en.wikipedia.org/wiki/C_(programming_language
[^posix]: POSIX https://en.wikipedia.org/wiki/POSIX
[^minix]: Minix https://en.wikipedia.org/wiki/Minix
[^slackware]: Slackware https://en.wikipedia.org/wiki/Slackware
[^debian]: Debian Linux https://en.wikipedia.org/wiki/Debian
[^controversy]: Linux naming controversy https://en.wikipedia.org/wiki/GNU/Linux_naming_controversy
[^gnuproject]: The GNU Project https://en.wikipedia.org/wiki/GNU_Project
[^distrolist]: Linux Distributions timeline and list https://upload.wikimedia.org/wikipedia/commons/b/b5/Linux_Distribution_Timeline_21_10_2021.svg