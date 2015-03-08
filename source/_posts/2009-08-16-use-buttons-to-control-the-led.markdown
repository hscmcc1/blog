---
author: hesicong
comments: true
date: 2009-08-16 11:59:45+00:00
layout: post
slug: use-buttons-to-control-the-led
title: 使用按键来控制LED
wordpress_id: 1926
categories:
- 硬件
tags:
- LED
- mini2440
- 嵌入式Linux应用开发完全手册
---

适用于MINI2440的实例3程序：修改自《嵌入式Linux应用开发完全手册》P84
MINI2440的LED和书中所说LED接口一致，分别是：
  * LED1,GPB5
  * LED2,GPB6
  * LED3,GPB7
  * LED4,GPB8
而按键不同，根据MINI2440的说明书和电路图可知：
  * K1, GPG0
  * K2, GPG3
  * K3, GPG5
  * K4, GPG6
  * K5, GPG7
  * K7, GPG11
而书中介绍的按键的通用输入输出口不一样，所以整个程序修改如下：

``` c
/* key_led.c */
#define GPBCON      (*(volatile unsigned long *)0x56000010)
#define GPBDAT      (*(volatile unsigned long *)0x56000014)

#define GPGCON      (*(volatile unsigned long *)0x56000060)
#define GPGDAT      (*(volatile unsigned long *)0x56000064)

/*
 * LED1-4对应GPB. GPB. GPB. GPB8
 */
#define GPB5_out        (1<<(5*2))
#define GPB6_out        (1<<(6*2))
#define GPB7_out        (1<<(7*2))
#define GPB8_out        (1<<(8*2))

/*
 * K1-K4对应GPG. GPG. GPG. GPG6
 */
#define GPG0_in    ~(3<<(0*2))
#define GPG3_in     ~(3<<(3*2))
#define GPG5_in     ~(3<<(5*2))
#define GPG6_in     ~(3<<(6*2))

int main()
{
        unsigned long dwDat;
        // LED1-LED4对应的4根引脚设为输出
        GPBCON = GPB5_out | GPB6_out | GPB7_out | GPB8_out ;

        // K1-K4对应的4根引脚设为输入
        GPGCON = GPG0_in & GPG3_in & GPG3_in & GPG6_in;

        while(1){
            //若Kn为0(表示按下)，则令LEDn为0(表示点亮)
            dwDat = GPGDAT;             // 读取GPG管脚电平状态

            if (dwDat & (1<<0))        // K1没有按下
                GPBDAT |= (1<<5);       // LED1熄灭
            else
                GPBDAT &= ~(1<<5);      // LED1点亮

            if (dwDat & (1<<3))         // K2没有按下
                GPBDAT |= (1<<6);       // LED2熄灭
            else
                GPBDAT &= ~(1<<6);      // LED2点亮

            if (dwDat & (1<<5))         // K3没有按下
                GPBDAT |= (1<<7);       // LED3熄灭
            else
                GPBDAT &= ~(1<<7);      // LED3点亮

            if (dwDat & (1<<6))         // K4没有按下
                GPBDAT |= (1<<8);       // LED4熄灭
            else
                GPBDAT &= ~(1<<8);      // LED4点亮
    }

    return 0;
}
```

