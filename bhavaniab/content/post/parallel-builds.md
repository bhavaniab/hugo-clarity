+++
author = "Bhavani Anantapur Bache"
title = "Significance of parallel builds"
date = "2012-04-11"
description = "Significance of parallel builds"
featured = true
tags = [
    "Programming",
    "Technology",
    "Linux",
]
categories = [
    "Programming",
    "Linux",
    "Technology"
]
series = ["Software Engineering"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/linux.png"
+++

Parallel builds play a significant role for building projects that require huge RAM to compile.
The best example is chromium build. Suppose the chrome is built by

`````bash
make chrome
`````
Sometimes, we run into linker errors.

`````bash
collect2: ld terminated with signal 6 Aborted terminate called 
after throwing an  instance of 'std::bad_alloc'
`````

Building the code by the command

`````bash
make -j6 chrome
`````
resolves the liker errors and the build goes through.

<h3>What really happens during a parallel build?</h3>

To do a parallel build, add -jX where X is the number of make processes to start-up. This is useful for multiple-core machines. Unlike the normal procedural languages like C,C++,PERL etc which execute the commands step by step, make is a utility which does not specify particular order for operations. Each step is taken when necessary dependencies are completed. For multiprocessor machines, the advantage of parallel build is much more because additional CPUs can perform multiple compilations faster than a single CPU.
Parallel builds can be faster even in uniprocessor environment, because make files perform tasks that can be carried out in parallel, such as such as compiling C source to object files and  creating libraries out of object files. Based on the dependencies of header files, multiple source files can be compiled in parallel.

<h3>How does parallel build resolve the linker errors?</h3>
The kernel is a piece of software that provides a layer between the hardware and the application programs running on a computer. All the other applications that we have with Linux distribution such as the GNOME window manager, web browsers, etc. are just applications that happen to run on Linux kernel and are not part of the operating system. The kernel makes its services available to the application programs that run on it through a large collection of entry points,known as system calls. From a programmer’s viewpoint,
these look just like ordinary function calls, although in reality a system call involves a distinct switch in the operating mode of the processor from user space to kernel space.
The kernel has to work hard to maintain this same abstraction when the filesystem itself might be stored in any of several formats, on local storage devices such as hard disks, CDs or USB memory sticks.
Swap space in Linux is used when the amount of physical memory (RAM) is full. If the system needs more memory resources and the RAM is full, inactive pages in memory are moved to the swap space. Swap space is located on hard drives, which have a slower access time than physical memory. Each process runs under the illusion that it has an address space (a valid range of memory addresses) to call its own. In reality, it’s sharing the physical memory of the computer with many other processes, and if the system is running low on memory, some of its address space may even be parked out on the disk in the swap area. The parallel builds run as a part of different processes and utilize the physical memory space more efficiently, thus preventing the linker errors during parallel build.
