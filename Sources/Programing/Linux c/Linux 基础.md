---
date created: 2024-07-28
id: linux 编程
tags:
  - 编程
title: Linux编程基本函数知识
---

### 首页：[[网络编程]]

### 系统知识

在linux下，每个进程在内核中都有一个<mark style="background: #FF5582A6;">进程控制块（PCB）</mark>来维护进程相关的信息，<mark style="background: #FF5582A6;">task_struct结构体</mark>，就代表了这个进程控制块，也代表了进程的所有标识信息。一般情况下在/usr/src/linux-headers-4.4.0-96/include/linux/sched.h文件下可以查看到这个结构体。

该结构体包括但不限于以下属性：

- 进程id。系统中每个进程有唯一的id，在C语言中用pid_t类型表示，其实就是一个非负整数。
- 进程的状态，有就绪、运行、挂起、停止等状态。
- 进程切换时需要保存和恢复的一些CPU寄存器。
- 描述虚拟地址空间的信息。
- 描述控制终端的信息。
- umask掩码。
- 文件描述符表，包含很多指向file结构体的指针。
- 和信号相关的信息。
- 用户id和组id。
- 会话（Session）和进程组。
- 进程可以使用的资源上限（Resource Limit）。

除此之外，一般而言，linux下的<mark style="background: #FF5582A6;">每个程序都拥有3GB的用户区，1GB的内核区</mark>，PCB就在内核区中，这与系统内存有关。但并不是说进程完全占有这些空间，这是一种内存资源的抽象，进程以为自己有这么多资源，但实际上不是。

进程整体的进程空间是：

- 0-3G的用户区：.data，.bss，.txt，动态库加载区，堆，栈，命令行参数，环境变量
- 3-4G的内核区：包括但不限于PCB

### fork 函数

描述：该函数会<mark style="background: #FF5582A6;">创建一个子进程</mark>，注意<mark style="background: #FF5582A6;">子进程和父进程的用户区是完全相同的</mark>，但是<mark style="background: #FFB86CA6;">内核区不完全相同</mark>，例如父子进程的PID就不一样。换言之，用户区的数据是相同的，而<mark style="background: #FF5582A6;">内核区中的部分数据是相同的</mark>，如文件描述符表、标准输入、输出等，这也能说明，当父进程先退出时，但控制台却依然存在一种运行的假象。

返回值：父进程会返回子进程的pid，而子进程会返回0。

二者的执行逻辑是，谁先抢到时间片，谁就执行。

### 僵尸进程和孤儿进程

父进程假如先退出，那么创建出来的子进程就无法管理，因此也就<mark style="background: #FF5582A6;">无法回收子进程的资源</mark>，这个进程也就变成<mark style="background: #FF5582A6;">孤儿进程</mark>。因此1号Init进程就会领养这个孤儿进程。

子进程如果执行完毕，但是<mark style="background: #FF5582A6;">父进程却没有调用</mark>[[Linux 基础#进程回收函数|进程回收函数]]<mark style="background: #FF5582A6;">来回收子进程的资源</mark>，那么子进程就会变成<mark style="background: #FF5582A6;">僵尸进程</mark>。 杀死父进程后，使僵尸进程变成孤儿进程，由1号Init进程回收这个子进程。

### 父子进程共享数据？

读共享，写复制！ 具体通信机制，涉及到[[进程间通信]]知识。具体的共享数据的双方为有血缘关系的进程，这涉及到[[Linux 基础#fork 函数|fork函数]]。

### exec函数族

描述：通过exec系列函数，在一个进程里面执行其他bash命令。

- int execl(const char \*path, const char \*arg, … \/* (char  \*) NULL \*/);
- (1) path: 要执行的程序的绝对路径。(2) arg:占位，写应用程序的名字。(3) 变参arg: 通常要执行的程序的需要的参数。(4) arg后面的: 命令的参数。(5) 参数写完之后: NULL。
- 该函数会在当前地址空间内，加载指定命令的代码段，数据段，堆段，栈段等，类似狸猫换太子。例如execl("/bin/", "ls", "my-command-ls", "-l", NULL);
- 请注意，如果当前函数执行成功，那么程序就运行结束了，因为代码段、数据段等已经被替换掉了。如果执行失败，会继续执行后面的代码。
---
- int execlp(const char \*file, const char \*arg, … \/* (char  \*) NULL \*/);
- 这个函数的功能同上，只不过这个函数可以不添加命令的位置，因此这个函数搜索的是环境变量，而上个函数是从指定的path中寻找。execl("./user-program", "program", "xxx", , NULL);

### 进程回收函数

- pid_t wait(int \*status);
- 函数描述：阻塞并等待子进程退出， 获取子进程结束状态(退出原因)。
- 返回值：失败返回-1，表示没有子进程，成功就返回清理掉的子进程ID。
- 指针类型的参数，一般是传出参数。
- 获取状态，WIFEXITED(status)为非0 表示 进程正常结束；
- WEXITSTATUS(status)：获取进程退出状态
- WIFSIGNALED(status)：为非0表示 进程异常终止
- WTERMSIG(status)：取得进程终止的信号编号。
---
- pid_t waitpid(pid_t pid, int \*status, in options);
- 函数描述：功能同wait函数，并且本函数默认也是阻塞函数，通过option设置为WNOHANG，可使该函数变为非阻塞函数。
- 返回值：>0 表示 返回的是回收掉的子进程ID；-1 表示 无子进程；=0：且只要参数3为WNOHANG，那么就代表子进程正在运行。
- 参数：如PID为 -1, 那么表示等待任一子进程；若PID > 0，那么表示等待与之PID相同的进程；如果PID = 0，那么表示待回收的子进程的ID是与等待进程组相同的进程，即与任何和调用waitpid()函数的进程在同一个进程组的进程。

[[Linux 基础#首页： 网络编程|返回到开头]]
