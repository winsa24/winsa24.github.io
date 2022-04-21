---
title: Course Design of Computer Hardware
date: 2019-03-01

description: "Engaged in the project of ‘Intelligent home system’, learnt Android development and knowledge about IP, been responsible for the encapsulation of IP and whole construction of the showing App."
categories: [Android Develop]
tags: [Android, BJUT]

draft: false
enableDisqus : true
enableMathJax: false
disableToC: false
disableAutoCollapse: true

---
# SmartHome App
A small tool for real-time monitoring of indoor air quality is designed, which can display the current indoor temperature and humidity, formaldehyde concentration and smoke. And realize the analysis of air quality over a period of time on the mobile phone and provide suggestions for improvement.


## Function description
Smoke sensor and formaldehyde sensor obtain their respective data and pass it to the development board;
The mobile phone can be connected to the development board via software using Bluetooth, and receive real-time data from the board;
The software can process the air quality data, draw a line graph, and provide it to users for analysis.

## Platforms and modules
The development board selected is PYNQ-Z1, which uses HC-05 embedded Bluetooth serial communication module, ZE08-CH2O formaldehyde module, ZH03 laser dust sensor module, Zhongjingyuan Electronics 2.42 inch OLED display module and digital temperature and humidity sensor DHT11 module . The choice of these sensors is to obtain indoor air quality data, the Bluetooth module is used as the connection between software and hardware, and the display module is the display in terms of hardware.

![homepage](/images/projects/hardware/homepage.jpg) ![report](/images/projects/hardware/report.jpg)
![subpage](/images/projects/hardware/data1.jpg) ![subpage](/images/projects/hardware/data2.jpg)

Check the [video](/images/projects/hardware/sh.mp4) to better learn how the App works.
[![Watch the video](/images/projects/hardware/homepage.jpg)](/images/projects/hardware/sh.mp4)
