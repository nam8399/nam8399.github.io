---
layout: single
title:  "[Kotlin] 코틀린 Char 타입을 Int로 변환하려면?"
categories: Kotlin
published: true
---

오늘은 백준 코딩테스트 문제를 풀다가 Char형을 Int형으로 형변환 해야 할 일이 있었는데 새로운 사실을 알게되었다.

이 부분에 대해서 간단하게 글을 써보고자 한다.

<br/><br/>

# Char 타입의 Char.toInt() 함수는?

---

코딩테스트에서 내가 구현하고자 했던 부분은 숫자를 한글자씩 Char형으로 받아서 원하는 동작을 처리한 뒤 마지막에 Int형으로 변환을 하는 작업이었다.

당연히 Char 타입의 toInt() 함수를 사용했는데 결과는 어떻게 됐을까?


![image](https://github.com/nam8399/NewProduct/assets/69960282/107b09d6-0548-4fab-ae33-89d27b0f4278)


~~이럴수가..~~

**결과는 실패였다.**

처음에는 당연히 toInt()에서 문제가 없을 것이라고 판단하고 다른 부분을 검사해봤지만 결국 찾을 수 없었고

컴파일러를 통해 로그를 찍어본 뒤에야 알 수 있었다.

```kotlin
"1".toInt()
```

위 코드를 실행하게 되면 **Int형식의 1이 아닌 49**가 반환된다.

이유는 무엇일까?

바로 Char형의 toInt() 함수는 **아스키(ASCII)** 값을 반환하기 때문이다.

그렇다면 우리가 원하는 Int값으로 변환하려면 어떻게 해야할까?


<br/><br/>

## Character.getNumericValue

바로 **Character.getNumericValue** 함수를 사용하면 된다.


```kotlin
Character.getNumericValue("1") // 1
```

위 코드처럼 Character.getNumericValue(Char) 함수를 사용하게 되면 우리가 원하는 결과값을 얻을 수 있다.

참고로 위 메서드는 자바에서도 동일하게 사용하면 된다.

