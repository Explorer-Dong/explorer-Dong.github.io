---
title: idle-self-config
categories: 
  - DevTools
  - IDLE
category_bar: true
---

## IDLE 偏好配置

由于连续两届篮球杯都是 CA 省二，迫于加分压力不得不转 PA 组（其实是摸鱼不刷题导致的）。阅读篮球杯 py 组手册后发现只能使用 3.8.6 版本且编辑器只能用自带的 IDLE，故特此记录一下相关配置，顺便说点题外话

### 编写文件

在双击快捷方式后会弹出一个 shell 窗口，支持简单的交互式编码：

![快捷方式](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091642746.png)

我们点击左上角的 File 即可进行新建或打开 `.py` 文件的操作：

![image-20240609164334223](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091647404.png)

### 修改样式

在 shell 或编辑界面中均有 Options 选项，点击后进行个性化定制：

![Options 选项](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091647404.png)

「字体部分」选择 consoles 字体、13 号大小、4 个标准空格的 tab：

![选择 consoles 字体、13 号大小、4 个标准空格的 tab](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091649022.png)

「高亮部分」选择 New 主题：

![选择 New 主题](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091650602.png)

「快捷键」部分，可修改运行模块快捷键为 Ctrl+F5 从而和 CLion 对应，当然也可以保持默认的 F5：

![修改运行模块快捷键为 Ctrl+F5 从而和 CLion 对应](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202406091651415.png)

### 题外话

为什么直接点击 `pythonw.exe` 无法打开 IDLE？说实在的困扰好久了，因为这个原因就没打开过所谓的 python 自带的编辑器，惭愧。搜遍 Google 和 Baidu 都未能找到准确的答案，比较多的是：因为 pythonw.exe 就没打算让你双击打开它，所以只能通过快捷方式打开？？？