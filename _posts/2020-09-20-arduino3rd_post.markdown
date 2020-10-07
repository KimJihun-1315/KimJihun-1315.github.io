---
layout: post
title:  "[Arduino] LCD_I2C에 대해서"
date:   2020-09-20 20:00:00 +0900
categories: hw
tags: arduino
---

먼저 LCD에 대해 알아보면 Liquid Crystal Display의 줄임말로 우리가 흔히 말하는 액정이다.

이번 포스트에서는 16*2배열의 캐릭터 LCD를 다루어 볼것이다.

(이외에도 여러배열의 캐릭터 LCD와 그래픽 LCD가 있다.)

------

먼저 LCD를 사용하기위해서는 LCD의 16개핀을 다루어야 하는데 이러한 불편함을 덜어주기윈한 I2C가 있는것이다.

이러한 I2C를 사용하기위해서는 I2C의 주소값을 알아야한다.

 

아래코드는 I2C의 주소값을 알아내는 코드이다. 

```
#include <Wire.h>

void setup()
{
  Wire.begin();

  Serial.begin(9600);
  while (!Serial);           
  Serial.println("\nI2C Scanner");
}

void loop()
{
  byte error, address;
  int nDevices;
  Serial.println("Scanning...");
  nDevices = 0;
  for(address = 1; address < 127; address++ ) 
  {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();

    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16) 
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");

      nDevices++;
    }
    else if (error==4) 
    {
      Serial.print("Unknow error at address 0x");
      if (address<16) 
        Serial.print("0");
      Serial.println(address,HEX);
    }    
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");

  delay(5000);
}
```

참고로 대부분 0x27 또는 0x3F이다.(나는 귀찮아서 27로 실행후 되지 않는다면 3F로 바꾸어 실행하는 편이다.

------

주솟값을 알아냈으면 LCD에 출력을 해 보아야한다.

1. \#include를 이용하여 <Wire.h>와 <LiquidCrystal_I2C.h>를 추가해준다
2. LiquidCrystal_I2C lcd(0x27, 16, 2);를 적어 준다.
3. void setup(){내용} 안의 내용을 입력한다
   1. lcd.init();을 이용하여 lcd통신을 초기화설정한다.
   2. lcd.backlight();를 이용하여 백라이트를 켠다.
4. void loop(){내용} 안의 내용을 입력한다.
   1. lcd.setCursor(0,0);을 이용하여 입력 시작위치를 잡는다.
   2. lcd.print("hello");를 이용하여 ""안의 값을 바꾸어 출력한다.

위의 방법으로 첫째줄 "Hello" 둘째줄 "World"를 출력하는 코드를 짜면

```
 #include <Wire.h>
 #include <LiquidCrystal_I2C.h>
 
 LiquidCrystal_I2C control_lcd(0x27, 16, 2);	//LCD I2C통신설정(주소값, 글자수, 줄수)
 
void setup(){
 	control_lcd.init();	//lcd통신 초기화
    control_lcd.backlight();	//백라이트 설정
}

void loop(){
	control_lcd.setCursor(0, 0);	//0,0에 해당하는 칸에 커서이동
    control_lcd.print("Hello");		//"Hello"출력
    control_lcd.setCursor(0, 1);	//0,1에 해당하는 칸에 커서이동
    control_lcd.print("World");		//"World"출력
}
```

라는 코드가 나오게 된다.

 

 

회로 연결은 아래처럼 하면된다.

| SCL  | A5   |
| ---- | ---- |
| SDA  | A4   |
| VCC  | 5V   |
| GND  | GND  |

 
