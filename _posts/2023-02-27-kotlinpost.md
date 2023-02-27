---
layout: single
title:  "[Kotlin] 코틀린 기본 문법 총정리"
categories: Kotlin
---

오늘은 안드로이드 개발에 정말 중요한 Kotlin 기본 문법을 정리해보려한다.

코틀린 관련 글을 이전에도 몇개 올렸었는데 기본 문법 정리글을 뒤늦게 작성해본다.

### 코틀린(Kotlin)이란?


---

코틀린은 간결한 문법과 높은 안정성으로 높은 생산성을 보장하여 개발된 프로그래밍 언어로 100% 자바와의 호환이 가능하다.

또 자바로 작성된 프로젝트에 코틀린 코드를 추가할 수 있다는 장점도 가지고 있다.

그리고 자바와의 호환이 가능이하기에 자바와 코틀린간의 변환도 가능하다.


<br/><br/>
### 간결한 문법 (세미콜론 불필요)


---

자바와 C 처럼 문장끝은 의미하는 세미콜론 ; 이 사라졌다.

```kotlin
fun main() {
    println("Hello, world!!!")
}
```


<br/><br/>
### 변수 선언 시 파스칼, 카멜 표기법 권장


---

파스칼 표기법 : ClassName

카멜 표기법 : className

<br/><br/>
### 변수 선언 방법


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

##### 널 안정성

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
### 자료형


---

변수에서 사용할 수 있도록 코틀린이 제공하는 기본 자료형은 자바와의 호환을 위해 **자바와 거의 동일**하다.


##### 정수, 실수형

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

##### 문자, 문자열

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


##### Bolean

```kotlin
fun main() {
    var booleanValue:Boolean = true
}
```

<br/><br/>
### 형 변환


---

