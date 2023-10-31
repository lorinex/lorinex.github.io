---
title: Chapter 1   Introduction
categories:
  - 计算机基础
  - OS
date: 2023-09-12 09:26:35
tags:
---
# Chapter 1 Introduction
Computer System Components
可以计算机系统分成三个基本组成部分：**底层的计算机硬件、中间层的操作系统以及上层的计算机应用程序**，操作系统属于承上启下的中间层，所以它在计算机系统中的地位和作用尤为重要。

> 操作系统是计算机系统中最基本的系统软件；

![截图_20230912091429.png](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230912091429.png)

## What Operating Systems do?
### User view
### System view
- resource allocator
- control program
### Defining Operating Systems
Kernel（nucleus）内核
- OS is the one program **running at all times** on the computer.(all else being system programs and application programs)
- Portion of operating system that is in main memory. 常驻内存
- Contains most-frequently used functions.频繁使用

操作系统是计算机系统中的一个**系统软件**，是一些**程序模块的集合**，它们能以尽量有效、合理的方式**组织和管理计算机的软硬件资源**，合理地**组织计算机的工作流程**，**控制程序的执行**，向用户**提供各种服务功能**

（1）操作系统是安装在计算机硬件之上的一层软件；

（2）操作系统之上可以安装各种应用程序软件；

（3）用户可以通过应用程序软件来间接使用操作系统，也可以直接使用操作系统，但通常都是通过操作系统来最终使用计算机硬件的；

（4）直接使用操作系统的含义就是让用户通过编写程序来调用操作系统提供的系统接口从而进入操作系统。

（5）用户通过系统接口进入操作系统后使用计算机硬件，即用户必须“穿过”操作系统才能使用计算机硬件；

（6）操作系统管理计算机硬件，目的是让用户对计算机硬件的使用更加简便，也更加高效；

## Operating System Feature
操作系统是一种系统软件，操作系统的基本特征包括**并发、共享、虚拟和异步**
### Concurrence
并发是指两个或多个事件在**同一时间间隔**内发生。操作系统的并发性是指计算机系统中同时存在多个运行的程序，因此它具有处理和调度多个程序同时执行的能力。在操作系统中，引入**进程**的目的是使程序能并发执行。
>Q：并发和并行的区别？
>A：
并发：同一时间间隔
并行：同一时刻
基于单处理机的背景，实际上每个时刻仅能有一道程序执行，在一段时间内，宏观上有多道程序在同时执行，微观上这些程序仍然是分时交替执行的 —— 操作系统的并发性是通过分时得以实现的；

而并行性需要有相关硬件的支持，要么是多流水线，要么是多处理机硬件；
### Sharing
共享也就是资源共享，是指系统中的资源可供内存中多个并发执行的进程共同使用，共享可分为以下两种资源共享方式：
**互斥共享方式**：规定在一段时间内只允许一个进程访问资源，将在一段时间内只允许一个进程访问的资源成为临界资源或独占资源；
**同时访问方式**：“同时”通常是指宏观上的，微观上这些进程可能是交替地对资源进行访问，即“分时共享”；
### Virtual
虚拟是指将一个物理上的实体变为若干逻辑上的对应物；
操作系统利用了多种虚拟技术来实现虚拟处理器、虚拟内存和虚拟外部设备等：
利用多道程序设计技术把一个物理上的CPU虚拟为多个逻辑上的CPU（即分时使用一个处理器），称为虚拟处理器；
将用户感受到的存储器称为虚拟存储器（实际存储器我们编程的时候根本接触不到）；
简单来说，操作系统的虚拟技术可以归纳为：时分复用技术（处理器的分时共享）和空分复用技术（虚拟存储器）
### Asynchronism
异步是在多道程序环境下允许多个程序并发执行极有可能导致的进程与时间有关的错误，操作系统可以解决多道程序引发的异步问题，保证多次运行进程后都能获得相同的结果；

## Computer-System Organization
### Computer-System Operation

I/O devices and the CPU can execute concurrently.

Each device controller is in charge of a particular device type. Each device controller has a local buffer.

CPU moves data from/to main memory to/from local buffers.

I/O is from the device to local buffer of controller.

Device controller informs CPU that it has finished its operation by causing an interrupt.

#### Interrupt Timeline

"Interrupt Timeline"（中断时间线）通常指的是计算机系统中的中断事件序列，记录了中断的发生时间和处理时间。在操作系统和计算机体系结构中，中断是一种用于处理异步事件的机制，例如硬件故障、外部设备的输入、定时器事件等。中断允许计算机系统在正常执行的过程中响应和处理这些事件，而不需要等待或轮询。

Interrupt Timeline包括以下关键元素：

1. 中断源：表示引发中断事件的来源，例如硬件故障、I/O设备请求、时钟中断等。
2. 中断请求时间：指中断事件发生的确切时间戳。
3. 中断处理时间：指操作系统或处理器开始处理中断事件的时间戳。这包括了操作系统的中断服务例程或中断处理程序的执行时间。
4. 中断完成时间：指整个中断处理过程完成的时间戳。这包括了中断服务例程的执行以及可能的上下文切换。

中断时间线通常用于性能分析、故障诊断和系统优化。通过分析中断时间线，可以识别中断的来源、频率和处理时间，帮助系统管理员和开发人员识别和解决性能问题以及改善系统的响应时间。中断时间线也有助于了解系统的稳定性和可靠性，因为它们提供了有关系统中断处理的详细信息。

![image-20231020235732529](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231020235732529.png)

### Storage Structure

- Main memory -- only large storage area that the processor can access directly.
- Secondary storage -- extension of main memory that provides large nonvolatile storage capacity.
- Magnetic disks -- rigid metal or glass platters covered with magnetic recording material.
  - Disk surface is logically divided into tracks, which are subdivided into sectors. 
  - The disk controller determines the logical interaction between the device and the computer.
- Magnetic tapes -- used for backup, for storage of infrequently used information.

#### Storage Hierarchy

- Storage systems organized in hierarchy.
  - Speed 
  - Cost 
  - Volatility
- Volatile storage loses its contents when the power to the device is removed.
- Principle of design a computer memory system：
  - uses only as much expensive memory as necessary.
  - provides as much inexpensive, nonvolatile memory as possible.

#### Storage-Device Hierarchy

![image-20231020220210038](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231020220210038.png)

### I/O Structure

- Programmed I/O 

- Interrupt-Driven I/O

  - Synchronous I/O 

  - Asynchronous I/O

- DMA

- I/O通道控制方式

#### I/O operation

- **设备控制器** device controller
  - Local buffer storage
  - A set of special-purpose registers
  - Moving data between device and its local buffer storage.
  - 设备控制器是一种硬件组件，用于管理特定类型的外部设备（如磁盘驱动器、键盘、鼠标等）。它包括一组特殊用途的寄存器，用于控制设备的操作和状态。设备控制器还包括本地缓冲存储，用于在设备和计算机之间传输数据。
- **设备驱动程序** device driver
  - One for each device controller.
  - Presents a uniform interface to the device.
  - 设备驱动程序是用于与设备控制器通信的软件组件。每种设备控制器都有一个相应的设备驱动程序。设备驱动程序提供了一个统一的接口，以便操作系统可以与不同类型的设备进行交互。
- **I/O 操作** I/O operation ：I/O 操作是指计算机系统与外部设备之间的数据传输和交互过程。它包括以下步骤：
  - Device driver loads registers within the controller. 设备驱动程序加载设备控制器内的寄存器，以配置设备的操作。
  - Device controller examines the contents of the registers to determine what action to take. 设备控制器检查寄存器的内容，以确定应采取什么操作。
  - Device controller starts the transfer of data between the device and its local buffer. 设备控制器开始在设备和其本地缓冲存储之间传输数据。
  - Once done, device controller sets its status, or informs the driver via an interrupt. 一旦完成数据传输，设备控制器设置其状态，或者通过中断通知设备驱动程序。
  - Device driver returns control to the OS, with data if 'read'. 设备驱动程序将控制返回给操作系统，如果是“读”操作，可能会将读取的数据一并返回。

#### Modern Computer System

- high-end systems use switch architecture.
- multiple components can talk to other components concurrently, rather than competing for cycles on a shared bus.

在计算机系统中，高端系统采用了一种称为"交换架构"的设计。这种设计允许多个系统组件同时相互通信，而不是竞争共享总线周期。在传统的计算机架构中，多个组件（如CPU、内存、输入/输出设备）必须通过共享总线来访问系统资源，这可能会导致资源争用和性能瓶颈。而交换架构允许各个组件之间以更高效的方式进行通信，从而提高了系统的并发性和性能。

在交换架构中，不同组件之间可以直接建立点对点的连接，而无需通过共享总线进行通信。这种架构通常使用交换网络或高速互连来实现，允许组件之间并行传输数据，提供更大的带宽和更低的延迟。这对于高性能计算、大规模服务器和数据中心等需要处理大量数据并进行高度并发处理的应用非常重要。

<img src="https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231020235530030.png" alt="image-20231020235530030" style="zoom:50%;" />



## Operating-System Structure

### Uniprogramming

Uniprogramming（单一程序系统）是一种计算机操作系统的早期模式，其中在计算机系统上一次只能执行一个程序。这种模式通常是单任务操作系统的基础，它允许计算机执行一个程序，直到该程序完成或发生错误。一旦程序执行完毕，用户可以手动或通过重启系统来加载和执行下一个程序。

![image-20231020235850374](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231020235850374.png)

Uniprogramming的特点包括：

1. **单一任务执行：** 在Uniprogramming系统中，计算机一次只能执行一个程序。这意味着系统在任何给定时间只能处理一个任务。
2. **简单性：** 由于只有一个程序在运行，Uniprogramming系统相对简单，不需要复杂的任务调度或多任务管理机制。
3. **资源共享：** 资源（如内存和CPU时间）完全由当前运行的程序占用，而其他程序必须等待。

Uniprogramming有一些明显的局限性。最显著的是效率问题，因为计算机在执行一个程序时，其他程序无法运行，导致资源利用率低下。此外，用户体验也受到影响，因为用户必须等待一个程序完成后才能运行下一个程序。

这包括：

1. **性能低下：** 由于Uniprogramming一次只能执行一个程序，因此性能有限，计算机不能同时处理多个任务，导致了性能低下的问题。
2. **I/O速度慢：** 输入/输出操作的速度较慢，因为计算机必须等待I/O指令完成后才能继续执行。这意味着I/O操作会阻塞其他计算任务的执行。

为了解决这些性能问题，提出了"离线I/O操作"的解决方案。这意味着某些计算机任务可以在后台或离线状态下进行，而不会干扰主要的计算任务。例如，将作业从磁带加载到内存中，或者进行卡片读取和行打印等操作，都可以在不干扰主计算任务的情况下进行。这样可以提高计算机的资源利用率和性能，减少了等待时间，使计算机更加高效地运行。

### Multiprogramming

1. **保持CPU和I/O设备始终繁忙：** 多道程序设计旨在最大程度地利用计算机资源，以确保CPU和I/O设备在任何给定时间都保持繁忙状态，以提高系统的性能和资源利用率。
2. **组织作业以保持CPU繁忙：** 多道程序设计将不同的作业（包括代码和数据）组织在系统中，以确保CPU始终有一个作业可供执行，从而减少CPU空闲时间。
3. **内存中的作业子集：** 系统中只保留了作业的一个子集（通常是部分作业）在内存中，以便它们可以立即被执行。
4. **作业调度：** 作业调度是多道程序设计的一部分，它选择要在CPU上运行的作业。作业调度算法根据不同的策略选择下一个要执行的作业。
5. **等待I/O时切换作业：** 当一个作业需要等待I/O操作完成时，操作系统会将CPU的控制权切换到另一个可执行的作业，以充分利用CPU时间，而不让CPU处于空闲状态。
6. **两个程序的多道程序设计：** 这是多道程序设计的一种特定情况，其中只有两个程序同时存在在内存中，操作系统可以根据需要在这两个程序之间切换，以确保CPU忙碌。

多道程序设计的核心思想是在系统中同时运行多个作业，以减少计算机资源的浪费，提高性能和响应速度。这种方法允许计算机在一个作业等待I/O操作的时候，执行另一个可执行的作业，从而最大化了资源的利用率。

![image-20231021000623932](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021000623932.png)

Example:

![image-20231021000710663](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021000710663.png)

![image-20231021000744104](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021000744104.png)

### Time sharing systems

1. **时间共享（多任务处理）是一个逻辑扩展：** 在时间共享系统中，CPU频繁切换执行的作业，以使用户能够与每个程序互动，同时运行多个程序，创造出交互式计算环境。Time sharing (multitasking) is a logical extension, in which CPU switches jobs so frequently that users can interact with each program while it is running, creating interactive computing.
   1. **响应时间应该小于1秒：** 时间共享系统旨在实现快速响应，以确保用户在与计算机交互时不会感到明显的延迟。Response time should be less than 1 second.
   2. **每个用户至少有一个在内存中执行的程序 = 进程：** 在时间共享系统中，每个用户可以同时运行一个或多个程序，每个程序被视为一个进程。Each user has at least one program executing in memory = process
   3. **如果有多个作业同时准备运行 = CPU调度：** 当多个进程准备好运行时，操作系统需要选择下一个要分配CPU的进程，这涉及到CPU调度。If several jobs ready to run at the same time = CPU scheduling
   4. **如果进程不能完全放入内存中，通过交换将它们移入和移出以运行 = 交换程序（Swapper）：** 如果内存空间不足以容纳所有进程，操作系统可以将进程移到磁盘上以腾出内存空间，然后将它们重新调入内存。这个过程通常由交换程序（Swapper）负责。If processes don't fit in memory, swapping moves them in and out to run = swapper
   5. **允许执行不完全在内存中的进程 = 虚拟内存：** 虚拟内存是一种技术，允许计算机执行那些不完全加载到内存中的进程，从而提高内存的有效利用率。allows execution of processes not completely in memory = Virtual memory
2. **使用多道程序设计来处理多个交互式作业：** 多道程序设计是一种核心技术，用于同时处理多个交互式作业，以提高资源利用率。Using multiprogramming to handle multiple interactive jobs.
3. **CPU被多个作业多路复用：** CPU的时间在多个作业之间共享，这些作业同时存在于内存和磁盘上。The CPU is multiplexed among several jobs that are kept in memory and on disk
   1. **处理器的时间被多个用户共享：** 在多用户系统中，CPU的时间被多个用户共享，每个用户都可以运行自己的程序。Processor's time is shared among multiple users.
   2. **CPU只分配给内存中的作业：** 只有在内存中的作业才能被分配CPU执行时间。The CPU is allocated to a job only if the job is in memory.
4. **作业可以被交换进出内存到磁盘：** 如果内存不足，作业可以被交换到磁盘上，以腾出内存空间。A job may be swapped in and out of memory to the disk.
5. **多个用户同时共享计算机：** 许多用户可以同时共享一台计算机，每个用户都可以执行自己的程序。Many users share the computer simultaneously.
   1. **提供用户和系统之间的在线通信：** 在线系统允许用户通过终端与系统进行实时通信，执行命令等。On-line communication between the user and the system is provided.
   2. **多用户通过终端同时访问系统：** 多用户系统允许多个用户通过终端同时访问计算机系统。Multiple users simultaneously access the system through terminals.
   3. **当操作系统执行完一个命令后，不再从卡片阅读机中获取下一个“控制语句”，而是从用户键盘获取：** 这表示用户能够直接与系统进行交互，输入命令等。when the operating system finishes the execution of one command, it seeks the next "control statement" not from a card reader, but rather from the user's keyboard.
6. **在线系统必须随时提供给用户访问数据和代码：** 在线系统需要保持随时可用，以便用户能够访问数据和代码。On-line system must be available for users to access data and code.

#### features

1. **I/O例程由系统提供：** 操作系统提供了用于输入/输出（I/O）操作的例程，这些例程用于管理设备和文件的读写，以及与外部设备的交互。
2. **内存管理：** 操作系统必须有效地分配内存给多个作业，以确保它们能够同时在计算机上运行。这包括内存的分配和释放，以及避免内存碎片化。
3. **虚拟内存：** 虚拟内存是一种技术，允许计算机执行那些不完全加载到内存中的作业，从而提高内存的有效利用率。虚拟内存允许操作系统将数据从磁盘交换到内存，以满足程序的需求。
4. **磁盘管理：** 操作系统需要提供文件系统，它管理文件的存储和访问。文件系统通常存储在多个磁盘上，操作系统必须有效地管理这些磁盘和文件的存储。
5. **CPU调度：** 操作系统需要实现CPU调度机制，以便多个作业可以并发执行。这包括作业的选择、作业同步和通信，以及避免死锁等问题。
6. **资源分配：** 操作系统必须合理分配系统资源，包括CPU时间、内存、I/O设备等，以满足不同作业的需求。资源分配是操作系统的一个核心任务，以确保公平性和效率。

## Operating-System Operations

- **现代操作系统是基于中断的** 现代操作系统采用了基于中断的工作方式，这意味着操作系统能够在需要时响应硬件中断或异常。*Modern operating systems are interrupt driven.*
- **硬件引发的中断**：硬件设备（如时钟、硬盘、键盘）可以引发中断，通知操作系统需要采取某种操作或响应。*Interrupt driven by hardware.*
- **软件错误或请求引发异常或陷阱** 操作系统还可以通过软件错误（如除以零）或用户请求（如请求操作系统服务）来引发异常或陷阱，从而触发操作系统的处理。*Software error or request creates exception or trap.*
  - *Division by zero, request for operating system service.*
- **其他进程问题包括无限循环，进程相互修改或修改操作系统：** 操作系统必须应对其他进程可能出现的问题，如无限循环或试图修改其他进程或操作系统的操作。*Other process problems include infinite loop, processes modifying each other or the operating system.*
- **双模式操作允许操作系统保护自身和其他系统组件：** 为了提高系统的安全性和稳定性，操作系统通常运行在两种不同的模式下，以保护自身和其他系统组件。**Dual-mode operation** allows OS to protect itself and other system components.
  - **提供硬件支持以区分至少两种操作模式：** 现代计算机硬件提供了支持双模式操作的机制，通常有两种模式：Provide hardware support to differentiate between at least two modes of operations.
    - User mode - execution done on behalf of a user. 
    - Monitor mode (also kernel mode, system mode, or privileged mode) - execution done on behalf of operating system.
  - **硬件提供的模式位**
    - kernel mode (0) or user mode (1)
    - Provides ability to distinguish when the system is running user code or kernel code.
    - Some instructions are designated as privileged, only executable in kernel mode.
    - System call changes mode to kernel mode, return from call resets it to user mode. 系统调用将模式切换到内核模式，调用返回后将其重置为用户模式。
- When an interrupt or fault occurs hardware switches to kernel mode.
  - ![image-20231021104445208](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021104445208.png)

![image-20231021104643987](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021104643987.png)

无论是操作系统内核代码还是应用程序代码都是装入内存后才执行的，而内核态代码和用户态代码在内存中放置的区域不同：

- 放置内核代码的那一段内存区域是“内核态区域” - 一般进程的kernel代码都是相同的，映射到物理地址同一块区域；
- 放置用户代码的那段内存区域就是“用户态区域”；

![image-20231022110311078](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231022110311078.png)

> 结论：一般来说kernel的代码都在高地址，方便用户态进程从低地址进行处理；

系统调用的“大门”作用体现在让执行在用户态区域的代码不能进入内核态区域（具体来说就是用户态代码不能通过jmp跳转到内核态内存区域中，用户态代码也不能通过mov访问存放在内核态内存中的数据）

![image-20231022110346106](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231022110346106.png)

Q：同样在内存中，如何区分是用户态内存还是内核态内存？

A：内存的使用由操作系统统一管理，操作系统在内存中划定一个区域并设置该区域的特权级，操作系统会及那个自己所在的内存区域的特权级别设置的非常高，将用户程序所在区域的特权级别设置的较低；

在用户程序执行时每访问一次内存都做一次审查。如果要访问的内存区域比自己的特权级高，CPU会拒绝执行，这就实现了操作系统的保护机制；

CPU提供了一种被称为特权环的机制来实现这个特权级检查；

由于很多指令在执行时都要进行这样的特权级检查，为了提高执行效率，应该用计算机硬件即CPU电路来实现这个权限检查，而不是用软件来完成这个检查；

特权级检查涉及两个重要的数值：

- 当前特权级（CPL）：表示当前执行指令的特权级
- 描述符特权级（DPL）：表示一个目标段的特权级，将目标内存区域的特权级信息放在目标段描述符中

双模态的实现

双模态是由操作系统和硬件一起实现的，硬件主要实现下面的功能：

1. 特权指令

   特权指令是指只有在CPU处于内核态的时候才能执行的指令，一个不严谨的说法是，影响其他进程的指令就是特权指令

   ![img](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/0bc27305c86472667f4a80d0a25feadf.png)

   特权指令不能通过普通应用直接执行，必须让操作系统帮助执行，操作系统向用户态应用提供了接口用来执行这些特权指令，这些接口被称为系统调用

   硬件会帮助操作系统检测程序的优先级

2. 内存保护

   内存保护可以保证物理地址和虚拟地址都是隔离的，当然这里硬件需要进行的地址的隔离是指`物理地址`；

   ![img](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/c72f7281820ad3cf5f1936d4ed358d02.png)

   分段法具有不容易实现共享、内存碎片化的问题，因此现代操作系统一般都采用分页的方法；

   分页法涉及的一个很重要的概念是虚拟地址到物理地址的转换（实际上是虚拟页到物理页的映射，我们之后会详细介绍），这个映射过程是操作系统和硬件一起执行的 - 由硬件(CPU中的MMU)来执行虚拟地址到物理地址之间的映射，由软件OS来决定映射的策略，OS和CPU的中间体就是页表，页表存在内存中；

3. 时间片中断

   时间片中断是一种经典的调配策略，是一种让操作系统重新获取CPU控制权的方式

   After timer interrupts, the OS schedules another process (could be the same one being interrupted) to run

4. 上下文切换

   本质上就是进程/线程切换

- Timer to prevent infinite loop/process hogging resources
  - **在特定时间段后设置中断：** 操作系统在特定时间段后设置一个中断，通常使用计时器来实现这一功能。*Set interrupt after specific period.*
  - **操作系统递减计数器：** 操作系统不断递减计数器的值。*OS decrements counter.*
  - **当计数器归零时生成中断：** 当计数器的值减至零时，操作系统生成一个中断，这通常用于触发时间相关的操作。*When counter zero generate an interrupt.*
  - **在调度进程之前设置中断以重新获得控制或终止超过分配时间的程序：** 操作系统在调度新进程之前设置中断，以确保程序在分配的时间内运行，否则会终止或重新获得控制。*Set up before scheduling process to regain control or terminate program that exceeds allotted time.*
  - **修改计时器内容的指令属于特权指令：** 操作系统通常将能够修改计时器内容的指令标记为特权指令，只有在特权模式下才能执行这些操作。*Instructions that modify the content of the timer are privileged.*

## 操作系统的运行环境
CPU通常会执行两种不同性质的程序：

一种是操作系统内核程序（简称管理程序，可以理解为系统软件的组成）；
一种是用户自编程序（简称应用程序，可以理解为应用软件的组成）；
对操作系统而言，前者是后者的管理者，因此“管理程序”会使用一些“应用软件”不允许执行的特权指令如I/O指令、置中断指令等；

具体实现就是将CPU的状态划分为用户态和核心态，使得用户自编程序运行在用户态，操作系统内核程序运行在核心态；

### 操作系统的内核
一些与硬件关联比较密切的模块如时钟管理、中断处理等；其次是运行频率较高的程序如进程管理、设备管理等，这两部分内容构成了操作系统的内核 —— 这部分的指令操作工作在核心态，内核是计算机上配置的底层软件；

内核是不能直接使用的，如Linux内核，必须包装（添加C库、C编译器、绘图软件等应用软件）之后才能给用户使用；

不同的系统对内核的定义有区别，大多数操作系统内核包括：

时钟管理：时序发生器是计算机最关键的设备，系统管理的方方面面都依赖于时钟；
中断机制：现代操作系统是靠中断驱动的软件；
原语：原语是指一系列特殊的程序，这种程序的运行具有原子性 —— 定义原语最直接的方法就是关闭中断，使其所有动作不可分割地完成后再打开中断；
操作系统管理和处理数据结构：为了对系统中的数据结构实现有效的管理，系统（本博客中的系统默认情况下都是指操作系统）需要一些基本的操作：
进程管理
存储器管理
设备管理
Q：程序和软件的区别是什么？

A：简单来说，软件=程序+文档=数据结构+算法+文档

### 中断和异常
上面我们提到，操作系统引入了核心态和用户态两种工作状态，这里我们讨论这两种状态如何切换：

中断是唯一让用户态切换到核心态的方式；
核心态转换为用户态只需要修改程序状态字PSW的标志位（通过执行特权指令来修改）；
当用户程序需要使用一些核心态的功能时，就需要借助一些建立在核心态的“门”以便实现从用户态进入核心态，实际操作中唯一能够进入这些gate的途径就是通过中断或异常；

发生中断或异常时，运行用户态的CPU会立即进入核心态，这是通过硬件实现的（一位寄存器0表示核心态、1表示用户态）

#### 中断的定义
中断也称外中断（外设请求、人工干预），指来自CPU执行指令以外的事件发生（I/O中断、时钟中断），这一类中断通常是与当前指令执行无关的事件；
异常也称内中断（例外、陷入），指来自CPU执行指令内部的事件（程序非法操作码、地址越界），对异常的处理通常依赖当前程序的运行现场，且异常不能被屏蔽，一旦出现应当立即处理；
#### 中断处理过程
中断（默认情况下所说的中断都是指外中断）处理流程大致如下：

关中断；
保存断点；
引出中断服务程序；
保存现场和屏蔽字；
开中断；
执行中断服务程序；
关中断；
恢复现场和屏蔽字；
开中断、中断返回；
上述流程我们在 计组期末复习笔记 - Tintoki_blog (gintoki-jpg.github.io) 有详细介绍；

## 操作系统的体系结构
操作系统在核心态应当为应用程序提供什么样的公共服务？主要有两种主要的体系结构解答：大内核和微内核；

### 大内核和微内核
大内核系统将操作系统的主要功能模块都作为一个紧密联系的整体运行在核心态，从而为应用提供高性能的系统服务；
由于复杂的交互关系使得层次之间的界限极其模糊，定义清晰的层次间接口非常困难；
微内核将内核中最基本的功能保留在内核，将那些不需要在核心态执行的功能移到用户态执行；
微内核结构最大的问题是性能问题，需要频繁在核心态与用户态之间进行切换；

## 四个基本管理模块

一个基本操作系统包含如下四个基本管理模块：进程管理、内存管理、I/O管理以及文件系统

### I/O管理

### Process Management

- **进程是正在执行的程序，是系统内的工作单元。可以分配给处理器并在其上执行的实体。**
  - *A process is a program in execution, is the unit of work within the system. The entity that can be assigned to and executed on a processor.*
- **进程需要资源来完成其任务。**
  - *Process needs resources to accomplish its task.*
- **进程终止需要回收可重用资源。**
  - *Process termination requires reclamation of any reusable resources.*
- **单线程进程具有一个程序计数器，指定下一条要执行的指令位置。**
  - *A single-threaded process has one program counter specifying the location of the next instruction to execute.*
- **多线程进程每个线程都有一个程序计数器。**
  - *A multi-threaded process has one program counter per thread.*
- **系统在一个或多个CPU上并发运行许多进程。**
  - *The system has many processes running concurrently on one or more CPUs.*
- **通过在进程/线程之间复用CPU来实现。**
  - *By multiplexing the CPU among the processes/threads.*

操作系统在与进程管理相关的活动中负责以下任务：

- 创建和删除用户和系统进程
- 挂起和恢复进程
- 提供进程同步的机制
- 提供进程通信的机制
- 提供死锁处理的机制

### Memory Management

- **对于现代计算机系统的运作至关重要。**
  - All data should be in memory before and after processing.
  - All instructions should be in memory in order to execute.
- **CPU能够直接寻址和访问的唯一大型存储设备。**
- **内存管理确定内存中存在什么内容，提高CPU利用率和计算机响应用户的速度。**
- **内存管理活动**
  - 跟踪当前正在使用内存的部分以及使用者。
  - 决定将哪些进程（或其中的部分）和数据移入和移出内存。
  - 根据需要分配和释放内存空间。

### Storage Management

- 操作系统提供了关于信息存储的统一逻辑视图。 

  - 从物理属性抽象出来，定义了逻辑存储单元 - 文件。 

  - 每种存储介质都由设备（如磁盘驱动器、磁带驱动器）控制。 
    - 不同的属性包括访问速度、容量、数据传输速率、访问方法（顺序或随机）。

#### File-System Management

文件通常组织成目录。 大多数系统具有**访问控制**来确定谁可以访问什么。 

操作系统的活动包括：

- 创建和删除文件
- 创建和删除目录以组织文件
- 支持用于操作文件和目录的基本操作
- 将文件映射到辅助存储设备
- 备份文件到稳定的（非易失性）存储介质。

#### Mass-Storage Management

通常，磁盘用于存储无法容纳在主内存中的数据，或者需要在较长时间内保留的数据。

操作系统在磁盘管理方面的活动包括：

- 空闲空间管理
- 存储分配
- 磁盘调度

计算机操作的整体速度高度依赖于磁盘子系统及其算法。

某些存储设备不必很快，三级存储包括光盘、磁带。 存储介质可能是WORM（只写一次，多次读取）或RW（可读写）。 虽然对系统性能不是至关重要的，但仍然需要进行管理，包括挂载和卸载、分配和释放，以及将数据从二级存储迁移到三级存储。

## Caching

- 缓存是计算机系统中的一个重要概念，广泛应用于多个计算机层次（包括硬件、操作系统和软件）
- 其目的是将正在使用的信息从较慢的存储复制到较快的存储中以便临时使用。
- 缓存是一种将信息复制到更快的存储系统中的方法，主内存通常被视为辅助存储的最后一级缓存。
- 在访问数据之前，系统会首先检查缓存，以确定信息是否存在。
  - 如果信息已经存在于缓存中，那么可以直接从缓存中获取，从而提高数据处理速度。
  - 如果信息不存在于缓存中，系统会将数据复制到缓存并在其中使用。
- 然而，由于缓存的容量通常小于被缓存的存储容量，因此缓存管理策略是一个重要的设计问题。设计决策还包括确定缓存的大小和数据替换策略。
- 缓存是计算机系统中的一个重要原则，它被广泛应用在多个层次，包括硬件、操作系统和软件。

- Disk Cache
  - 主内存的一部分被用作缓冲区，用于临时存储磁盘数据。
  - 磁盘写入会进行聚类
  - 有些被写出的数据可能会再次被引用。
  - 这些数据可以从软件缓存中快速检索，而不是从磁盘上缓慢地读取。

- Cache Memory
  - 对操作系统不可见
  - 提高内存速度
  - 处理器速度比内存速度快

## Protection and Security

- **保护（Protection）：** 保护是指控制进程或用户对操作系统定义的资源访问的任何机制。它确保系统资源的安全使用和访问控制。
- **安全（Security）：** 安全是指系统抵御内部和外部攻击的防御措施。这包括防止拒绝服务、蠕虫、病毒、身份盗窃和服务盗用等多种安全威胁。
- **访问控制（Access Control）：** 访问控制用于规定用户对系统的访问权限，以确定用户能够执行什么操作。
- **信息流控制（Information Flow Control）：** 信息流控制用于规定数据在系统内的流动以及传递给用户的方式。
- **认证（Certification）：** 认证是指证明访问控制和信息流控制是否按照规范执行的过程。
- **系统通常首先区分用户，以确定谁可以执行什么操作。**
- **用户标识（用户ID、安全ID）包括用户名和相关编号，每个用户有一个。**
- **然后，用户ID与该用户的所有文件和进程相关联，以确定访问控制。**
- **组标识符（组ID）允许定义一组用户，并进行管理，然后也与每个进程和文件相关联。**
- **特权提升允许用户切换到具有更多权限的有效ID。**

## 操作系统的接口
操作系统系统了一系列接口为用户服务，主要分为两类：

命令接口：用户利用这些操作命令来组织和控制作业的执行，使用命令接口进行作业控制的主要方式有两种，按照控制的方式可将命令接口分为两类
脱机控制接口：脱机命令接口又称为批处理命令接口，适用于批处理系统，批处理类比于雇主将工人需要做的事写在清单上，工人按照清单命令逐条完成这些事；
联机控制接口：又称为交互式命令接口，适用于分时或实时系统的接口，雇主说一句话工人做一件事并反馈，强调交互性；
程序接口：编程人员使用它们来请求操作系统服务，实际上就是系统调用；
