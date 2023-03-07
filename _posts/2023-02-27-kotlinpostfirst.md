---
layout: single
title:  "[Kotlin] 코틀린 기본 문법 총정리"
categories: Kotlin
---

오늘은 안드로이드 개발에 정말 중요한 Kotlin 기본 문법을 정리해보려한다.

코틀린 관련 글을 이전에도 몇개 올렸었는데 기본 문법 정리글을 뒤늦게 작성해본다.

# 코틀린(Kotlin)이란?


---

코틀린은 간결한 문법과 높은 안정성으로 높은 생산성을 보장하여 개발된 프로그래밍 언어로 100% 자바와의 호환이 가능하다.

또 자바로 작성된 프로젝트에 코틀린 코드를 추가할 수 있다는 장점도 가지고 있다.

그리고 자바와의 호환이 가능이하기에 자바와 코틀린간의 변환도 가능하다.


<br/><br/>
# 간결한 문법 (세미콜론 불필요)


---

자바와 C 처럼 문장끝은 의미하는 세미콜론 ; 이 사라졌다.

```kotlin
fun main() {
    println("Hello, world!!!")
}
```


<br/><br/>
# 변수 선언 시 파스칼, 카멜 표기법 권장


---

파스칼 표기법 : ClassName

카멜 표기법 : className

<br/><br/>
# 변수 선언 방법


---

var : 일반적으로 통용되는 변수로 언제든지 읽기 및 쓰기가 가능하다

```kotlin
  var x = 5
  x += 1
```

val : 선언시에만 초기화가 가능하고 중간에 값을 변경할 수 없다. (Java의 final과 유사함)

```kotlin
 val a: Int = 1
  val b = 2 // int type 추론
  val c: Int // 컴파일 오류, 초기화 필요(값 할당 안함)
  c = 3 // 컴파일 오류, 읽기 전용이라 추후에 할당 불가
```

변수는 선언 위치에 따라 두가지 이름으로 부른다.

클래스에 선언된 변수 : Property(속성)

이 외의 Scope 내에 선언된 변수 : Local Variable (로컬변수)

고전적인 언어들의 경우 변수가 선언된 후 초기화되지 않는다면 기본 값으로 초기화되거나 값이 할당하지 않았다는 표시로 null 값을 가지게 된다.

**코틀린은 기본 변수에서 null을 허용하지 않고** 변수에 값을 할당하지 않은채로 사용하게 되면 문법 에러를 표시하고 컴파일을 막아주므로 의도치 않은 동작이나 null pointer exception 등을 원천적으로 차단해준다는 장점이 있다.

참고로 변수에 값을 할당하는 것은 __반드시 선언시에 할 필요는 없으며__ 변수를 참조하여 사용하기 전까지만 하면 된다.

### 널 안정성

그런데 프로그램에 따라서는 변수에 값이 할당되지 않았다는 것을 하나의 정보로 사용하는 경우도 있을 수 있다.

```kotlin
fun main() {
    var a:Int? = null
}
```

그런 경우에는 위처럼 null을 허용하는 nullable 변수로 선언해줄수 있다.

자바에서는 객체 타입의 변수에서 null 값 허용 여부에 대한 구분을 할 수 없었지만 코틀린에서는 ?, !! 를 이용하여 null 값을 구분할 수 있다.

null 값 구분 이유 : 널값의 허용 여부를 컴파일 단계에서 검사하기 때문에 런타임 에러를 줄일수있다.


<br/><br/>
# 자료형


---

변수에서 사용할 수 있도록 코틀린이 제공하는 기본 자료형은 자바와의 호환을 위해 **자바와 거의 동일**하다.


### 정수, 실수형

```kotlin
fun main() {
    var intValue:Int = 1234
    var longValue:Long = 1234L //L을 붙여 더 큰 메모리를 사용하는 정수임을 표시
    var intValueByHex:Int = 0x1af //16진수
    var intValueByBin:Int = 0b //2진수 (binary 약자)
    //코틀린은 8진수 표기는 지원하지 않는다.
    
    var doubleValue:Double = 123.5 //실수는 소수점을 포함해 숫자를 쓰거나
    var doubleValueWithExp:Double = 123.5e10 //필요시 지수 표기법을 추가한다.
    var floatValue:Float = 123.5f //Float는 f를 붙인다.
}
```

코틀린은 내부적으로 문자열을 UTF-16 BE 방식을 사용한다. 따라서 글자 하나하나가 2byte의 메모리 공간을 사용한다.

### 문자, 문자열

```kotlin
fun main() {
    var charValue:Char = 'a'
    var koreanCharValue:Char = '가'
    var stringValue = "one line string test"
    var multiLineStringValue = """multiline
    string
    test"""
}
```


### Boolean

```kotlin
fun main() {
    var booleanValue:Boolean = true
}
```

<br/><br/>
# 형 변환

---

코틀린은 형변환시 발생할 수 있는 오류를 막기위해 다른 언어들이 지원하는 '암시적 형변환'은 지원하지 않는다.

명시적 형변환 : 변환될 자료형을 개발자가 직접 지정함.

암시적 형변환 : 변수를 할당할 시 자료형을 지정하지 않아도 자동으로 형변환 된다.

**형변환 함수**

- toByte()

- toShort()

- toInt()

- toLong()

- toFloat()

- toDouble()

- toChar()


자바에서의 암시적 형변환

```java
 int a = 1;
 double b = 3.5;
        
 double c = a + b;        // 정상
 int d = a + b;            // 오류 : int + double 은 double 타입이며, 이 double 타입을 int 에 저장할 수 없습니다.
```

위처럼 암시적 형변환을 허용하는 자바에서는 int형 변수와 double형 변수의 연산을 할 때 int형 변수를 자동으로 double 타입으로 바꿔준다.

하지만 코틀린에서는 앞서 말했듯 암시적 형변환을 지원하지 않기에 명시적 형변환을 통해 변환해주어야한다.

```kotlin
fun main() {
    var a: Int = 54321
    var b: Long = a.toLong() //int를 long형으로 변환
}
```

<br/><br/>
# 배열


---

```kotlin
fun main() {
    var intArr = arrayOf(1,2,3,4,5) //int 배열
    var nullArr = arrayOfNulls<Int>(5) //비어있는 5크기의 배열
    
    int Arr[2] = 8 //index2에 8값 할당
}
```

배열은 처음 선언했을 때의 전체크기를 변경할 수 없다는 단점이 있지만 한번 선언하면 다른 자료구조보다 빠른 입출력이 가능한 장점이 있다.

<br/><br/>
# 타입 추론


---

변수나 함수들을 선언할 때나 연산이 이루어 질 때 자료형을 코드에 명시하지 않아도 코틀린이 자동으로 자료형을 추론해주는 타입 추론 기능이 있다.

```kotlin
fun main() {
    var a = 1234 //int
    var b = 1234L //long
    
    var c = 12.45 //Double
    var d = 12.45f //float
    
    var e = 0xABCD //16진수
    var f = 0b0101010 //2진수
    
    var g = true //boolean
    var h = 'C' //Char
}
```

<br/><br/>
# 함수


---

함수는 fun 키워드로 정의한다.

```kotlin
fun sum(a: Int, b: Int): Int { return a + b }
```

이때 함수 몸체가 식인 경우 return도 생략이 가능하다. (return type 추론)

```kotlin
fun sum(a: Int, b: Int) = a + b
```

리턴할 값이 없는 경우 Unit(Object)로 리턴한다. (자바의 void 역할)

```kotlin
fun printKotlin(): Unit { println(“Hello Kotlin”) }
```

코틀린에서 함수는 내부적으로 기능을 가진 형태이지만, 외부에서 볼 때는 파라미터를 넣는다는 점 외에는 자료형이 결정된 변수라는 개념으로 접근하는 것이 좋다.

<br/><br/>
# 조건문


---

if문은 자바와 동일하다.

```kotlin
fun main() {
	var a = 7
	if(a > 10){
        println("a는 10보다 크다")
    } else{
        println("a는 10보다 작거나 같다")
    }
}
```

다른 언어에서의 switch문을 when으로 사용한다.

Any자료형 : 어떤 자료형이든 상관없이 호환되는 코틀린의 최상위 자료형

```kotlin
fun main() {
	doWhen(12L)
}
 
fun doWhen (a :Any){
    when(a) {
        1 -> println("정수 1이다")
        "DiMo" -> println("디모의 코틀린 강좌")
        is Long -> println("Long 타입이다")
        !is String -> println("String 타입이 아니다")
        else -> println("어떤 조건도 만족하지 않는다")
    }
}
```

실행결과 :

```
Long 타입이다.
```

when의 조건이 맞을때 값을 반환하는 표현식으로서의 역할을 하게 하려면 동작대신 값을 써주면 된다.

```kotlin
fun main() {
	doWhen(12L)
}
 
fun doWhen (a :Any){
    var result = when(a) {
        1 -> "정수 1"
        "DiMo" -> "디모의 코틀린"
        is Long -> "Long 타입"
        !is String -> "String 타입 아니다"
        else -> "어떤 조건도 만족하지 않는다"
    }
    
    println(result)
}
```


<br/><br/>
# 반복문


---

조건형 반복문 : 조건이 참인 경우 반복을 유지 (while, do...while)

범위형 반복문 : 반복 범위를 정해 반복을 수행 (for)

### while문

```kotlin
	
fun main() {
	var a = 0
    
    while(a < 5){
        println(a++)
    }
}
```

실행결과

```kotlin
0
1
2
3
4
```


### do...while문

while에 의해 조건을 체크하여 반복한다는 점은 같지만 최초 한번은 조건없이 do 에서 구문을 실행한 후 while로 조건을 체크하는 선 후 관계에 차이가 있다.

```kotlin
	
fun main() {
	var a = 0
    
    do
    {
        println(a++)
    }while (a < 5)
}
```

### for문

기본적으로 for문은 값을 1씩 증가시키며 반복한다.

```kotlin
fun main() {
	for(i in 0..9){ //0~9까지 반복
        print(i)
    }
}
```
실행결과 : 

```kotlin
0123456789
```

증가값은 step을 이용하여 변경이 가능하다.

```kotlin
fun main() {
	for(i in 0..9 step 3){ //0~9까지 3 단위로 반복
        print(i)
    }
}
```

실행결과 : 

```kotlin
0369
```

for문 감소방법

```kotlin
fun main() {
	for(i in 9 downTo 0){ //1씩 감소하며 반복 
        print(i)
    }
}
```

실행결과 : 

```kotlin
987654321
```

<br/><br/>
# 흐름제어


---

```kotlin
fun main() {
    for(i in 1..10){
        if(i==3) break
        print(i)
    }
}
```

break로 반복문을 종료시킨다.

실행결과 : 

```kotlin
12
```

```kotlin
fun main() {
    for(i in 1..10){
        if(i==3) continue
        print(i)
    }
}
```

위 코드에서 continue를 사용하면 3인 경우 다음 숫자로 넘어가서 for문을 계속 실행하게 된다.

실행결과 : 

```kotlin
1245678910
```

여기까지는 자바와 비슷하지만 코틀린은 하나의 기능이 더 추가되었다.

다중 반복문에서 break나 continue가 적용되는 반복문을 label을 통해 지정할 수 있는 기능이다.

```kotlin
fun main() {
    for(i in 1..10){
        for(j in 1..10){
            if(i == 1 && j == 2) break
        }
        //또 체크?
    }
}
```

i가 1이고 j가 2일 경우 모든 반복문을 종료해야 한다고 가정할 때 위와 같이 break를 사용하게 되면 안에 있는 반복문은 종료되지만 바깥 반복문에서 또 다시 조건을 체크하며 반복될 것이다.

코틀린에서는 외부 반복문에 레이블 이름과 @ 기호를 달고, break문에서 @와 레이블 이름을 달아주면 레이블이 달린 반복문을 기준으로 즉시 break를 시켜준다.

```kotlin
fun main() {
    loop@for(i in 1..10){
        for(j in 1..10){
            if(i == 1 && j == 2) break@loop
        }
    }
}
```

위는 loop라는 이름의 label을 만들어서 다중 반복문을 break로 빠져나온 예시이다.

이는 break뿐만 아닌 continue의 경우에도 마찬가지이다.


```kotlin
fun main() {
    loop@for(i in 1..10){
        for(j in 1..10){
            if(i == 1 && j == 2) break@loop
            println("i : $i, j : $j")
        }
    }
}
```
위 코드처럼 변수명앞에 $를 적어서 변수 값을 출력할 수도 있다.

출력결과 : 

```kotlin
i : 1, j : 1
```


# 마무리


---

오늘은 코틀린의 기본 문법에 대해서 알아보았다.

아직은 자바에 더 익숙해져있기에 코틀린 기본 문법 하나하나도 완벽하게 익혀야 할 필요가 있다.

현재 코틀린을 사용해서 사이드 프로젝트를 진행중인데 빠르게 습득해서 유연하게 개발할 수 있도록 해보겠다.
