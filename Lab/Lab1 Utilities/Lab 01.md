# Lab01 Util

几个lab，根据第一章讲述的system call实现一些功能。

## Sleep

### Goal

Implement the UNIX program `sleep` for xv6; your `sleep` should pause for a user-specified number of ticks. A tick is a notion of time defined by the xv6 kernel, namely the time between two interrupts from the timer chip. Your solution should be in the file `user/sleep.c`.

在/user中实现sleep.c文件。

+ sleep should pause for a user-specified number of ticks
+ tick是定时器芯片两次中断(interrupt)之间的时间

理解为检测到tick就发出一个pause的动作？

### hints

+ 根据Lec01的任务，先阅读xv6 book的Chapter 1
+ 去看/user文件中的一些C文件的源码，see how can obtain the **command-line arguments** passed to a program.
+ If the user forgets to pass an argument, sleep should print an **error** message.  
  + 没有这玩意儿就会报错
+ The command-line argument is passed as a string; you can convert it to an integer using `atoi` (see user/ulib.c).
  + 命令行参数作为字符串传递，可以降低转换为int
+ Use the system call `sleep`. 
  + 利用系统调用sleep程序
+ Make sure `main` calls `exit()` in order to exit your program. 
  + 记得退出的时候要有exit()，提醒不要犯基础的错误
+ Add your `sleep` program to `UPROGS` in Makefile; once you've done that, `make qemu` will compile your program and you'll be able to run it from the xv6 shell.
+ Look at Kernighan and Ritchie's book *The C programming language (second edition)* (K&R) to learn about C.

### Step

+ 所以要先找到这个arguments如何获得

