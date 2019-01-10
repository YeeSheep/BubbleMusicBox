# BubbleMusicBox
## 介紹
將光感測器連接到樹梅派，然後把裝置放在盒子裡，利用盒子的開闔判斷有沒有光源。如果光大於200，LED會亮，泡泡機會啟動吹泡泡，音樂也會播出來。當沒有光的時候，LED不亮，泡泡機停止，音樂也會停止。
## 設備
- Raspberry Pi 3
- 光照度感測模組
- 伺服馬達
- 蜂鳴器
- LED 
- 風扇
- 麵包版
## 程式說明
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
ˋˋˋ
## 參考來源
1. 光照度感測 http://atceiling.blogspot.com/2017/03/raspberry-pi-gy30.html
2. 吹泡泡機 https://learn.adafruit.com/make-it-bubble/sub-assemblies
## 工作分配
- 張真 -- 買素材、製作蜂鳴器樂譜
- 楊宜明 -- 製作光照度感測模組
- 劉嘉瑜 -- 寫文件、問題支援
