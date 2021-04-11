# 原神 MIDI_OUT

## 1.0 开发记录

#### - 参与人员

***

    SPiCa AS s

***

#### - 食用指南

***

# 事先声明 可能存在封号风险

# 事先声明 可能存在封号风险

# 事先声明 可能存在封号风险

> 将.mid音乐文件，通过3MD转为.mml文件。将文件路径放入main方法中即可。
>
> 3ml下载地址：http://3ml.jp/index.html

> a站: https://www.acfun.cn/v/ac27362776
> 
> b站: https://www.bilibili.com/video/BV1xh411D77p/

***

#### - 运行流程

***

    1.读取部分
    读取.mml文件.
    取出.mml文件中的音轨数据，去除头尾数据。或者List<音轨字符串>集合.
    for(String str:List<音轨字符串>){
        将字符串转为按键，存入按键队列中。
    }
    获得List<Queue<按键>>集合.

    2.输出部分
    创建多线程,每个线程获得一个Queue<按键>队列。
    ...(此处存在一些转换操作)
    线程全部创建完毕后，同时启动所有线程.
    while(Queue<按键>不为空){
        输出队列元素（使用dll驱动输出
    }

    完毕。
***
#### - ~~亿点细节~~
#### - 一点细节
***

1. 输出问题
   - java自带的输出工具类存在一些局限性，输出可能被其他程序禁用。例如 java.awt.Robot 工具类，拟按键的方法是keyPress()
   底层调用是c++的方法。深入查看了之后发现这段java代码调用的c++代码中调用的是windows.h中SendInput方法。此方法会被一些程序
   屏蔽掉，因为一般输出设备进行输出是没有系统方法调用的。或者进行更深层次的模拟，例如达到ring0级别的输出模拟。
   - 因此原因，所以需要一些更深层次的模拟，但是java代码不能过多涉及操作系统，此时需要调用一些类库来支援，通过java的jna
   来调用一些动态类库。我这里采用的是DD64驱动，winRing0也可以进行模拟按键，但win10似乎不行。此处暂时没有过多排查。

2. mid以及mml的问题
   - mid是一种音乐文件的格式，内含纯净的乐器音乐音轨。mml是一款能更方便看清以及编辑mid的文件格式。
   - .mml的一些参数
       - C D E F G A B 分别代表简谱中的 1 2 3 4 5 6 7
       - 数字代表是什么类型的音符，4就代表是四分之一音符
       - 黑键使用+-符号来标识,
       - .代表附点音符
       - r代表休止符
       - v代表音量，
       - o代表音域，默认是4，
       - l代表默认音符小节，默认是4，
       - t代表bpm，默认是120，
       - n是固定按键n12 = o1c，以此类推
       - <>的意思是在降低或提高默认音域
> 可以写一段简单的mml，例如小星星的旋律：
> 
> > ccggaagrffddssa

***
## 1.1 改动记录
***
| Date | Author | Content | Schedule | 
| :----:| :----:| :----: | :----: |
| 03-27-2021 | s | 项目搭建 | 文件读取模块，多线程输出模块
| 03-31-2021 | s | demo | 初始功能完成，存在部分问题
| 04-11-2021 | s | 问题记录 | 由于按键次数不同，导致会出现部分时间差。准备修复此问题
***

