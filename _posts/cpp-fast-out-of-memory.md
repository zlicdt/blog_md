---
title: 如何用 C++ 快速撑爆内存？
date: 2009-03-16 04:23:36
tags: C++
categories: Programming
---
节约内存，请（（
<!-- more -->
## Codes
```c++
void nya() {
    while(true) {
        int* ptr = new int;
    }
}
int main() {
    nya();
    return 0;
}
```
## 原理
**反复创建新指针并分配一些内存空间。由于 `while(true)` 是无限循环的，内存会很快被填满。**
{% note warning %}
由于这个程序会占满内存，不会自动`kill`进程的操作系统（如Windows、macOS）将使用`swap`进行内存交换。
这可能导致大量写入磁盘！
在Windows上，它会导致系统不稳定甚至出现黑屏。在严重情况下，未保存的数据可能会丢失并且可能出现蓝屏。请确保在运行此程序之前已保存所有数据。
{% endnote %}
## 现象
**在现代Linux发行版（x86_64 Linux-6.6.1-ARCH），当内存满时，进程将被终止。
在macOS（arm64 Darwin 22.1.0）上，它将占用`swap`到最大限制，并且进程将不会被终止。
Windows（x86_64 NT10）不会终止进程，在内存满时屏幕会变黑。可能已经无法给核显驱动内存了罢（笑**
