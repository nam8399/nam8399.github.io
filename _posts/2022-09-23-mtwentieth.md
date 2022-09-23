---
layout: single
title:  "모바일앱 개발일기 #20 안드로이드 Kotlin Object와 Companion Object 차이"
categories: Android
---

우리는 Kotlin에서 Java의 static과 같은 정적 변수 및 메서드를 사용하기 위해 보통 object나 Companion object를 사용한다. 

비슷한거 같으면서 다른 Object declaration와 Companion object의 차이에 대해서 알아보자.

<br/><br/>
### Object declaration이란?


---

Object declaration은 싱글톤 패턴을 더 쉽게 사용하기 위해 코틀린에서 제공하는 일종의 객체 선언 키워드라고 볼 수 있다.

예시 코드를 한번 봐보자.

```kotlin
object ObjectDecl {

    const val OB_STRING = "1"

    fun obtest() {}
}
```

Object는 위와 같은 형태로 선언할 수 있고 다음과 같은 특징이 있다.

- Singleton 형태 
- thread-safe
- lazy initialized : 실제로 사용될 때 초기화(initialized) 된다 
- const val로 선언된 상수는 static 변수.
- object 내부에 선언된 변수와 함수들은 java 의 static 이 아님. 단, 아래 케이스들은 static
    - const val 로 상수 선언한 것들.
    - @JVMStatic 또는 @JVMField의 어노테이션이 붙은 변수 및 함수들.

위와 같은 특징 중에서도 Object declaration의 장점은 thread-safe와 lazy initiallized라고 볼 수 있다.
- Thread-safe란?
  멀티 스레드 환경에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램 실행에 문제가 없는 것
- lazy initialization란?
  Object 키워드가 선언된 클래스는 외부에서 객체가 사용되는 시점에 초기화가 이루어진다.

참고로 Object 키워드가 선언된 클래스는 주/부 생성자를 사용할 수 없다. 객체 생성과 동시에 생성자 호출 없이 바로 만들어지기 때문이다. 

또한 중첩 object 선언이 가능하며, 클래스나 인터페이스를 상속할 수 있다.


<br/><br/>
### Companion Object란?


---

앞서 한번 따로 정리했었던 Companion Object이다. Companion object 설명 : https://nam8399.github.io/android/mnineteenth/

Companion object는 클래스 내부의 객체 선언을 위한 object 키워드다. 한 마디로 클래스 내부에서 싱글톤 패턴을 구현하기 위해 사용한다.

companion object의 예시코드도 한번 봐보자.

```kotlin
class CompanionObjectTest {

    companion object {
        const val CONST_TEST = 2

        fun test() { }
    }
}
```

companion object는 클래스 인스턴스 없이 어떤 클래스 내부에 접근하고 싶을 때 선언한다. 

클래스당 하나만 사용할 수 있고, Object declaration과 같이 생성자를 가질 수 없으며, static으로 선언되는 것이 아니라 런타임시 실제 객체의 인스턴스로 실행된다.

companion object에는 다음과 같은 특징이 있다.

- 해당 클래스(companion obejct 는 클래스 내부에 들어가는 블럭이므로) 자체가 static 이 아님. 
- 즉, CompanionObjectTest() 로 생성할 때 마다 객체의 주소값은 다름.
- 해당 클래스가 로드될 때 초기화 됨.
- const val로 선언된 상수는 static 변수.
- companion object 내부에 선언된 변수와 함수들은 java 의 static 이 아님. 단, 아래 케이스들은 static
  - const val 로 상수 선언한 것들.
  - @JVMStatic 또는 @JVMField의 어노테이션이 붙은 변수 및 함수들.

<br/><br/>


### Object declaration와 Companion Object 차이


---

두 선언 방식의 차이점은 앞서 개념만 봐도 알 수 있듯이 Object declaration은 클래스 전체가 하나의 싱글톤 객체로 선언되지만, companion object는 클래스 내에 일부분이 싱글톤 객체로 선언된다. 

또한 둘은 초기화 시점이 다르다. 

Object declaration이 선언된 클래스는 해당 클래스가 사용될 때 초기화가 되지만, companion object는 해당 클래스가 속한 클래스가 load될 때 초기화가 이루어진다.

두 선언 방식의 차이점에 대해 다시 정리해보자면

- 초기화 시점이 다름.
- object declaration 초기화 시점 : 실제로 사용될 때 initialized 된다. 실제로 내부 함수를 접근해야 init 블럭이 호출됨 
- companion object 초기화 시점 : 해당 클래스가 로드될 때 initialized 된다. 실제로 해당 클래스 (CompanionObjectTest)를 생성하면 companion object 내부 init 블럭이 호출됨을 확인.
 


<br/><br/>


### 마무리

---

오늘은 object declaration와 companion object의 차이에 대해서 알아봤는데 두 선언 방식의 차이점을 명확히 이해하면서 개발할 때 필요한 기능에 따라서 적합하게 사용해야겠다는 생각이 들었다.

공부하고 싶은 것은 많고 개발하고 싶은 것도 많지만 회사 생활과 병행하면서 하기 쉽지는 않은 것 같다.

그래도 시간 관리를 잘하면서 꾸준히 성장하는 개발자가 되도록 항상 노력할 것이다.

