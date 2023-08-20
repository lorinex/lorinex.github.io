---
categories:
  - 计算机基础
  - 计算机系统
---
# GDB

## GDB是什么

是 Linux 下常用的程序调试器，全称“GNU symbolic debugger”

### 功能

借助 GDB 调试器可以实现以下几个功能：

1. 程序启动时，可以按照我们自定义的要求运行程序，例如设置参数和环境变量；
2. 可使被调试程序在指定代码处暂停运行，并查看当前程序的运行状态（例如当前变量的值，函数的执行结果等），即支持断点调试；
3. 程序执行过程中，可以改变某个变量的值，还可以改变代码的执行顺序，从而尝试修改程序中出现的逻辑错误。

## GDB调试C/C++程序

正常情况下，使用 GCC 编译该源代码的指令如下：

```bash
[root@bogon demo]# ls
main.c
[root@bogon demo]# gcc main.c -o main.exe
[root@bogon demo]# ls
main.c  main.exe
```

只需要使用 gcc -g 选项编译源文件，即可生成满足 GDB 要求的可执行文件。仍以 main.c 源程序文件为例：

```bash
[root@bogon demo]# ls
main.c
[root@bogon demo]# gcc main.c -o main.exe -g
[root@bogon demo]# ls
main.c  main.exe
```

由此生成的 main.exe，即可使用 GDB 进行调试。

### GDB常用的调试指令

| 调试指令                    | 作 用                                                        |
| --------------------------- | ------------------------------------------------------------ |
| (gdb) break xxx (gdb) b xxx | 在源代码指定的某一行设置断点，其中 xxx 用于指定具体打断点的位置。 |
| (gdb) run (gdb) r           | 执行被调试的程序，其会自动在第一个断点处暂停执行。           |
| (gdb) continue (gdb) c      | 当程序在某一断点处停止运行后，使用该指令可以继续执行，直至遇到下一个断点或者程序结束。 |
| (gdb) next (gdb) n          | 令程序一行代码一行代码的执行。                               |
| (gdb) print xxx (gdb) p xxx | 打印指定变量的值，其中 xxx 指的就是某一变量名。              |
| (gdb) list (gdb) l          | 显示源程序代码的内容，包括各行代码所在的行号。               |
| (gdb) quit (gdb) q          | 终止调试。                                                   |

## 启动调试

### 哪类程序可被调试

对于C程序来说，需要在编译时加上-g参数，保留调试信息，否则不能使用GDB进行调试。
但如果不是自己编译的程序，并不知道是否带有-g参数，如何判断一个文件是否带有调试信息呢？

#### gdb 文件

例如：

```text
$ gdb helloworld
Reading symbols from helloWorld...(no debugging symbols found)...done.
```

如果没有调试信息，会提示no debugging symbols found。
如果是下面的提示：

```text
Reading symbols from helloWorld...done.
```

则可以进行调试。

#### readelf查看段信息

例如：

```text
$ readelf -S helloWorld|grep debug
  [28] .debug_aranges    PROGBITS         0000000000000000  0000106d
  [29] .debug_info       PROGBITS         0000000000000000  0000109d
  [30] .debug_abbrev     PROGBITS         0000000000000000  0000115b
  [31] .debug_line       PROGBITS         0000000000000000  000011b9
  [32] .debug_str        PROGBITS         0000000000000000  000011fc
```

helloWorld为文件名，如果没有任何debug信息，则不能被调试。

#### file查看strip状况

下面的情况也是不可调试的：

```text
$ file helloWorld
helloWorld: (省略前面内容) stripped
```

如果最后是stripped，则说明该文件的符号表信息和调试信息已被去除，不能使用gdb调试。但是not stripped的情况并不能说明能够被调试。

### 调试方式运行程序

程序还未启动时，可有多种方式启动调试。

#### 调试启动无参程序

例如：

```text
$ gdb helloWorld
(gdb)
```

输入run命令，即可运行程序

#### 调试启动带参程序

假设有以下程序，启动时需要带参数：

```c
#include<stdio.h>
int main(int argc,char *argv[])
{
    if(1 >= argc)
    {
        printf("usage:hello name\n");
        return 0;
    }
    printf("Hello World %s!\n",argv[1]);
    return 0 ;
}
```

编译：

```text
$ gcc -g -o hello hello.c
```

这种情况如何启动调试呢？需要设置参数：

```text
$ gdb hello
(gdb)run 编程珠玑
Starting program: /home/shouwang/workspaces/c/hello 编程珠玑
Hello World 编程珠玑!
[Inferior 1 (process 20084) exited normally]
(gdb)
```

只需要run的时候带上参数即可。
或者使用set args，然后在用run启动：

```text
$ gdb hello
(gdb) set args 编程珠玑
(gdb) run
Starting program: /home/hyb/workspaces/c/hello 编程珠玑
Hello World 编程珠玑!
[Inferior 1 (process 20201) exited normally]
(gdb) 
```

#### 调试core文件

当程序core dump时，可能会产生core文件，它能够很大程序帮助我们定位问题。但前提是系统没有限制core文件的产生。可以使用命令limit -c查看：

```text
$ ulimit -c
0
```

如果结果是0，那么恭喜你，即便程序core dump了也不会有core文件留下。我们需要让core文件能够产生：

```text
$ ulimit -c unlimied  #表示不限制core文件大小
$ ulimit -c 10        #设置最大大小，单位为块，一块默认为512字节
```

上面两种方式可选其一。第一种无限制，第二种指定最大产生的大小。
调试core文件也很简单：

```text
$ gdb 程序文件名 core文件名
```

#### 调试已运行程序

如果程序已经运行了怎么办呢？
首先使用ps命令找到进程id：

```text
$ ps -ef|grep 进程名
```

或者：

```text
$ pidof 进程名
```

##### attach方式

假设获取到进程id为20829，则可用下面的方式调试进程：

```text
$ gdb
(gdb) attach 20829
```

接下来就可以继续你的调试啦。

可能会有下面的错误提示：

```text
Could not attach to process.  If your uid matches the uid of the target
process, check the setting of /proc/sys/kernel/yama/ptrace_scope, or try
again as the root user.  For more details, see /etc/sysctl.d/10-ptrace.conf
ptrace: Operation not permitted.
```

解决方法，切换到root用户：
将/etc/sysctl.d/10-ptrace.conf中的

```text
kernel.yama.ptrace_scope = 1
```

修改为

```text
kernel.yama.ptrace_scope = 0
```

##### 直接调试相关id进程

还可以是用这样的方式gdb program pid，例如:

```text
gdb hello 20829  
```

或者：

```text
gdb hello --pid 20829
```

#### 已运行程序没有调试信息

为了节省磁盘空间，已经运行的程序通常没有调试信息。但如果又不能停止当前程序重新启动调试，那怎么办呢？还有办法，那就是同样的代码，再编译出一个带调试信息的版本。然后使用和前面提到的方式操作。对于attach方式，在attach之前，使用file命令即可：

```text
$ gdb
(gdb) file hello
Reading symbols from hello...done.
(gdb)attach 20829
```

### 小结

本节主要介绍了两种类型的GDB启动调试方式，分别是调试未运行的程序和已经运行的程序。对于什么样的程序能够进行调试也进行了简单说明。

[GDB调试入门指南 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/74897601)

[GDB break（b）：设置断点 (biancheng.net)](http://c.biancheng.net/view/8189.html)

[GDB watch命令：监控变量值的变化 (biancheng.net)](http://c.biancheng.net/view/8191.html)

## gdb各种指令

### info

### x查看内存

x/参数 <地址>  访问地址的内存，其实就是间接访问，也是很好用的指令，关于参数，s是输出为字符串，d为输出为十进制，x为输出为十六进制，b、w、l、q控制输出字节，默认是w，四字节，s字符串不受这个控制除外。

你可以使用***\*examine\****命令（***\*简写是x\****）来查看内存地址中的值。x命令的语法如下所示：

***\*x/<n/f/u> <addr>\****

n、f、u是可选的参数。

***\*n\****是一个正整数，***\*表示显示内存的长度\****，也就是说从当前地址向后显示几个地址的内容。
***\*f 表示显示的格式\****，参见上面。如果地址所指的是**字符串**，那么格式可以是**s**，如果 地址是指令**地址**，那么格式可以是***\*i\****。
***\*u 表示从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes\****。u参数可以用下面的字符来代替，b表示单字节，h表示双字节，w表示四字 节，g表示八字节。当我们指定了字节长度后，GDB会从指内存定的内存地址开始，读写指定字节，并把其当作一个值取出来。

<addr>表示一个内存地址。
n/f/u三个参数可以一起使用。例如：

命令：***\*x/3uh 0x54320\**** 表示，从内存地址**0x54320**读取内容，***\*h\****表示以双字节为一个单位，**3**表示三个单位，***\*u\****表示按十六进制显示。



输出格式
一般来说，GDB会根据变量的类型输出变量的值。但你也可以自定义GDB的输出的格式。例如，你想输出一个整数的十六进制，或是二进制来查看这个整型变量的中的位的情况。要做到这样，你可以使用GDB的数据显示格式：

***\**\*x 按十六进制格式显示变量。\*\*
\*\*d 按十进制格式显示变量。\*\*
\*\*u 按十六进制格式显示无符号整型。\*\*
\*\*o 按八进制格式显示变量。\*\*
\*\*t 按二进制格式显示变量。\*\*
\*\*a 按十六进制格式显示变量。\*\*
\*\*c 按字符格式显示变量。\*\*
\*\*f 按浮点数格式显示变量。\*\**\***
(gdb) p i
$21 = 101 

(gdb)***\*p/a i\****
$22 = 0x65

(gdb)***\*p/c i\****
$23 = 101 'e'

(gdb)***\*p/f i\****
$24 = 1.41531145e-43

(gdb)***\*p/x i\****
$25 = 0x65

(gdb)***\*p/t i\****
$26 = 1100101

====================================





【yasi】



1）用x命令查看内存

x/**3*****\*u\**\**h\**** 0x54320从地址0x54320开始，读取***\*3\****个双字节（***\*h\****），以十六进制方式显示（***\*u\****）

***\*3\****可以替换成任意正整数

***\*u\****可以替换成：

***\*d 按十进制格式显示变量\**
x 按十六进制格式显示变量
\**a 按十六进制格式显示变量
\**\**u 按十六进制格式显示无符号整型
\******o 按八进制格式显示变量
****t 按二进制格式显示变量
****c 按字符格式显示变量
****f 按浮点数格式显示变量**



***\*h\****可以替换成：

b表示单字节，h表示双字节，w表示四字 节，g表示八字节



2）p命令中加参数以不同形式打印变量的值

(gdb)**p/a i   或   (gdb)\**\*\*p/x i\*\**\***
$22 = 0x65

(gdb)***\*p/c i\****
$23 = 101 'e'

(gdb)***\*p/f i\****
$24 = 1.41531145e-43

(gdb)***\*p/t i\****
$26 = 1100101