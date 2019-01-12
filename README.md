# BubbleMusicBox
## 介紹

這是一個簡單的吹泡泡音樂機器，操作方法如下：
1. 先將泡泡水(濃度需調為較濃)放入置前方盒子中。
2. 將主程式打開，並手動開啟右邊風扇按鈕。
3. 在盒子下方有個按鈕打開。
- 完成以上三個步驟就會有邊吹泡泡邊唱歌了 ~ 是不是很簡單呢?恩真的很簡單 ~ 

## 成品
-內部線路
![image](https://ppt.cc/fMxmSx@.jpg)

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

## 結構原理
1. Arduino板 : 這個板子上面的功用是當有電的時候開啟左邊小小開關，而後面的燈泡感應到有電就會亮起來，蜂鳴器就會開始唱歌。

2. Raspberry Pi 3 : 板子上接上一個感光元件，並於元件旁裝置LED燈，當有光且亮度為30以上的時候，就會啟動馬達(吹泡泡的棍子)進行上下晃動。

- 會用Arduino主要是說麵包版那邊好像沒辦法一次供應感光元件、Servo(接吹泡泡的棍子)以及風扇馬達，因為在光感應還是馬達(我忘記哪邊QQ)通電的時候為瞬間吃掉大量的電，導致Servo那裡在執行的時候會有抽蓄的情形，所以才使用這個板子去接Raspberry板子。

## 線路圖
- Arduino  
![image](https://ppt.cc/fI4Tfx@.png)
- Raspberry Pi
![image](https://ppt.cc/fIgVZx@.png)

- 左邊的風扇馬達~不小心畫錯~ 應該是與旁邊的AA電池接在一起，由手動的方式啟動(拍謝~)

## 程式說明

- Arduino : 小蜜蜂 
```
const int speaker=2;	
unsigned int frequency[7]={523,587,659,694,784,880,988};
char toneName[]="CDEFGAB";	
 
char beeTone[]="GEEFDDCDEFGGGGEEFDDCEGGEDDDDDEFEEEEEFGGEEFDDCEGGC"; 
byte beeBeat[]={1,1,2,1,1,2,1,1,1,1,1,1,2,
1,1,2,1,1,2,1,1,1,1,4,
1,1,1,1,1,1,2,1,1,1,1,1,1,2,
 
1,1,2,1,1,2,1,1,1,1,4};
const int beeLen=sizeof(beeTone);
unsigned long tempo=180;	
 
int i,j; void setup()
{}
void loop()
 
{
for(i=0;i<beeLen;i++)	
playTone(beeTone[i],beeBeat[i]);
delay(3000);	
}
void playTone(char toneNo,byte beatNo)	
{
unsigned long duration=beatNo*60000/tempo; 
for(j=0;j<7;j++)
{
if(toneNo==toneName[j])	
{
tone(speaker,frequency[j]);	
delay(duration);	
noTone(speaker);	
}
}
}
```
- Raspberry pi :
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
- 光照度感測 : http://atceiling.blogspot.com/2017/03/raspberry-pi-gy30.html
- 吹泡泡機 : https://learn.adafruit.com/make-it-bubble/sub-assemblies
- Servo馬達 : 
1. https://blog.everlearn.tw/%E7%95%B6-python-%E9%81%87%E4%B8%8A-raspberry-pi/raspberry-pi-3-mobel-3-%E5%88%A9%E7%94%A8-pwm-%E6%8E%A7%E5%88%B6%E4%BC%BA%E6%9C%8D%E9%A6%AC%E9%81%94
2. https://www.electronicshub.org/raspberry-pi-servo-motor-interface-tutorial/#Code
3. https://www.youtube.com/watch?v=Sg8VaKiZFVE

- Python錯誤相關 :
1. SyntaxError: invalid syntax https://blog.csdn.net/zcf1784266476/article/details/71335939
2. IndentationError: unexpected indent https://blog.csdn.net/lhshu2008/article/details/25793785
3. IndentationError:expected an indented block https://blog.csdn.net/NeilHappy/article/details/7724959
4. IOError: [Errno 121] Remote I/O error https://www.raspberrypi.org/forums/viewtopic.php?t=191300

- Python 基本知識 :
1. http://smandyscom.blogspot.com/2016/01/raspberry-pi-pwm.html
2. https://github.com/tomlinNTUB/Python/blob/master/01-1%20%E5%85%A7%E5%BB%BA%E5%9E%8B%E6%85%8B(int%2C%20float%2C%20str%2C%20bool).md


- 對一個程式不強且從來沒碰過 python 跟 Raspberry 的小菜鳥來說 python基本上就是瘋狂的去debug 跟上網查看相關基本架構 QAQ
## 工作分配

| 編號 | 工作項目    | 工作者  |
|----|:-----------:|:-------:|
|1.|採購元件|張真|
|2.|系統功能||
|(2.1)|蜂鳴器|張真|
|(2.2)|馬達|楊宜明|
|(2.3)|感光元件|楊宜明|
|3.|音樂盒製作|張真、楊宜明|
|4.|文件撰寫|劉家瑜、楊宜明|
|5.|報告Demo|劉家瑜、楊宜明|
