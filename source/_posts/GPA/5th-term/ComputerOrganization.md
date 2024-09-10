---
title: ComputerOrganization
categories:
  - GPA
  - 5th-term
category_bar: true
---

## 计算机组成原理

## 前言

学科地位：

| 主讲教师 | 学分配额 | 学科类别 |
| :------: | :------: | :------: |
|  闫文珠  |   3.5    |  自发课  |

成绩组成：

| 平时 | 期中 | 期末 |
| :--: | :--: | :--: |
| ？%  | ？%  | ？%  |

教材情况：

|    课程名称    |         选用教材         | 版次 |  作者  |     出版社     |      ISBN号       |
| :------------: | :----------------------: | :--: | :----: | :------------: | :---------------: |
| 计算机组成原理 | 《计算机组成与系统结构》 |  3   | 袁春风 | 清华大学出版社 | 978-7-302-59988-3 |

最基本的知识普及：

{% fold light @最基本的知识普及 %}

- :tv: [【硬核科普】从零开始认识主板](https://www.bilibili.com/video/BV1xQ4y1b7JS/)
- :tv: [【硬核科普】从零开始认识显卡](https://www.bilibili.com/video/BV1xE421j7Uv/)

**主板（Motherboard）**

- **作用**：主板是所有硬件的连接平台，它连接并协调 CPU、内存、存储设备、显卡、网络设备和其他外部接口。主板上还包含芯片组，负责管理数据传输和设备间的通信。
- **与其他部分的关系**：主板是各个硬件组件的核心枢纽，确保数据能够在各个组件之间高效传输和通信。

![机箱视角中的主板](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102038540.png)

![主板的 6 大区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102039187.png)

**输入/输出设备（I/O Devices）**

- **作用**：输入设备（如键盘、鼠标、触摸屏）用于用户与计算机的交互。输出设备（如显示器、打印机）用于显示计算机的操作结果。网络设备（如网卡、Wi-Fi模块）则允许电脑连接到网络。
- **与其他部分的关系**：输入设备通过主板将用户的操作传递给CPU进行处理，输出设备则从CPU或显卡接收处理后的数据进行展示。

![I/O 区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102039284.png)

**中央处理器（CPU）**

- **作用**：CPU是电脑的大脑，负责执行计算任务和指令。它从内存中读取指令，进行运算，然后将结果写回内存。现代CPU具有多个核心，可以并行处理多项任务，提高计算效率。
- **与其他部分的关系**：CPU与内存和存储设备密切配合。它从内存中获取数据和指令，然后进行处理，最终将结果写回内存或存储设备。

![CPU 区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102040689.png)

**内存（RAM）**

- **作用**：内存是计算机中的临时存储器，用于存储当前运行的程序和数据。它的访问速度极快，但数据在断电后会丢失。内存用于支持CPU的高速操作。
- **与其他部分的关系**：内存是CPU执行任务时的工作区域，存储从存储设备（如硬盘）中读取的数据和指令。内存的数据会在需要时被写回到存储设备中。

![内存区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102040651.png)

**显卡（GPU）**

- **作用**：显卡负责处理图形计算任务，特别是在图像和视频渲染、游戏以及某些并行计算任务中（如深度学习）。高性能显卡能够加速这些计算任务。
- **与其他部分的关系**：显卡通过主板与CPU和内存通信，从内存中读取图形数据，并进行处理，然后将渲染结果输出到显示器。

**存储设备（Storage Devices）**

- **作用**：存储设备如机械硬盘 hard disk (HDD)、固态硬盘 solid state disk (SSD)，用于长期保存数据和程序，即使在电脑断电后数据也不会丢失。SSD的速度比传统硬盘更快，因此越来越常用。
- **与其他部分的关系**：存储设备保存操作系统、应用程序和用户数据。程序执行时，数据会从存储设备加载到内存中供CPU使用。

**网卡（Network Interface Card, NIC）**

- **作用**：网卡是计算机与网络之间的桥梁，负责处理计算机与网络间的数据通信。它将计算机的数据转换为网络信号，以太网网卡通过有线或无线方式连接局域网（LAN）或互联网。
- **与其他部分的关系**：网卡通过主板与南桥芯片组或PCIe总线连接，接收来自CPU的数据，经过处理后通过网络接口发送到外部网络。同时，它也接收来自网络的数据，传递给CPU进行处理，确保计算机能够与其他网络设备进行通信。

![扩展区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102040086.png)

**南桥芯片组（Southbridge）**

- **作用**：南桥芯片组是主板上的一个重要芯片，为了避免过多的模块与 CPU 直连导致主板排线困难以及布局问题，产生了南桥芯片组用于负责管理较慢的外围设备的连接和数据传输。包括USB接口、SATA接口、网络接口、音频设备和其他I/O设备。南桥还处理BIOS、系统时钟、硬盘和光驱的控制。
- **与其他部分的关系**：南桥通过主板与CPU和北桥芯片组（或直接与CPU）连接，接收来自CPU的指令，并将其转化为外设可以理解的信号。它还通过I/O总线连接各种外设，确保数据在外围设备与系统之间顺畅传输。

![南桥芯片组区域](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102040366.png)

**BIOS（Basic Input/Output System）**

- **作用**：BIOS是主板的固件程序，它存储在主板的 BIOS 芯片里。负责在计算机启动时进行硬件初始化，指导所有的硬件运行，并启动操作系统。
- **与其他部分的关系**：BIOS在开机时检查并初始化硬件，然后引导操作系统，从而开始系统的工作。

![BIOS 芯片](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202409102040350.png)

{% endfold %}