---
layout:     post
title:      Linux的boot目录
date:       2019-12-31
author:     白板
catalog: true
tags: 
    - Linux
    
---

# /boot

## Kernel 的配置文件

## 启动管理程序GRUB的目录

- ps GRUB简介

GNU GRUB是一个非常强大的引导加载程序，是计算机启动时运行的第一个程序，它负责将控制加载转移到操作系统内核软件。

GRUB起源于1995年，当时Erich Boleyn试图用犹他大学的Mach 4微内核（现在称为GNU Mach）启动GNU Hurd。埃里希和布莱恩·福特设计了多重引导规范，因为他们决定不加入大量的互不兼容的PC的启动方式。

## Initrd文件

是系统启动时的模块供应的主要来源；

我们在uboot的环境变量里经常看到root=/dev/mtdblock3这样的字样，我们知道这个是用来指定我们最后要挂载的文件系统的位置的。
但是问题出现了，/dev/mtdblock3是指文件系统当中的一个文件，但是在挂载文件系统之前根本还没有目录和文件和文件的概念，典型的先有鸡还是先有蛋的问题。
挂载文件系统需要目录文件，有目录文件的前提是要挂载了文件系统。
这样肯定是不行的。所以我们就要现提供一只“鸡”，就是文件系统，让我们能找到/dev/mtdblock3这个文件，然后再去挂载其上面的文件系统。
那么又有问题了，我们是通过什么方法和步骤提供的那只“鸡”（文件系统）的？
这就是下面要讲的initrd机制和initramfs机制

这两个机制的原理和作用有很大的相似：
1. 在挂载真正的文件系统之前，它们会挂载一个基于内存的虚拟文件系统，这个文件系统只是起个过度的作用，为了挂载真正的文件系统（root=/dev/mtdblock3指定位置的文件系统）做铺垫的。
2. 在挂载虚拟文件系统后都会执行一个可执行文件，initrd机制会执行虚拟文件系统根目录下的linuxrc或文件，initramfs会执行虚拟文件系统根目录下的init文件。
3. 使用这两种机制还有一个好处：现在文件系统可以存在多种介质上，每一种介质都有相应的驱动，如果把这些驱动全部编进内核，势必造成内核十分庞大。
引入了这两种机制后，我们可以把各种驱动编译成模块，然后在启动的时候根据检测到的硬件来插入模块驱动。
插入模块驱动这个动作就是在上面的linuxrc或init可执行文件中。
插入模块之后，就有了操作硬件的驱动，然后就可以进行真正的文件系统挂载了。

## System.map文件时系统Kernel中的变量对应表；

## vmlinuz是在启动过程中最重要的一个文件，因为这个文件就是实际系统所使用的kernel。

# 参考

[Linux boot 目录介绍](https://www.cnblogs.com/the-tops/p/7871933.html)

[Linux的initrd与initramfs机制](http://www.360doc.com/content/17/0926/16/18252487_690345148.shtml)

[Windows系统启动过程详解](https://blog.csdn.net/xiongyouqiang/article/details/79360459)


