###### tags: `makeNTU`
# 2019MakeNTU ^ ^ 
## 得獎啦～大家都好讚
## 簡介
**Project: smart lock 不要叫我鎖(by 建中夯棋王)**

成員:
高偉堯(NT) (建中夯棋王、建中冷梗王(汪 )
陳俊廷(GT) (我好爛、躺分仔)
吳宜庭(1T) (延平夯琴王、延平夯電神、延平最速傳說)
黃善莛(3T) (北一大大大大大學姐<(_ _)>、北一呆呆獸、啊我就強阿、北一機研社創社大神、大神快拜) 


## 描述
有時候可能因為下雨而一手撐傘，或是剛買完活大的麥當勞飲料而只有一手可以使用，此時要開腳踏車的鎖就不太方便。所以我們想做一個可以單手開啟的腳踏車鎖。
鎖的設計參考路上的共享自行車，開鎖方式可以用手機中的App遙控控制或是利用RFID感應學生證來開鎖。另外我們利用霍爾感測器判斷腳踏車是否有被騎乘，如果停留長達一定的時間則會自動上鎖，這樣就不用擔心腳踏車忘記上鎖而被偷走。
另外車鎖上也附有能用手機App遙控的LED燈和蜂鳴器，協助晚上找車。

Reference
[共享車車鎖原理](http://www.hbspcar.com/2105.html)
[自動上鎖](https://kknews.cc/zh-tw/digital/z5mxokl.html)
[I LOCK IT](https://www.ilockit.bike/en/about-i-lock-it)

## 用途
1. 利用刷卡or手機藍芽單手開鎖，下雨天再也不用淋濕~
2. 忘記鎖車也可以自動上鎖喔^^
3. 晚上電路學實驗大家都亂停找不到車? 有蜂鳴器和LED怎麼可能還找不到~~ (就是說阿) :+1: 

## 功能
1. RFID:
    * 利用手機藍芽設定一張悠遊卡為key card
    * 刷卡後開鎖、閉鎖
2. BlueTooth:
    * 開/關鎖 
    * 開/關LED燈 
    * 開/關蜂鳴器 
    * 設定key card
3. LED:
    * 當車燈

## 材料
1. Arduino UNO
2. 麵包板、杜邦線
3. 蜂鳴器(喇叭)、藍芽(HC-05)、RFID(mfrc522)、LED、霍爾模組(KY-024)、步進馬達 + 擴充模板(IC: ULN2003)
4. 3D-print frame
5. 密集板
6. 電池模組


## 分工
* NT、3T: Inventor 畫架構、3D列印、硬體整合
* GT: 軟體藍芽(蜂鳴器、LED)、手機app、整合code
* 1T: RFID

## 車鎖結構

## Arduino接線
* RFID:
    * SDA: 10
    * MOSI: 11(固定)
    * CLOCK: 13(固定)
    * MISO: 12(固定)
    * RST: A0
* Blue tooth
    * Rx: 6(藍芽的Rx)
    * Tx: 7(藍芽的Tx)
* Hall_pin: A1, GND
* buzzer_pin: 9, GND
* stepper:
    * pin1: 2
    * pin2: 3
    * pin3: 4
    * pin4: 5
* LED_pin: 8, 5V


## Code
* RFID
    * [reference 1](http://milktea132.blogspot.com/2018/02/arduino-18-rfid-rc522led.html)
    * [reference 2](https://swf.com.tw/?p=930)
* Blue tooth
    * reference: 電資工程入門
    * 手機端控制APP: 利用[APP Inventor](https://blog.cavedu.com/2014/04/04/%E9%9B%99a%E8%A8%88%E5%8A%83-part1%EF%BC%9Aapp-inventor-%E7%B6%93%E7%94%B1%E8%97%8D%E7%89%99%E6%8E%A7%E5%88%B6-arduino-led-%E4%BA%AE%E6%BB%85/)寫出簡單的控制程式
    * 回傳值在code有註解，利用char command儲存手機傳值，每次void loop就重設command為'd'(default)，避免重複執行指令
* buzzer
    * [reference](http://yhhuang1966.blogspot.com/2016/09/arduino_17.html)
    * include"note.h"是define音階用的，為了讓蜂鳴器唱出小蜜蜂^^
    * buzzer_check(): 在RFID成功讀取卡片資料時逼~一聲
    * buzzer_find(int): 唱出小蜜蜂，利用void loop唱出陣列中儲存的音符(變數i 紀錄唱到哪個音符)，因為如果用while讓蜂鳴器唱，此時無法讀取藍芽傳得值
* LED
    * reference: 常識(?
    * 利用void loop，及變數flash紀錄上一個迴圈的亮暗，製造閃爍效果，同buzzer_find()，若在void loop中使用while，就無法讀藍芽的回傳值，除非把BT.read()寫在每個while但這樣很麻煩
* Hall
    * [reference](http://bruce-iot.blogspot.com/2016/05/iot29-arduino.html)
    * 有磁場回傳1，沒有就0，就這麼簡單^^
    * millis(): 內建函式，回傳從arduino開機到現在的ms，最多記錄到49天後會[rollover](https://www.baldengineer.com/arduino-how-do-you-reset-millis.html)
    * 變數unsigned long lock_interval: 預計停下多久要自動上鎖 
    * 變數unsigned long previous_time: 紀錄上次時間停下的時間，霍爾模組有讀值就reset到millis()，當millis() - previous_time >= lock_interval 時就上鎖

* Stepper motor
    * Cheap step motor: 2相4線
    * ULN2003模組 + 擴充板
    

# 完成時間~  2019/3/31 11:23 ^^



