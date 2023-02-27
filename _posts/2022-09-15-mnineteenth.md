---
layout: single
title:  "[Android] 안드로이드 Kotlin Companion Object란?"
categories: Android
---

자바에 static 변수(필드), 메서드가 존재하듯 코틀린에서도 정적 변수와 함수가 존재한다. 그러나 코틀린의 문법 특성 상 클래스 안에 이것들을 클래스 안에 둘 수는 없고, 코틀린에는 static이라는 키워드가 존재하지 않는다. 

그럼 어떻게 사용할까? 이 때를 위해 존재하는 것이 companion object라는 것이다.

오늘은 Kotlin의 Companion Object에 대해서 알아볼 것이다.

먼저 Companion Object에 대해 알아보기 전에 Java의 상수 코드를 먼저 봐보자.

```java
public class Animal {
  static final int MAX_AGE = 100;
}

public static void main() {
  System.out.prinln("최대 나이는 " + Animal.MAX_AGE);
}
```

Animal이라는 클래스 내부에 MAX_AGE라는 상수가 public 접근 제한자로 존재하고, 이는 외부에서 Animal.MAX_AGE로 접근할 수 있다.

클래스 안에서만 적용하게 할지 밖에서도 사용하게 할지는 접근 제한자를 public, private로 바꿔가면서 사용하면 된다.

그리고 클래스와 인스턴스 관점에서 바라보면 static 필드인 MAX_AGE는 Animal의 인스턴스가 생길때마다 메모리가 추가로 할당되는 것이 아니고 Animal 클래스 설계도 자체에 함께 존재하게 된다.

<br/><br/>
### Kotlin에서의 companion object


---

위의 코드를 똑같이 Kotlin으로 바꿔보자.

```kotlin
class Animal {
  companion object {
    const val MAX_AGE : Int = 100
  }
}

fun main() {
  println("최대 나이는 ${Animal.MAX_AGE}")
}
```

class Animal 내부에 companion object라는 구문이 존재하고 해당 객체 내에 const val이 붙은 변수가 존재하게 된다.

const가 붙은 이유는 MAX_AGE는 런 타임이 아니라 컴파일 타임에 100이란 값으로 초기화가 되기 때문이다.

또한 코틀린에서는 기본 접근 제한자 public이기에 class와 const val 앞에 public을 붙이지 않아도 된다.


<br/><br/>

##### Java에서의 static method

자바에는 static 필드 외에도 static method가 존재한다.

```java
public class Animal {
  public static void sayHi() {
    System.out.println("안녕")
  }
}

public static void main() {
  System.out.prinln(Animal.sayHi());
}
```

코틀린에서도 companion object를 이용해 비슷한 구현이 가능하다.

<br/><br/>

##### Kotlin에서의 companion object

```kotlin
class Animal {
  companion object {
    fun sayHi() {
      println("안녕")
    }
  }
}

fun main() {
  Animal.sayHi()
}
```

겉으로 보면 static과 companion object가 동일한 역할을 하는 것처럼 보이지만 흥미롭게도 코틀린의 companion object에는 다른 점이 두 가지 존재한다.


<br/><br/>

### companion object는 객체


---

companion object에서 기억해야 할 중요한 점은 객체라는 것이다. 그래서 다음과 같은 코딩이 가능해진다.

```kotlin
class Animal{
    companion object{
        val prop = "나는 Companion object의 속성이다."
        fun method() = "나는 Companion object의 메소드다."
    }
}
fun main(args: Array<String>) {
    println(Animal.Companion.prop)
    println(Animal.Companion.method())
    val comp1 = Animal.Companion  //--(1)
    println(comp1.prop)
    println(comp1.method())
    val comp2 = Animal  //--(2)
    println(comp2.prop)
    println(comp2.method())
}

```

##### companion object에 이름 붙이기

그리고 객체이기에 이름을 붙일 수가 있다.

```kotlin
class Animal {
  companion object Constant {
    const val MAX_AGE : Int = 100
  }
}

```

물론 이름을 붙였다고 해서 사용법이 달라지지는 않는다.

```kotlin
println(Animal.MAX_AGE)
println(Animal.Constant.MAX_AGE)
```
위와 같이 변경한 이름으로도 사용이 가능하다.


##### companion object에서 인터페이스 구현하기

companion object는 객체이기에 interface도 구현이 가능하다.


```kotlin
interface Movable {
  fun move()
}

class Animal {
  companion object : Movable {
    override fun move() {
      println("Animal Move")
    }
  }
}

fun main() {
  Animal.move()
}
```

여기서 주의해야 할 점은 companion object는 클래스에 붙어있는 객체이기에 프로세스에서 한 인스턴스만 존재하게 된다.


```kotlin
class Animal {
  companion object : Movable {
    var age : Int = 10
  }
}

fun main() {
  println(Animal.age) // 10
  Animal.age++
  println(Animal.age) // 11
}
```

그리고 interface 내에서도 companion object를 정의할 수 있다.

```kotlin
interface MyInterface{
    companion object{
        val prop = "나는 인터페이스 내의 Companion object의 속성이다."
        fun method() = "나는 인터페이스 내의 Companion object의 메소드다."
    }
}

fun main(args: Array<String>) {
    println(MyInterface.prop)
    println(MyInterface.method())
    val comp1 = MyInterface.Companion
    println(comp1.prop)
    println(comp1.method())
    val comp2 = MyInterface
    println(comp2.prop)
    println(comp2.method())
}
```

덕분에 인터페이스 수준에서 상수항을 정의할 수 있고 관련된 중요 로직을 이곳에 기술할 수 있다.

이 특징을 잘 활용하면 설계하는데 도움이 될 것이다.

<br/><br/>


### 마무리

---

오늘은 companion object에 대해서 알아봤는데 사실 companion object에 대한 특징은 더 다양하고 많지만 간단하게만 알아봤다.

개발할 때 companion object를 잘 활용해서 코드를 구현해야겠다는 생각이 들었고 앞으로 더욱 더 개발과 관련된 부족한 부분들을 공부하고 잘 정리하면서 성장할 것이다.



