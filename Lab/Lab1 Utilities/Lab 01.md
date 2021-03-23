# Lab01 Util

## 前言

### Do what?

+ 首先理解这些经典的lab到底是要我们做什么，不然还是一头雾水。

+ 进入xv6-riscv-2020项目后可以看到两个比较重要的目录：
  + kernel为xv6内核源码，里面除了os工作的核心代码（如进程调度），还有向外提供的接口（system call）；
  + user中则是用户程序，如我们熟悉的`ls`，`echo`命令等。本次实验的目的就是`在user中增加用户程序`，借助`kernel`中提供的`system call`来实现所需的功能。

+ 几个任务，根据第一章讲述的system call实现一些功能。

### Help Understadning

+ 在 `/xv6-riscv-2020` 终端目录下执行 `make qemu` ，启动xv6 kernel
+ 输入一些 Shell 命令 `ls`, `echo` 可以看到其执行的功能，与 Linux 下的功能类似。
+ 可以先观察`/xv6-riscv-2020/user` 中的各类 `.c` 文件，了解这些用户命令实现的逻辑，引用的头文件等，便于理解整个结构
+ Lab 目的就是在自己在 `/user` 中编写程序以实现相应的功能

## Boot xv6

```bash
make qemu
```

Note:

```bash
Ctrl-p: print process information
Ctrl-a x: quit qemu type
```

## Sleep

### I. Goal

Implement the UNIX program `sleep` for xv6; your `sleep` should pause for a user-specified number of ticks. A tick is a notion of time defined by the xv6 kernel, namely the time between two interrupts from the timer chip. Your solution should be in the file `user/sleep.c`.

在/user中实现sleep.c文件。

+ sleep should pause for a user-specified number of ticks
+ tick是定时器芯片两次中断(interrupt)之间的时间

理解为检测到tick就发出一个pause的动作？

### II. Hints

+ 根据Lec01的任务，先阅读xv6 book的Chapter 1
+ 去看/user文件中的一些C文件的源码，see how can obtain the **command-line arguments** passed to a program.
+ If the user forgets to pass an argument, sleep should print an **error** message.  
  + 没有这玩意儿就会报错
+ The command-line argument is passed as a string; you can convert it to an integer using `atoi` (see user/ulib.c).
  + 命令行参数作为字符串传递，可以降低转换为int
+ Use the system call `/user/sleep.c` you created. 
+ Make sure `main` calls `exit()` in order to exit your program. 
  + 记得退出的时候要有exit()，提醒不要犯基础的错误
+ Add your `sleep` program to `UPROGS` in Makefile; once you've done that, `make qemu` will compile your program and you'll be able to run it from the xv6 shell.
+ Look at Kernighan and Ritchie's book *The C programming language (second edition)* (K&R) to learn about C.
+ xv6中一个tick为100ms

### III. Step

+ 观察`/user`中的一些源码，在`sleep.c`文件中引入所需要头文件
+ 接受传入 main 函数的参数，并调用 sleep 函数。
+ 退出时记得设置 `exit()`

### IV. Code

```c++
#include "kernel/types.h"
#include "kernel/stat.h"
#include "user/user.h"

#define STDIN_FILENO  0   
#define STDOUT_FILENO 1
#define STDDER_FILENO 2

int main(int argc, char *argv[]){
  if(argc != 2){
      fprintf(2, "usage: sleep time\n");  //argc!=2返回错误或退出，是为了限制参数个数为2
      exit(1);
  } else {
      int time = atoi(argv[1]);  //string -> int
      sleep(time);  //just call sleep 
      exit(0);
  }
}
```

