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

## Computer-System Organization
### 1.2.1 Computer-System Operation

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

- **双模式操作允许操作系统保护自身和其他系统组件：** 为了提高系统的安全性和稳定性，操作系统通常运行在两种不同的模式下，以保护自身和其他系统组件。*Dual-mode operation allows OS to protect itself and other system components.*

  - **提供硬件支持以区分至少两种操作模式：** 现代计算机硬件提供了支持双模式操作的机制，通常有两种模式：*Provide hardware support to differentiate between at least two modes of operations.**

    - **User mode - execution done on behalf of a user.*

    - *Monitor mode (also kernel mode, system mode, or privileged mode) - execution done on behalf of operating system.*

### Dual-Mode Operation

