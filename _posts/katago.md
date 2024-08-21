---
title: KataGo 安装
date: 2022-12-15 09:34:27
tags: AI
categories: Software
---
现在开源的最强围棋AI。elo大概13k+？
<!-- more -->
源代码可用。 [Github link](https://github.com/lightvector/KataGo).
## 获取模型文件
如果你有现代GPU（比如GCN1.0/Volta往后的），我建议用 OpenCL 版本。核显也是能跑的。
没卡或者卡太老的话，用 Eigen 这个用 CPU 跑的版本。还有支持 AVX2 的，要求4代 Core 以后了。
CUDA 版本配置过于繁琐，而且必须要用N卡。
Windows:
{% btn https://github.com/lightvector/KataGo/releases/download/v1.14.0/katago-v1.14.0-opencl-windows-x64.zip, openCL %}
&&
{% btn https://github.com/lightvector/KataGo/releases/download/v1.14.0/katago-v1.14.0-eigenavx2-windows-x64.zip, Eigen AVX2 %}

Linux:
{% btn https://github.com/lightvector/KataGo/releases/download/v1.14.0/katago-v1.14.0-opencl-linux-x64.zip, openCL %}
&&
{% btn https://github.com/lightvector/KataGo/releases/download/v1.14.0/katago-v1.14.0-eigenavx2-linux-x64.zip, Eigen AVX2 %}
**把这些解压，一会要用**
## 下 KaTrain
官方推荐的 KataGO GUI
![](../img/katago/1.png)
{% btn https://github.com/sanderland/katrain, Github Link %}
三平台运行支持，但是
* 官方没有 Linux 版，需要自己编译;
* macOS 版本不支持 aarch64，要用 Rosetta 2 转译

Windows:
{% btn https://github.com/sanderland/katrain/releases/download/v1.14.0/KaTrain.exe, Download %}
解压然后运行 `KaTrain.exe` ->
![](../img/katago/2.png)
打开菜单，选择 `General & Engine Settings` ->
![](../img/katago/3.png)
把模型路径放到`Path to KataGo executable`里.

## 还有
`Maximum number of visits in analysis`越大，这个 AI 越强
![](../img/katago/4.png)