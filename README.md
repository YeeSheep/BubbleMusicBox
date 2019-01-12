# BubbleMusicBox
## 介紹

這是一個簡單的吹泡泡音樂機器，操作方法如下：
1. 先將泡泡水(濃度需調為較濃)放入置前方盒子中。
2. 將主程式打開，並手動開啟右邊風扇按鈕。
3. 在盒子下方有個按鈕打開。
- 完成以上三個步驟就會有邊吹泡泡邊唱歌了 ~ 是不是很簡單呢?恩真的很簡單 ~ 

## 設備
- Raspberry Pi 3
- 光照度感測模組
- 伺服馬達
- 蜂鳴器
- LED 
- 風扇
- 麵包版
- Arduino
- 紙箱
- 吹泡泡玩具
- LED小燈泡
##結構原理
- Arduino版 : 在底部感光元件
## 線路圖
- Arduino  
![image](https://ppt.cc/fI4Tfx@.png)
- Raspberry Pi
![image](https://ppt.cc/fIgVZx@.png)
## 成品
-內部線路
![image](https://ppt.cc/fMxmSx)
## 程式說明
```
-*- coding: UTF-8 -*-
/!/usr/bin/python
import smbus
import time
import RPi.GPIO as GPIO



CONTROL_PIN = 27
PWM_FREQ = 50
STEP=15


control = [5,10]

servo = 4

GPIO.setmode(GPIO.BCM)
GPIO.setup(CONTROL_PIN, GPIO.OUT)

pwm = GPIO.PWM(CONTROL_PIN,50)
pwm.start(2.5)



num = 0
LED = 11
GPIO.setup(LED,GPIO.OUT)


def main():

 while True:
    num = readLight()
    print "Light Level : " + str(num) + " lx"
    if num >200:
       GPIO.output(LED,GPIO.HIGH)

       for x in range(2):
         pwm.ChangeDutyCycle(control[x])
         time.sleep(0.5)
         print x

       for x in range(0,0,-1):
         pwm.ChangeDutyCycle(control[x])
         print x

    else:
       GPIO.output(LED,GPIO.LOW)
       time.sleep(0.2)
if __name__=="__main__":
   main()

   pwm.stop()
   GPIO.cleanup()
```
## 參考來源
1. 光照度感測 http://atceiling.blogspot.com/2017/03/raspberry-pi-gy30.html
2. 吹泡泡機 https://learn.adafruit.com/make-it-bubble/sub-assemblies
## 工作分配
- 採購元件--張真
- 系統功能 :
1. 蜂鳴器-- 張真
2. 馬達-- 楊宜明
3. 感光元件-- 楊宜明
- 音樂盒製作-- 張真、楊宜明
- 文件撰寫-- 劉家瑜、楊宜明
- 報告Demo-- 劉家瑜、楊宜明
