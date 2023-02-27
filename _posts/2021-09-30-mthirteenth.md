---
layout: single
title:  "[Java] 자바 문자열 비교하기"
categories: Java
---

오늘 날씨 API를 받아와서 날씨 상황마다 사진이 다르게 표시되는 작업을 하고있었는데 문자열을 비교해야 할 일이 생겼다.

보통 자바에서 양쪽에 있는 데이터를 비교할 때 ==를 사용했다. 

하지만 이와 비슷한 메소드인 equals()와 차이점이 뭘까?

한번 알아보도록 하자.

 <br/><br/>

위 두 연산자는 두개의 데이터를 비교한 결과값을 Boolean Type으로 변환한다는 공통점을 가지고 있다. 하지만 그 기능에서 차이가 난다.

 <br/><br/>

##### 형태

equals()는 메소드이며 객체끼리 내용을 비교할 수 있는 기능을 제공한다.

그와 반대로 ==는 비교를 위한 연산자이다.

 <br/><br/>

##### 주소값 비교와 내용 비교

equals()는 비교하고자 하는 대상의 내용 자체를 비교하지만 ==연산자는 비교하고자 하는 대상의 주소값을 비교한다.

예시를 통해 보도록 하겠다.

```dart
String a = "aaa";
String b = a;
String c = new String("aaa")
```

String 형태로 a,b,c를 각각 만들어주고 결과적으로 세 문자열은 모두 "aaa"라는 값을 가지고 있다.

하지만 a와 b는 같은 주소를 사용하는 방면 c는 새로운 String 형태로 정의해줬으므로 새로운 주소를 가지게 된다.

그렇다면 위 a,b,c를 =와 equals()를 사용해서 비교하면 어떻게 될까?

 <br/><br/>

##### 1) a.equals(b)

a와 b의 내용을 비교했으므로 true

##### 2) a==b

a와 b가 가지고 있는 주소값을 비교했으므로 true

##### 3) a==c

a와 c가 가지고 있는 주소값을 비교했으므로 false

##### a.equals(c)

a와 c의 내용을 비교했으므로 true

 <br/><br/>

그렇다면 내가 현재 개발중인 어플에서 JsonObject로 날씨의 상태를 받아왔을 때 받아온 날씨 데이터마다 배경사진을 다르게 하려면

```java
if (weather.equals("clear sky")) {
        bgimg.setBackgroundResource(R.drawable.wdn01);
    }
}else if (weather.equals("few clouds")) {
        bgimg.setBackgroundResource(R.drawable.wether);
}
```

와 같은 형식으로 구현해주면 된다.
