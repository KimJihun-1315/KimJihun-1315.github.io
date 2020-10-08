---
layout: post
title:  "[Arduino] 가스센서 다루기(MQ시리즈)"
date:   2020-09-27 20:00:00 +0900
categories: 
tags: 
---
오늘 설명할 센서는 제목에서 보이는거 처럼 가스센서를 다룰것이다. 

가스센서의 종류는 아래의 그림처럼 다양하고 필요에 따라 골라 쓸수있다.

![img](https://KimJihun-1315.github.io/assets/img/posting/20200927/img01.png)

<br/>
<br/>

각각의 센서 모두 4핀으로 이루어져 있고 각 핀마다 VCC, GND, D0, A0의 구성을 하고 있다. 

센서의 디지털 출력전압은 5V로 통일 되어있으며 

센서의 아날로그 출력전압은 1.5~5.0V로 통일 되어있지만 부탄가스센서 모듈만 4V를 사용합니다.

<br/>
<br/>

**가스센서 모듈의 동작원리**

----

![img](https://KimJihun-1315.github.io/assets/img/posting/20200927/img02.png)

가스센서를 작동시키면 내부에 있는 히터가 가열이 되어, 센서 내부의 금속막에 대기중의 성분이 달라붙게 됩니다. 이 성분이 달라 붙으면 저항값이 낮아지게 되고, 낮아진 저항값을 아두이노 시리어 모니터를 통해 수치적으로 볼 수 있는 것이다.

<br/>
<br/>

**사용예제**

----

아두이노에 아래의 표처럼 연결을 시킨다.

<br/>

 아두이노  센서 
   5V      VCC  
   GND     GND  
   A0      AOUT 

<br/><br/>

센서와 아두이노를 연결을 하고 아래의 코드를 작성하면 된다.

```c
int GasPin = A0;	//아날로그 핀의 값을 A0으로 선언
 
void setup() {
  pinMode(GasPin ,INPUT);
  Serial.begin(9600);
}
 
void loop() {
  Serial.println(analogRead(GasPin));   
  delay(1000);
} 
```

<br/><br/>

위의 코드를 설명하면

```c
int GasPin = A0;

pinMode(GasPin ,INPUT);
//핀모드를 이용하여 GasPin(A0)의 값을 INPUT으로 지정
```


```c
int GasPin = A0;

Serial.println(analogRead(GasPin)); 
//analogRead를 통해 읽은 GasPin의 값을 시리얼모니터에 출력을 한다.
```



이렇게 작성된 코드를 실행하고 시리얼 모니터를 열어보면 

![img](https://KimJihun-1315.github.io/assets/img/posting/20200927/img03.png)

위의 사진처럼 출력이 된다.
