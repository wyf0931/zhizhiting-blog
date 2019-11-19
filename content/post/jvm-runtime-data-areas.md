---
title: JVM 运行时数据区介绍
author: Scott
tags:
  - Java
  - JVM
categories: []
date: 2019-07-02 14:47:00
---
JVM 定义了在执行程序期间使用的各种运行时数据区（Run-Time Data Areas），其中一些数据区的生命周期与 JVM 进程相同，其他数据区是每个线程拥有一个，其生命周期与线程的生命周期相同。
<!--more-->

### 程序计数器（The pc Register）
JVM 可以支持多线程同时运行。**每个 JVM 线程都有一个 pc（程序计数器）寄存器。**在任何时候，每个 JVM 线程正在执行一个方法的代码，即该线程的当前方法([§2.6](http://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6))。如果该方法不是 *native* 的，则 pc 寄存器包含当前正在执行的 JVM 指令的地址。如果当前由线程执行的方法是 *native* 的，pc寄存器的值是空。pc 寄存器存储空间最少需要保存一个 *returnAddress* 或与平台相关的本地指针的值。

{% note %}
`returnAddress` 类型由 JVM 的 `jsr`、`ret` 、`jsr_w` 指令使用。 `returnAddress` 类型的值是 JVM 指令操作码的指针。与原始数字类型不同，`returnAddress` 类型与任何 Java 编程类型都不同，且不能被正在运行的程序修改。
{% endnote %}

### 堆（Heap）
JVM 有一个在所有 JVM 线程之间共享的堆（heap）。堆是运行时数据区域，从中分配所有类实例和数组的内存。

**堆在 JVM 启动时创建**。垃圾回收器（garbage collector）回收对象的堆存储空间；对象永远不会被显示地释放。JVM 假定没有特定类型的垃圾回收器，可以根据实现者的系统要求选择垃圾回收策略。**堆大小可以固定也可以动态扩展**，如果不需要较大的堆，则堆可以收缩。**堆的内存不需要是连续的。**

{% note warning%}
若程序所需的堆大小超出设定的最大值，则 JVM 将抛出 `OutOfMemoryError`。
{% endnote %}

### 栈（Stacks）
**每个 JVM 线程都有一个专用的 JVM 堆栈，在线程创建时创建**。 JVM 堆栈存储的数据为帧（frames）。 JVM 堆栈类似于常规语言的堆栈，例如C：它保存局部变量和部分结果，并在方法调用和返回中起作用。因为除了push 和 pop 帧之外，JVM 堆栈不会被直接操作，所以可以在堆中分配帧。**JVM 堆栈的内存不需要是连续的**。

{% note %}
在Java® 虚拟机规范的第一版中，Java虚拟机堆栈被称为Java堆栈。
{% endnote %}

允许 JVM 堆栈大小固定或根据需要动态扩展和收缩。如果 JVM 堆栈具有固定的大小，则可以在创建堆栈时单独选择每个 JVM 堆栈的大小。

{% note %}
JVM 提供了可以控制初始 JVM 堆栈大小的参数，可以通过设定最大和最小堆栈大小来实现动态扩展或收缩堆栈大小。
{% endnote %}

{% note warning %}
如果线程执行过程中需要的堆栈大小超出了设定的最大值，则 JVM 将抛出 `StackOverflowError`。
{% endnote %}

{% note warning %}
如果 JVM 堆栈可以动态扩展，并且尝试扩展，但是可用内存不足，或如果可用内存不足以为新线程创建初始 JVM 堆栈，则JVM 抛出 `OutOfMemoryError`。
{% endnote %}