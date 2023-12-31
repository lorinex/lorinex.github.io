---
title: Chapter 2 Operating-System Structures
categories:
  - 计算机基础
  - OS
date: 2023-10-21 20:53:53
tags:
---

# Operating-System Structures

## Operating System Services

操作系统提供了一个程序执行的环境，并向程序和用户提供各种服务。

![image-20231021220150613](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021220150613.png)

操作系统提供一组服务，旨在满足用户的需求，包括以下功能：

1. **用户界面 (UI)：** 操作系统提供不同类型的用户界面，以便用户与计算机系统交互。这包括命令行界面 (CLI)，批处理界面，图形用户界面 (GUI) 等。用户可以通过这些界面执行命令、操作文件和访问系统功能。
2. **程序执行：** 操作系统负责加载程序到内存并执行它。它可以正常或异常地终止程序的执行，以指示是否发生了错误。
3. **I/O 操作：** 操作系统管理输入/输出操作，允许用户访问磁盘上的文件，输出到打印机或屏幕等。这包括文件的读写、目录的创建和删除、文件搜索、权限管理等。
4. **文件系统操作：** 操作系统支持对文件和目录的操作，包括读取和写入文件，创建和删除文件和目录，搜索文件，列出文件信息以及权限管理。
5. **通信：** 进程可以在同一台计算机上或在网络上的不同计算机之间交换信息。这可以通过共享内存、消息传递等方式实现。
6. **错误检测：** 操作系统能够检测硬件、I/O 设备或用户程序中可能发生的错误。对于每种类型的错误，操作系统应采取适当的措施以确保计算的正确性和一致性。
7. **调试功能：** 操作系统提供调试工具，以帮助用户和程序员高效地使用系统。这些工具可以用于识别和解决程序中的错误，从而提高系统的可靠性和稳定性。

操作系统还具备一组功能，旨在通过资源共享来确保系统自身的高效运行，这包括以下方面：

1. **资源分配：** 当多个用户或多个作业同时运行时，必须为它们分配资源。不同类型的资源需要不同的分配策略，例如 CPU 时间、主内存、文件存储等。某些资源可能需要特殊的分配代码，而其他资源可能使用通用的请求和释放代码，例如 I/O 设备。
2. **记账（Accounting）：** 记账是跟踪用户使用了多少以及什么类型的计算机资源的过程。这有助于了解资源的使用情况，以便进行资源管理和计费。
3. **保护和安全性：** 多用户或网络化计算机系统中存储的信息的所有者可能希望控制该信息的使用。同时运行的进程不应相互干扰。保护涉及确保对系统资源的所有访问都受到控制。系统的安全性要求用户进行身份验证，并扩展到保护外部 I/O 设备免受非法访问尝试的攻击。如果要保护和确保系统的安全性，必须在整个系统中采取预防措施。

这些功能有助于有效管理系统资源、跟踪资源使用情况、确保资源的安全性和一致性。保护和安全性是操作系统的关键任务之一，以确保系统的可靠性和用户数据的保密性。

## User Operating System Interface


### Command Interpreter (CLI)

命令解释程序（Command Interpreter）是用户与操作系统之间的接口。它负责解释用户输入的命令并将其传达给操作系统执行。命令解释程序可以以不同的方式实现，取决于操作系统的设计和类型：

1. **内核中实现（例如 MS-DOS）：** 在某些操作系统中，命令解释程序可以直接内置在内核中，这使得它更加紧密地与操作系统集成。
2. **由系统程序实现（例如 Windows / UNIX）：** 在其他操作系统中，命令解释程序是作为系统程序运行的，不在内核中。这种设计允许更灵活地扩展和定制命令解释程序。
3. **Shell（例如 UNIX / Linux）：** 在类UNIX系统中，通常使用一个称为"Shell"的特殊程序来充当命令解释程序。不同的Shell可以有不同的特性和功能，例如Bourne Shell、C Shell等。

命令解释程序的主要职责是接收用户输入的命令并执行它。命令可以通过两种方式来实现：

- **内置命令（例如 MS-DOS）：** 一些命令解释程序支持内置命令，这意味着某些命令的实现是直接嵌入到命令解释程序中的。这些命令通常执行速度快，但无法轻松扩展或添加新功能。
- **系统程序（例如 UNIX）：** 在其他系统中，大多数命令是通过系统程序来实现的，这些程序可以独立于命令解释程序进行开发和维护。这种方法使得添加新功能和扩展系统功能变得更加容易，因为不需要修改命令解释程序本身。

### Graphical User Interface (GUI)

图形用户界面（Graphical User Interface，GUI）是一种用户友好的桌面元喻界面，通常使用鼠标、键盘和显示器来与计算机进行交互。GUI的特点包括：

- **图标表示文件、程序和操作：** GUI使用图标来代表不同的文件、程序、操作等，使用户可以通过可视化方式来管理和执行任务。
- **鼠标操作：** 用户可以使用鼠标来点击、拖动、右键单击等与界面中的对象互动，触发不同的操作和功能。
- **提供信息和选项：** 用户可以通过鼠标操作来获取对象的信息，查看选项，执行功能，等等。
- **文件夹结构：** GUI通常使用文件夹（也称为目录）来组织文件和数据，用户可以打开文件夹以查看其中的内容，这个操作通常被称为"打开目录"。

GUI的概念最早在20世纪70年代初由帕洛阿尔托研究中心（Xerox PARC）发明。现代操作系统通常提供了CLI和GUI两种接口，以满足不同用户的需求。一些相关的系统和信息包括：

- **Microsoft Windows：** Windows操作系统使用GUI界面，但也包含了命令行界面（CLI）。
- **Apple Mac OS X：** Mac OS X使用名为"Aqua"的GUI界面，底层采用UNIX内核，并提供了多种不同的shell来满足不同的需求。
- **Unix和Linux：** Unix和Linux系统通常提供CLI界面，并可选择性地提供GUI界面，例如Common Desktop Environment（CDE）、KDE Desktop Environment和GNOME GNU Desktop等。

## System Calls

系统调用是操作系统提供的接口，允许应用程序与底层硬件和文件系统交互。操作系统提供的服务分为面向客户（命令接口）和面向编程人员（系统调用）。
1. **命令接口**：用户通过这些操作命令来组织和控制作业的执行。根据控制方式的不同，命令接口可分为两类：  
   - **脱机控制接口**：也称为批处理命令接口，适用于批处理系统。用户将任务清单交给操作系统，操作系统会按照清单顺序逐条执行。  
   - **联机控制接口**：也称为交互式命令接口，适用于分时或实时系统。用户可以与操作系统进行实时交互，操作系统根据用户的指令实时执行任务。
2. **程序接口**：编程人员使用这些接口请求操作系统服务，实际上就是系统调用。通过程序接口，开发者可以方便地实现程序间的通信和协作，提高软件开发效率。

系统调用按照功能可分为以下几类：
1. 设备管理：完成设备的请求、释放和启动等功能。
2. 文件管理：完成文件的读、写、创建和删除等功能。
3. 进程控制：完成进程的创建、撤销、阻塞和唤醒等功能。
4. 进程通信：完成进程之间的消息传递或信号传递等功能。
5. 内存管理：完成内存的分配、回收以及获取作业占用内存区大小和始址等功能。

操作系统内核程序负责处理系统调用，运行在核心态。大多数应用程序通过高级应用程序接口（API）而不是直接的系统调用来访问操作系统的功能。API 提供了一组可供程序员使用的函数，简化了与操作系统交互的过程。操作系统通常使用高级编程语言（如 C/C++）编写，并通过库（如 libc）提供 API，使程序员能够更轻松地访问和利用操作系统的功能。这提高了应用程序的开发和维护效率，同时降低了底层系统调用的复杂性。

> 库函数与系统调用的区别和联系:
> 库函数是语言或应用程序的一部分，可以运行在用户空间中。而系统调用是操作系统的一部分，是内核为用户提供的程序接口，运行在内核空间中，而且许多库函数都会使用系统调用来实现功能。未使用系统调用的库函数，其执行效率通常要比系统调用的高。因为使用系统调用时，需要上下文的切换及状态的转换（由用户态转向核心态）。

### System Call Implementation

运行时支持系统（run-time support system）提供了一个系统调用接口，它作为与系统调用的连接。

这个接口通常包含在与编译器一起提供的库中，这些库中内置了一组函数。

通常，每个系统调用都与一个数字相关联，系统调用接口维护一个表，根据这些数字进行索引。

系统调用接口的功能包括：

- 拦截API中的函数调用。
- 调用操作系统中所需的系统调用。
- 返回系统调用的状态以及任何返回值。

![image-20231021220948796](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021220948796.png)

调用者无需了解系统调用的实现方式，或者在执行期间它执行了什么操作。

他们只需要遵守API，并理解作为结果操作系统将执行的操作。

API隐藏了程序员不需要了解的大部分操作系统接口的细节，这些细节由运行时支持库进行管理。

### System Call Parameter Passing

通常，传递给操作系统的信息通常不仅仅是所需的系统调用的标识。

所需的信息类型和数量因操作系统和调用而异。

有三种常见的方法用于将参数传递给操作系统：

1. 寄存器，最简单的方法。
2. 内存块，参数存储在内存中的块或表中，块的地址作为参数传递给寄存器（Linux 和 Solaris 采用这种方法）。
3. 栈，程序将参数推送到栈上，由操作系统从栈上弹出。

![image-20231021221054409](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/image-20231021221054409.png)

## Types of System Calls

### Process control

- 结束、中止、加载和执行进程：用于管理进程的创建和终止。
- 创建进程、终止进程：用于创建和终止进程。
- 获取进程属性、设置进程属性：用于获取和设置进程的属性信息。
- 等待时间、等待事件、信号事件：用于处理等待时间、事件和信号的操作。
- 分配和释放内存：用于管理内存的分配和释放。
- 内存转储：在发生错误时将内存内容转储到文件中，以进行故障诊断。
- 调试器：用于识别和调试程序中的错误，支持单步执行和监视程序的执行。
- 锁：用于管理多个进程之间对共享数据的访问，以确保数据的一致性和互斥访问。

fork、exec、wait和exit这四个系统调用是和进程有关的最为重要的四个系统调用：

- fork用来创建进程；
- exec从磁盘上载入并执行某个可执行程序；
- exit是进程自己退出时要调用的函数；
- 调用wait的进程会等到子进程退出时才继续执行；

fork

fork系统调用的函数原型定义为：

int fork();
1
这个函数没有参数，调用该函数的进程会再创建一个进程，新创建的进程是原进程的子进程；
两个进程都从fork（）这个地方继续往下执行，并且执行“同样”的代码。但是父进程执行fork（）会返回子进程的ID，而子进程调用fork（）会返回0，父子进程正是通过对这个返回值的判断（用if 语句）来决定分别执行哪段代码；

exec

系统调用exec（）的功能是在当前进程中执行一段新程序，进程的PID保持不变。可以这样形象地理解，一个进程就像一个壳子，在这个壳子里可以装各种可执行代码。fork（）创建了这个壳子，并且将父进程的代码装在这个壳子中执行，而exec（）是用一个磁盘上的可执行程序（exec（）的参数告诉操作系统是哪个可执行程序）替换了这个壳子里原有的内容；

exec()函数分为两类，分别以execl和execv开头，其函数原型定义如下：

void execl(const char*filepath,const char*arg1,char*arg2,…);
void execlp(const char*filename,const char*argl,char*arg2,…); 
void execv(const char* filepath,char* argv[]); 
void execvp(const char* filename,char* argv[]);
1
2
3
4
这些函数基本上一样，只是execl中对应可执行程序入口函数的参数，即其中的arg1、arg2等，是一个一个列举出来的，而execv是将这些参数组织成一个数组告诉操作系统的；

可执行程序入口函数的参数就是可执行程序的main（int argc，char*argv[]）函数中的参数argv，我们都知道这些参数是通过命令行输入的，命令行中的参数实际上就是用系统调用execl/execv函数中的参数传进去的；

函数名中带p和不带p的差别在于：execv（）中filepath是绝对路径，而execvp（）中的filename是相对路径；

exit

系统调用exit用来终止一个进程，在进程中可以显式调用exit来终止自己，也可以隐式调用exit；

操作系统在编译main（）函数时，当遇到main（）函数的最后一个}时会“塞入”一个exit；

exit（）函数的原型定义为：

void exit(int status);
1
exit中的参数status是退出进程返回给其父进程的退出码。同时，退出的进程会向其父进程发送一个SIGCHILD信号，一个进程执行wait系统调用时就会暂停自己的执行来等待这个信号。所以wait和exit合在一起可以完成这样一种进程之间的同步合作：父进程启动了一个子进程，调用wait 等待子进程执行完毕；子进程执行完毕以后调用exit给父进程发送一个信号SIGCHILD，父进程被唤醒继续执行；

wait

wait系统调用的函数原型定义为：

int wait(int *stat_addr);
1
其返回值是exit子进程的PID，stat_addr是进程中定义的一个变量，用于存放子进程调用exit时的退出码，即exit 系统调用的参数status；

### File management

- 创建文件：用于创建新的文件。
- 删除文件：用于删除现有文件。
- 打开文件：用于打开文件以进行读取和写入操作。
- 关闭文件：用于关闭已打开的文件。
- 读取文件：用于从文件中读取数据。
- 写入文件：用于将数据写入文件。
- 重新定位文件指针：用于移动文件指针到文件中的不同位置。
- 获取文件属性：用于获取文件的属性信息，如大小、权限等。
- 设置文件属性：用于设置文件的属性，如权限、时间戳等。

fork

fork系统调用的函数原型定义为：

int fork();
1
这个函数没有参数，调用该函数的进程会再创建一个进程，新创建的进程是原进程的子进程；
两个进程都从fork（）这个地方继续往下执行，并且执行“同样”的代码。但是父进程执行fork（）会返回子进程的ID，而子进程调用fork（）会返回0，父子进程正是通过对这个返回值的判断（用if 语句）来决定分别执行哪段代码；

exec

系统调用exec（）的功能是在当前进程中执行一段新程序，进程的PID保持不变。可以这样形象地理解，一个进程就像一个壳子，在这个壳子里可以装各种可执行代码。fork（）创建了这个壳子，并且将父进程的代码装在这个壳子中执行，而exec（）是用一个磁盘上的可执行程序（exec（）的参数告诉操作系统是哪个可执行程序）替换了这个壳子里原有的内容；

exec()函数分为两类，分别以execl和execv开头，其函数原型定义如下：

void execl(const char*filepath,const char*arg1,char*arg2,…);
void execlp(const char*filename,const char*argl,char*arg2,…); 
void execv(const char* filepath,char* argv[]); 
void execvp(const char* filename,char* argv[]);
1
2
3
4
这些函数基本上一样，只是execl中对应可执行程序入口函数的参数，即其中的arg1、arg2等，是一个一个列举出来的，而execv是将这些参数组织成一个数组告诉操作系统的；

可执行程序入口函数的参数就是可执行程序的main（int argc，char*argv[]）函数中的参数argv，我们都知道这些参数是通过命令行输入的，命令行中的参数实际上就是用系统调用execl/execv函数中的参数传进去的；

函数名中带p和不带p的差别在于：execv（）中filepath是绝对路径，而execvp（）中的filename是相对路径；

exit

系统调用exit用来终止一个进程，在进程中可以显式调用exit来终止自己，也可以隐式调用exit；

操作系统在编译main（）函数时，当遇到main（）函数的最后一个}时会“塞入”一个exit；

exit（）函数的原型定义为：

void exit(int status);
1
exit中的参数status是退出进程返回给其父进程的退出码。同时，退出的进程会向其父进程发送一个SIGCHILD信号，一个进程执行wait系统调用时就会暂停自己的执行来等待这个信号。所以wait和exit合在一起可以完成这样一种进程之间的同步合作：父进程启动了一个子进程，调用wait 等待子进程执行完毕；子进程执行完毕以后调用exit给父进程发送一个信号SIGCHILD，父进程被唤醒继续执行；

wait

wait系统调用的函数原型定义为：

int wait(int *stat_addr);
1
其返回值是exit子进程的PID，stat_addr是进程中定义的一个变量，用于存放子进程调用exit时的退出码，即exit 系统调用的参数status；

### Device management

- 请求设备：用于请求使用特定设备。
- 释放设备：用于释放先前请求的设备。
- 读取设备：用于从设备读取数据。
- 写入设备：用于将数据写入设备。
- 重新定位设备指针：用于移动设备指针到不同位置。
- 获取设备属性：用于获取设备的属性信息，如设备类型、状态等。
- 设置设备属性：用于设置设备的属性，如配置参数、状态等。
- 逻辑附加或分离设备：用于将设备逻辑连接或分离到系统中，以便程序可以访问它。

printf和scanf是用来分别操纵显示器和键盘的函数，函数原型是：

void printf（格式化输出字符串，输出内容，…）；//如printf("ID:%d",3)
void scanf（格式化输入字符串，输入内存地址，…）；//如scanf("ID:%d",&id)
1
2
注意，这两个函数不是系统调用，仅仅只是两个库函数，这两个库函数的实现依赖于write和read实现“写显示器”和“读键盘”

Q：库函数和系统调用的区别在哪里？

A：

系统调用是最底层的应用，是面向硬件的。而库函数的调用是面向开发的，相当于应用程序的API接口；
各个操作系统的系统调用是不同的，因此系统调用一般是没有跨操作系统的可移植性，而库函数的移植性良好；
库函数属于过程调用，调用开销小；系统调用需要在用户空间和内核上下文环境切换，开销较大；
库函数调用函数库中的一段程序，这段程序最终还是通过系统调用来实现的；系统调用调用的是系统内核的服务；

### Communications

1. **消息传递模型（Message-Passing Model）：** 在消息传递模型中，进程通过操作系统提供的进程间通信（IPC）机制来交换信息。这种模型涉及创建和删除通信连接，以及通过发送和接收消息来进行通信。进程可以使用系统调用来发送消息给其他进程，同时也需要相应的系统调用来接收消息。这种模型通常用于实现进程间的协作和数据传输。
2. **共享内存模型（Shared-Memory Model）：** 在共享内存模型中，进程使用映射内存的系统调用来访问其他进程拥有的内存区域。这允许多个进程在相同的内存区域中读取和写入数据，从而实现了共享数据。进程可以使用系统调用来附加或分离共享内存区域，以便其他进程可以访问它。这种模型通常用于实现高效的数据共享和协作。

无论是消息传递模型还是共享内存模型，它们都提供了不同的方式来实现进程之间的通信和协作，以满足不同的应用需求。

### Information maintenance

以下是一些与时间、日期、系统数据、进程、文件和设备属性相关的系统调用：

1. 获取时间或日期：
   - 系统调用可以用来获取当前系统的时间和日期信息。这通常包括当前的日期、时间、时区等信息。

2. 设置时间或日期：
   - 系统调用也可以用于设置系统的时间和日期。这通常需要适当的权限以确保时间和日期的准确性。

3. 获取系统数据：
   - 系统调用可用于获取有关系统状态和性能的信息。这可能包括CPU使用情况、内存使用情况、系统负载等数据。

4. 设置系统数据：
   - 类似地，系统调用也可以用于设置系统数据，如性能参数或系统配置。

5. 获取进程、文件或设备属性：
   - 这些系统调用允许程序查询有关正在运行的进程、文件或设备的属性信息。这可以包括文件大小、权限、进程状态、设备参数等。

6. 设置进程、文件或设备属性：
   - 同样，系统调用也可以用于修改或设置进程、文件或设备的属性。这可能需要适当的权限以确保数据的完整性和安全性。

这些系统调用对于操作系统和应用程序来说都非常重要，因为它们允许程序获取和修改系统和资源的信息，以便更有效地管理和控制计算机系统。

### Protection

这涉及到操作系统中的权限和访问控制，它允许操作系统控制对资源的访问和资源的保护。以下是一些相关的概念和操作：

1. **资源访问控制：** 操作系统用于控制对资源（如文件、目录、设备等）的访问的机制，允许或拒绝用户或进程访问资源。

2. **权限：** 权限规定了哪些用户或进程具有对资源的访问权限，以及允许执行的操作类型（读取、写入、执行等）。

3. **获取和设置权限：** 操作系统提供了一些系统调用或命令，允许管理员或拥有资源的用户获取和设置资源的权限。

4. **允许和拒绝用户访问：** 操作系统允许管理员或资源所有者明确允许或拒绝特定用户或用户组对资源的访问。这通常通过权限设置来实现。

5. **用户身份验证：** 为了访问受保护的资源，用户必须通过身份验证，通常通过用户名和密码或其他身份验证方法。

6. **资源保护：** 操作系统负责保护资源免受未经授权的访问或损坏，以确保数据的完整性和安全性。

7. **资源拥有者：** 每个资源都有一个拥有者，通常是创建或分配资源的用户。资源拥有者通常具有更广泛的权限来控制资源的访问。

这些概念和操作对于确保操作系统的安全性和资源管理至关重要。通过适当的权限和访问控制，操作系统可以确保只有授权用户和进程可以访问资源，从而提高系统的安全性和可靠性。

## System Programs

系统程序是计算机操作系统中的一组程序，用于提供各种服务和功能。以下是系统程序的主要知识点：

**文件管理：**
- 创建、删除、复制、重命名、打印、导出、列出文件和目录。
  

**状态信息：**
- 一些系统程序可以获取系统信息，如日期、时间、可用内存、磁盘空间和用户数量。
- 其他系统程序提供详细的性能、日志记录和调试信息，通常将输出格式化并显示在终端或其他输出设备上。
- 一些系统实现了注册表，用于存储和检索配置信息。

**文件修改：**
- 文本编辑器用于创建和修改文件，而特殊命令可搜索文件内容或进行文本转换。

**编程语言支持：**
- 编译器、汇编程序、调试器和解释器有时会提供编程语言支持。

**程序加载和执行：**
- 包括绝对装载程序、可重定位装载程序、链接编辑器和覆盖加载程序等。
- 还包括用于高级语言和机器语言的调试系统。

**通信：**
- 提供创建进程、用户和计算机系统之间虚拟连接的机制。
- 允许用户将消息发送到其他用户的屏幕、浏览网页、发送电子邮件、远程登录、在不同机器之间传输文件等。

**系统实用程序/应用程序：**
- 由用户运行，通常不被视为操作系统的一部分。
- 可通过命令行、鼠标点击或手指触摸等方式启动。
- 例如，Web浏览器、文字处理器、电子表格、数据库系统、编译器、统计分析软件和游戏等。

**后台服务：**
- 在系统启动时启动，一些在系统启动后终止，一些在系统启动到关机期间一直运行。
- 提供诸如磁盘检查、进程调度、错误日志记录、打印等功能。
- 在用户上下文中运行，而非内核上下文中。
- 通常被称为服务、子系统或守护程序。

## Operating System Design and Implementation

操作系统的设计与实现是一个复杂的任务，没有一个“完美”的解决方案，但一些方法已被证明是成功的。不同操作系统的内部结构可以有很大的差异。设计与实现操作系统通常涉及以下关键概念：

**目标与规格定义：**
- 操作系统的设计首先需要明确定义目标和规格。这些目标受到硬件选择、系统类型（批处理、分时、实时、单用户或多用户、分布式等）的影响。

**难以规定的要求：**
- 操作系统的需求难以精确定义，因为它们需要满足多方面的用户和系统目标。
- 用户目标包括方便使用、易学易用、可靠、安全和高效。
- 系统目标包括易于设计、实现和维护、灵活、可靠、无错误和高效。
- 规定和设计操作系统是软件工程的高度创造性任务。

**策略与机制分离：**
- 重要原则是将策略与机制分离。
- 策略是关于“做什么”的问题，而机制是关于“如何做”的问题。
- 分离策略和机制是一个非常重要的原则，它允许在以后更改策略决策时具有最大的灵活性。
- 例如，设置时间片长度和给予某些类型的程序优先级就是策略和机制分离的例子。
- Unix和Windows的调度策略就是这种分离的例子。

**多种编程语言的使用：**
- 操作系统的实现通常包含多种编程语言的混合使用。
- 较低层次的部分可能使用汇编语言编写，主体部分通常使用C语言编写。
- 系统程序可能使用C、C++以及脚本语言（如PERL、Python、Shell脚本）编写。
- 使用更高级别的语言使操作系统更容易移植到不同的硬件平台，但可能会导致性能较差和存储需求较大。

**模拟（Emulation）：**
- 模拟允许操作系统在非本机硬件上运行。
- 模拟可以通过模拟硬件或虚拟机来实现。
- 这样的模拟可以提高操作系统在不同平台上的可移植性。

**操作系统性能的提升：**
- 提高操作系统性能的主要方法包括改进数据结构和算法。
- 仅有少量代码对于高性能至关重要，包括中断处理程序、I/O管理、内存管理和CPU调度器。
- 写好并正确运行后，可以识别性能瓶颈的关键例程，然后将其替换为汇编语言等效代码以提高性能。

## Operating System Structure

### 简单结构 Simple structure

#### MS-DOS

MS-DOS（Microsoft Disk Operating System）是为了在最小的空间内提供最大功能而编写的操作系统。然而，它存在一些结构上的不足：

1. **缺乏模块化分割：** MS-DOS没有被精心分割为模块化的组件。这意味着其功能没有被很好地分离和隔离，而是被混杂在一起，这使得系统的维护和扩展变得更加困难。

2. **接口和功能的分离问题：** MS-DOS中，接口和功能的分离程度较低。这意味着不同功能的代码没有被清晰地分隔开，这可能导致代码的不稳定性和难以维护。

3. **受限于硬件：** MS-DOS在某种程度上受到了硬件的限制。例如，它最初是设计用于Intel 8088处理器，这个处理器缺乏一些现代操作系统所需的功能，比如双模式（用户模式和内核模式）和硬件保护机制。

因此，MS-DOS虽然在其时间内非常重要，但由于结构上的不足，它在面对较新的硬件和复杂的任务时变得不够灵活。这促使了后来的操作系统设计采用更模块化、分层的方法，以提高系统的稳定性和可维护性。

#### original UNIX

原始的UNIX操作系统在结构上较为有限，这主要是由硬件功能的限制所决定的。它由两个可分离的部分组成：

1. **系统程序：** 这部分包括用户可以运行的各种应用程序，如文本编辑器、编译器等。系统程序构成了用户与操作系统交互的一部分。

2. **内核：** 内核是操作系统的核心部分，位于系统的最底层。内核被分为一系列接口和设备驱动程序，包括系统调用接口、文件系统、CPU调度、内存管理等功能。内核位于系统调用接口以下，位于物理硬件之上，它为系统提供了基本的操作系统功能。

UNIX操作系统的API（应用程序编程接口）是通过系统调用定义的，而通常可用的系统程序则构成了用户界面。这种结构使得用户可以运行各种程序，而内核负责管理硬件和提供系统服务。

需要注意的是，原始的UNIX操作系统的结构相对简单，这是因为它诞生于较早的计算机时代，当时的硬件功能和资源有限。后来的UNIX变种和其他操作系统采用了更模块化和层次化的结构，以适应更复杂的硬件和应用需求。

### 分层结构 Layered Approach

操作系统可以分解成多个层次（或称为层级），每一层建立在较低层次之上。这些层次构成了操作系统的层次结构，其中最底层（第0层）是硬件，而最高层（第N层）是用户界面。

在这种层次结构中，每个操作系统层次都表示一个抽象对象，由数据和可以操作这些数据的函数组成。模块化设计使得每一层次都可以使用较低层次的函数和服务，以构建更高层次的功能。

这种结构使系统各部分相互隔离，降低复杂性，更易于维护和扩展。主要优点是简化系统设计和开发，降低错误风险，更容易扩展和维护，有助于管理复杂性。

将操作系统视为一系列层次结构是一种有助于管理系统复杂性的方法。这个方法的主要思想是将系统的功能和组件分解为多个层次，每个层次负责执行相关的功能，同时依赖于较低层次的功能来完成更基本的操作。这种分解有以下主要优势：

主要优势：

1. 构建和调试的简单性：通过将系统划分为多个层次，每个层次有明确定义的功能，使得系统的构建和调试变得更加简单。每个层次都可以独立开发和测试，减少了复杂性。
2. 设计和实施的简化：层次结构化方法有助于将系统的设计和实施任务分解为更小的、可管理的子问题。这种分解有助于管理复杂性，使整个过程更加清晰。

主要困难：

1. 层次的仔细定义：确定每个层次的功能和接口需要仔细的定义。不同层次之间的依赖性和接口必须清晰明确，以确保系统的正确运行。
2. 复杂性的分层结构：在一些情况下，分层结构可能导致复杂性的增加，尤其是在处理诸如磁盘驱动器和内存管理等关键组件时。这可能需要更复杂的协作和通信。

在操作系统中，将其视为一系列层次结构可以帮助管理系统的复杂性，但这种方法可能不如其他结构类型高效。在这种分层结构中，每个层次负责一组相关的功能，而每个系统调用必须经过多个层次才能达到硬件层面，这可能会导致性能开销。以下是有关这一点的详细信息：

1. 效率较低：这种分层结构可能导致系统调用的效率较低。当执行系统调用时，它必须经过多个层次，每个层次可能需要对参数进行修改、传递数据等。这会增加系统调用的开销。

2. 举例：考虑执行I/O操作的情况。当应用程序执行I/O操作时，它会触发系统调用，该系统调用被传递到I/O层次。然后，I/O层次可能需要与内存管理层次和CPU调度层次进行通信，最终数据被传递给硬件执行。这些多次通信和数据传递可能导致性能下降。

3. 宏内核（Monolithic Kernel）：在宏内核结构中，整个核心操作系统运行在内核空间和监管者模式下。这种结构通常没有明确的层次结构，所有功能都在一个单一的内核中实现。这可能会导致较低的模块化和较高的性能，但也可能增加维护的复杂性。

总的来说，分层结构虽然有助于管理系统复杂性，但可能在性能方面存在一些开销。不同的操作系统采用不同的设计方法，以在性能和复杂性之间取得平衡。对于某些用途，如实时系统，可能更倾向于采用更紧凑的内核结构，而对于通用用途的操作系统，可能更愿意牺牲一些性能以获得更好的可维护性和可扩展性。

### 微内核结构 Microkernels

微内核（Microkernel）是一种操作系统设计结构，旨在将操作系统的基本功能模块化，并将尽可能多的功能移到用户空间，以提高系统的模块性、灵活性和可维护性。以下是有关微内核的一些关键概念和优点：

**基本功能：** 微内核包含操作系统的基本功能，如内存管理、CPU调度和进程间通信（IPC）。这些功能通常被认为是不可或缺的，但微内核试图将其他功能从内核中分离出来。

**用户空间服务：** 微内核将额外的服务移到用户空间中，这些服务可以在用户空间中运行并与客户程序通信。客户程序可以使用这些服务来执行特定任务，而无需直接涉及内核。这提高了系统的可定制性和灵活性。

**通信方式：** 微内核系统通常使用消息传递来实现客户程序和用户空间服务之间的通信。消息传递允许客户程序发送请求或数据给服务，并通过消息响应来执行相应的操作。

**优点：** 微内核结构的主要优点包括：
- 模块化：它允许操作系统的各个部分相对独立地开发和维护，减少了模块之间的相互影响，提高了可维护性。
- 灵活性：新的服务可以相对容易地添加到用户空间，而不必更改整个内核，提供了更大的可配置性和扩展性。
- 安全性和可靠性：微内核的模块化设计使得每个模块都可以进行严格测试，如果一个服务出现问题，不会影响整个操作系统的稳定性。

然而，微内核结构可能会导致一定的性能开销，因为消息传递和用户空间切换可能比在内核中执行相同功能更慢。因此，在对性能要求非常高的应用程序中，可能会选择其他内核结构。不同的操作系统采用不同的内核结构，以满足其设计目标和需求。

微内核结构的劣势在于性能可能受到一定的影响，主要表现在以下方面：

**系统功能开销增加：** 使用微内核结构会引入更多的系统功能开销，例如消息传递和用户空间切换。这些额外的开销可能会导致性能下降，特别是在对性能要求非常高的应用程序中。

**性能历史问题：** 一个明显的例子是Windows NT操作系统的性能历史。最初的Windows NT 1.0采用了分层微内核组织，导致性能较低，与Windows 95相比表现不佳。随后的Windows NT 4.0部分改进了性能问题，将某些层次从用户空间移到内核空间，并更加紧密地集成它们。然而，随着Windows XP的设计，Windows的架构变得更加单片化，不再典型的微内核结构。

1. 大内核和微内核
	1. 大内核系统将操作系统的主要功能模块都作为一个紧密联系的整体运行在核心态，从而为应用提供高性能的系统服务；由于复杂的交互关系使得层次之间的界限极其模糊，定义清晰的层次间接口非常困难；
	2. 微内核将内核中最基本的功能保留在内核，将那些不需要在核心态执行的功能移到用户态执行；微内核结构最大的问题是性能问题，需要频繁在核心态与用户态之间进行切换；

### 模块化结构 Modules

现代许多操作系统采用可加载的内核模块。内核包含一组核心组件，并通过模块动态链接其他服务，可以在启动时或运行时进行。这种方法在现代UNIX的实现中非常常见，例如Solaris、Linux和Mac OS X，以及Windows。以下是有关可加载内核模块的一些关键概念：

**常见性：** 可加载内核模块在现代操作系统中广泛采用，尤其是在UNIX系列操作系统中。

**面向对象的方法：** 可加载内核模块采用面向对象的方法。每个核心组件都是独立的模块，它们之间使用已知接口进行通信。

**模块化：** 每个核心组件都是可加载的，可以根据需要在内核中加载。这增加了系统的可扩展性和定制性。

**受保护接口：** 每个核心组件都有定义的受保护接口，确保安全性和隔离性。

**更高的灵活性：** 与微内核结构相似，可加载内核模块具有更高的灵活性，因为任何模块都可以调用其他模块，而不必通过消息传递来进行通信。

**更高的效率：** 相对于一些其他结构，可加载内核模块通常更加高效，因为模块之间的通信无需调用消息传递，从而减少了开销。

总的来说，可加载内核模块是一种在现代操作系统中广泛采用的内核结构，它融合了模块化、面向对象和高效的设计原则，以提供更大的灵活性和可扩展性。

## Virtual Machines

操作系统通过CPU调度和虚拟内存的方式，为多个进程创造了各自在其自己处理器上执行以及拥有自己（虚拟）内存的幻觉。

物理计算机的资源被共享以创建虚拟机。

虚拟机将分层方法推向了逻辑极限。它将硬件和操作系统内核视为硬件。

虚拟机提供与底层裸机硬件相同的接口。虚拟化是一项技术，允许操作系统作为其他操作系统内的应用程序运行。

虚拟机管理器（VMM）提供虚拟化服务，本机编译的操作系统可以运行本机编译的客户操作系统。

在源CPU类型与目标类型不同的情况下使用模拟。

例如，模拟设备“Rosetta”允许为IBM CPU编译的应用程序在Intel CPU上运行。

当计算机语言未编译为本机代码时，使用解释（例如，BASIC、Java）。

虚拟机提供了对系统资源的完全保护。

每个虚拟机与所有其他虚拟机隔离开来。这种隔离不允许资源的直接共享。

虚拟机系统是操作系统研究和开发的理想工具。

系统的开发是在虚拟机上进行的，因此不会干扰正常的系统操作。

随着虚拟机的普及，它们越来越多地用于解决系统兼容性问题。虚拟机的实现较为困难，因为需要提供与底层机器完全相同的副本。

虚拟用户模式和虚拟监控模式在物理用户模式下运行。

导致从用户模式转移到监控模式的实际机器上的操作也必须导致从虚拟用户模式转移到虚拟监控模式的虚拟机上。

两类虚拟化方法：
1. 第一类
	从技术上讲，第一类虚拟机管理程序就像一个操作系统，因为它是唯一一个运行在最高特权
	级的程序。它在裸机上运行并且具备多道程序功能。虚拟机管理程序向上层提供若干台虚拟机，
	这些虚拟机是裸机硬件的精确复制品。由于每台虚拟机都与裸机相同，所以在不同的虚拟机上可
	以运行任何不同的操作系统。图1.7（ a） 中显示了第一类虚拟机管理程序。
	虚拟机作为用户态的一个进程运行，不允许执行敏感指令。然而，虚拟机上的操作系统认为
	自己运行在内核态（实际上不是），称为虚拟内核态。虚拟机中的用户进程认为自己运行在用户
	态（实际上确实是）。当虚拟机操作系统执行了一条CPU处于内核态才允许执行的指令时，会陷
	入虚拟机管理程序。在支持虚拟化的CPU上，虚拟机管理程序检查这条指令是由虚拟机中的操作
	系统执行的还是由用户程序执行的。如果是前者，虚拟机管理程序将安排这条指令功能的正确执
	行。否则，虚拟机管理程序将模拟真实硬件面对用户态执行敏感指令时的行为。
	在过去不支持虚拟化的CPU上，真实硬件不会直接执行虚拟机中的敏感指令，这些敏感指
	令被转为对虚拟机管理程序的调用，由虚拟机管理程序模拟这些指令的功能。
2. 第二类
图1.7（ b） 中显示了第二类虚拟机管理程序。它是一个依赖于Windows、Linux等操作系统分配
和调度资源的程序，很像一个普通的进程。第二类虚拟机管理程序仍然伪装成具有CPU和各种设
备的完整计算机。VMware Workstation是首个X86平台上的第二类虚拟机管理程序。
运行在两类虚拟机管理程序上的操作系统都称为客户操作系统。对于第二类虚拟机管理程
序，运行在底层硬件上的操作系统称为宿主操作系统。
首次启动时，第二类虚拟机管理程序像一台刚启动的计算机那样运转，期望找到的驱动器可
以是虚拟设备。然后将操作系统安装到虚拟磁盘上（其实只是宿主操作系统中的一个文件）。客
户操作系统安装完成后，就能启动并运行。
虚拟化在Web主机领域很流行。没有虚拟化，服务商只能提供共享托管（不能控制服务器的
软件）和独占托管（成本较高）。当服务商提供租用虚拟机时，一台物理服务器就可以运行多个
虚拟机，每个虚拟机看起来都是一台完整的服务器，客户可以在虚拟机上安装自己想用的操作系
统和软件，但是只需支付较低的费用。这就是市面上常见的“云"主机。
有的教材将第一类虚拟化技术称为裸金属架构，将第二类虚拟化技术称为寄居架构。
![截图_20231102170614.png](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20231102170614.png)

## Operating System Generation

操作系统被设计为可以运行在某一类计算机上；系统必须为每个具体的计算机站点进行配置。

SYSGEN程序获取有关硬件系统特定配置的信息。

系统生成的方法

1. 修改操作系统源代码的副本。

2. 从预编译库中创建表格并选择模块。

3. 创建适当的表格以描述系统，系统完全由表格驱动。

选择发生在执行时。

## System Boot

引导 - 通过加载内核来启动计算机

操作系统必须被提供给硬件，以便硬件能够启动它。
操作系统引导是指计算机利用CPU运行特定程序，通过程序识别硬盘，识别硬盘分区，识别硬盘分区上的操作系统，最后通过程序启动操作系统，一环扣一环地完成上述过程。


常见操作系统的引导过程如下：
① 激活CPU。激活的CPU读取ROM中的boot程序，将指令寄存器置为BIOS （基本输入/输出系统）的第一条指令，即开始执行BIOS的指令。
② 硬件自检。启动BIOS程序后，先进行硬件自检，检查硬件是否出现故障。如有故障，主板会发出不同含义的蜂鸣，启动中止：如无故障，屏幕会显示CPU、内存、硬盘等信息。
③ 加载带有操作系统的硬盘。硬件自检后，BIOS开始读取Boot Sequence （通过CMOS里保存的启动顺序，或者通过与用户交互的方式），把控制权交给启动顺序排在第一位的存储设备，
然后CPU将该存储设备引导扇区的内容加载到内存中。
④ 加载主引导记录MBRo硬盘以特定的标识符区分引导硬盘和非引导硬盘。如果发现一个存储设备不是可引导盘，就检查下一个存储设备。如无其他启动设备，就会死机。主引导记录MBR的作用是告诉CPU去硬盘的哪个主分区去找操作系统。
⑤ 扫描硬盘分区表，并加载硬盘活动分区。MBR包含硬盘分区表，硬盘分区表以特定的标识符区分活动分区和非活动分区。主引导记录扫描硬盘分区表，进而识别含有操作系统的硬盘分
区（活动分区）。找到硬盘活动分区后，开始加载硬盘活动分区，将控制权交给活动分区。
⑥ 加载分区引导记录PBR。读取活动分区的第一个扇区，这个扇区称为分区引导记录（ PBR） ,其作用是寻找并激活分区根目录下用于引导操作系统的程序（启动管理器）。
⑦ 加载启动管理器。分区引导记录搜索活动分区中的启动管理器，加载启动管理器。
⑧ 加载操作系统。

