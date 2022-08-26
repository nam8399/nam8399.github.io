---
layout: single
title:  "모바일앱 개발일기 #18 Compose란? Compose의 필요성"
categories: Android
---

오늘은 Compose에 대해서 한번 알아볼 것이다. 

요즘 정말 안드로이드 개발자들 사이에서 뜨거운 Compose가 무엇인지 왜 필요한지에 대해서 정리해보고자 글을 써봤다.

일단 Compose가 필요한 이유는 크게 3가지이다.

1. xml을 벗어난 UI 개발
 
2. 선언형 UI

3. 상속이 아닌 확장

<br/><br/>
### XML을 벗어난 UI 개발

---


먼저 기존 안드로이드 UI의 특징을 살펴보자.

기존까지 안드로이드 UI를 다룰때는 Xml에다가 UI 속성을 정의를 하는경우가 있고, Class에서 프로그래밍으로 정의를 하는 경우가 있었다.

안드로이드는 기본적으로 Xml를 통해서 UI를 만든 다음에 class(Activity or Fragment or Dialog 등등)에서 UI를 컨트롤 하고 xml의 UI들을 클래스에서 사용할 셋팅이 필요하다. (findViewById, ViewBinding,DataBinidng)

그리고 셋팅 후 액티비티에서 UI를 어떻게 컨트롤 할지 정의를 해야한다.

이 과정에서 불필요한 반복 코드가 매우 많이 생성되고 코드 작성시 내가 의도하지 않는 UI를 만들거나 실수로 UI를 잘못 정의하는 경우가 있다.

그래서 xml로 작업하고 액티비티에서 컨트롤 하는 작업은 UI 생산속도의 저하를 일으킨다.

그리고 가장 중요한 다른 개발자들과의 협업 과정에서 다른 작업자가 내 코드를 보거나 오랜시간이 지나서 다시 유지보수를 하는일이 생기면

그 작업자는 xml과 class를 오가며 살펴봐야 하는경우부터 생기기 때문에 불편해진다.

(xml의 id를 살펴보고 class 에서 사용한 변수명을 체크하는 경우 등등..)
(findViewById, ViewBinding,DataBinidng)

무엇보다 가장 큰 이유는 기존 명령형 UI 제작 방식은 개발자가 직접 set,get를 사용해야해서 내부 동작까지 모든걸 만들어야하는 과정이 있고 사람이 직접 로직을 만들기 때문에 그로인해 작은 실수들이 발생하여 버그 및 불필요한 작업 시간이 매우 소요 된다.

위와 같은 불편함을 해결 해줄 새로운 UI 제작 방식이 바로 Compose인 것이다. 

<br/><br/>

##### 안드로이드 UI의 기존 명령형 방식

그렇다면 앞서 말한 기존 명령형 UI 제작 방식이라는게 도대체 뭘까?

기존에는 view와 데이터 사이에 중간 계층이 하나 끼어들어야만 하는 형태였다. 

**(Data -> Extra Layer(ex : findViewById) -> XML Layout)**

UI를 그리기 위해 XML을 사용하고 있음에도 불구하고, 코틀린 역시 UI에 어떤 id 값이 있고, 어떤 UI를 포함하려 하는지를 알고 있어야 하기 때문에 둘은 강하게 의존하게 된다.

시간이 많이 흐른 뒤 view를 수정하고 싶어서 xml을 수정하게 되면, 어쩔 수 없이 kotlin 코드도 수정해야 하며, 이때 하나라도 놓치면 런타임 에러가 나게 된다.

결국 코틀린 개발자와 코틀린 코드가 어떠한 안전장치도 없이 UI에 개념적으로 의존해야 한다는 의미이기도 한다.

MVVM과 databinding 조합은 그나마 상황이 낫긴 하다. 

bindingAdapter를 사용하면 programming language에서 코드적으로나 개념적으로 UI의 구조에 덜 의존하게 될 수 있게 되고, 어느 정도는 동적인 UI 제어가 가능해지긴 했다.

그럼에도 불구하고 여전히 XML을 벗어나지 못했다는 한계가 있는데, 특정 view의 attribute 정도를 동적으로 적용하기 쉬울 뿐, UI 그룹들을 동적으로 바꾸기에는 적합하지 않다.

Fragment를 써서 Kotlin 코드로 Fragment를 교체하는 게 그나마 나은 방법이 되겠지만, Fragment 역시 쉽지 않다.

code로 Fragment를 조작하는 일은 매우 귀찮은 작업일 뿐만 아니라 생명 주기도 까다로워서 예상치 못한 상황에 애를 먹기도 한다.

하지만 Jetpack Compose를 사용하면 Fragment는 자연스럽게 없어진다. 아래에서 다시 다루도록 하겠다.

<br/><br/>

##### XML을 버린 Jetpack Compose 방식

기존 명령형 방식과 다르게 Jetpack Compose 방식을 설명하면 오른쪽과 같다. 

**(Data -> Jetpack Compose)**

위 설명처럼 선언형 UI toolkit인 Jetpack Compose(only kotlin)로 구현할 경우 나타나는 패턴이다. 

즉, 데이터를 입력받으면 UI를 그려주는 Kotlin 함수(composable 함수)를 사용하여 UI를 그리게 되는 상황인데, ViewModel에서 변경된 데이터를 Composable 함수의 파라미터로 바로 넘겨주기만 하면 되기 때문에, 서로 다른 분야를 이어주며 강한 결합도를 가지는 Extra Layer가 사라지게 된다.

즉 UI가 바뀌면, Composable 함수만 수정하면 될 뿐이다.

<br/><br/>

이렇게 된다면 Fragment는 자연스럽게 사라지게 된다.

액티비티 한 개만 사용하는 상황에서, 두 화면을 각자 독립적인 클래스로 다룰 때도 결국은 각각 Activity의 Lifecycle이 필요하다.

즉 Fragment는 Android 컴포넌트가 아니라 단지 Activity의 생명주기를 추종하는 View라고 볼 수 있다.

따라서 하나의 Activity에 성격이 다른 화면(View)이 두 개 필요할 때, Fragment를 사용하면 findViewById와 기타 로직이 Activity 하나에 너무 많아지는 걸 방지할 수 있게 되었다. 

그러나 결국 Lifecycle을 추종하는 View 일 뿐이기 때문에 UI Toolkit이 Jetpack Compose로 바뀌면 View를 위한 Fragment의 사용성은 사라진다.

findViewById는 역사속으로 사라지고, 기타 로직은 ViewModel에서 수행하기 때문에 Fragment가 설 자리는 없어진다.

실제로 React나 Flutter같은 선언형 UI 들을 보면 fragment같은 개념이 없다.

<br/><br/>

### 선연형 UI

---

명령형 프로그래밍과 선언형 프로그래밍이란 단어를 한 번쯤 들어 보았을 것이다.

명령형 프로그래밍은 우리가 흔히 코딩하듯이, 컴퓨터에게 하나하나 명령을 하는 형태로 코딩을 하는 것이다.

반면에 선언형 프로그래밍은 컴퓨터에게 우리가 원하는 것을 선언 또는 표현하듯이 코딩 하는 것이다.



##### 명령형 프로그래밍

명령형 코드를 먼저 살펴보자

```kotlin
val myList = listOf(1,2,3)
val targetList = mutableListOf<Int>()
for (i in myList) {
    if (i % 2 == 0) tempList.add(i)
}
```

로직이 간단하기 때문에, 코드를 순차대로 훑어 보면 이 5줄의 코드가 어떤 의미를 가지는 코드인지 알 수 있다.

먼저 myList라는 숫자를 담는 list가 있다.
targetList라는 임시 리스트를 만들었다.
for문을 사용해서 myList의 모든 요소를 한 번씩 순회 한다.
순회하면서 각 요소의 나머지가 0인 것들을 targetList에 담는다.
컴퓨터에게 구체적으로 하나하나 명령한 이 코드를 따라가다 보니, 그제서야 list에서 짝수만 필터링하는 기능이라는 것을 알 수 있다. 
우리는 방금 리스트에서 짝수만 필터링하는 그 과정, 즉 필터링을 하기 위해 어떻게(how) 하는지를 본 것이다.

이같은 프로그래밍 방법이 명령형 프로그래밍이다.

##### 선연형 프로그래밍

반대로 선언형 프로그래밍은 다음과 같다.


```kotlin
val targetList = listOf(1,2,3).filter { it % 2 == 0 }
```

같은 필터링 기능인데, 세부 동작 하나하나를 명령한 느낌이 아니다.

“나는 이 list에서 이것들을 필터링 할거야”라고 선언한 것에 불과한다.

내부적으로 어떻게(how) 할지는 관심이 없다. 단지 “이렇게 표현할래”를 나타낸 것이다.

우리는 컴퓨터가 아니라 사람이다. 

우리는 특정 개발자가 컴퓨터에게 명령한 로직들을 보고 그게 무슨 기능인지 이해하는 것보단, 개발자가 원했던 기능의 선언을 보고 이해하는 게 훨씬 익숙하다.

그게 바로 선언형 UI가 대세가 된 이유라고 생각하다.

선언형 프로그래밍은, 결국 잘 된 추상화라고도 볼 수도 있다.

filter 함수도 내부 로직을 타고 들어가면 결국엔 컴퓨터에게 명령을 하는 코드로 되어있다.

다만 그걸 잘 추상화 하는 게 참 어려운 일이고, 그게 특히나 단순 로직의 추상화가 아니라 UI를 그리는 영역이라면 더더욱 어렵다.

누군가 그 어려운 일을 다 해준다면 우리는 컴퓨터에게 원하는 UI를 선언하기만 하면 된다(고맙게도 Jetpack Compose가 그 어려운 일을 다 해주고 있다).

##### 명령형 UI 프로그래밍

명령형 UI 프로그래밍은 아래와 같다.

```kotlin
val linearLayout = LinearLayout()
linearLayout.orientation = VERTICAL

val textView1 = TextView().apply {
    text = "hello"
}
val textView2 = TextVew().apply {
    text = "world"
}
linearLayout.add(textView1)
linearLayout.add(textView2)
```
아까 for 문을 돈 것과 비슷하게, 컴퓨터에게 명령을 내리고 있다.

코드를 다 보고, 어디에 무엇이 add 되는지 다 읽고 나면, “아~ textView 두 개가 위아래로 배치되어 있도록 그리는 코드이구나”라는 걸 이해하게 된다.

##### 선언형 UI 프로그래밍

이번엔 같은 기능을, 명령하지 않고 선언해보겠다.

```xml
<LinearLayout
    ...
    android:orientation="vertical">
    <TextView
				...
        android:text="hello"/>
    <TextView
        ...
        android:text="world"/>
</LinearLayout>
```

자신이 원하는 UI를 그저 표현했다. 어떤 UI가 될지 아까보다 훨씬 유추하기 쉽다.

사실 XML도 선언형이기 때문에 Android 개발자라면 이미 선언형 UI에 익숙한 것이다. 다만 XML 파일에 선언하였기 때문에 더 다이나믹하게 UI 자체를 바꾸는 건 굉장히 귀찮고 어렵다.

위의 코드를 컴포즈로 바꿔보면, 아래와 같다.


```kotlin
@Composable
fun MyView() {
    Column {
        Text("hello")
        Text("world")
    }
}
```

만약 다른 뷰로 바꾸고 싶으면, 그저 다른 Composable 함수를 호출하면 그만이다. Fragment 사용해서 바꿀 필요가 없다.

마지막으로 일단 Compose에 대해 마지막으로 정리를 하자면 

1) Compose는 Kotlin으로 제작된 UI 제작 도구

2) Compose는 불필요한 코드가 줄어듬

3) Compose는 직관적인 코드로 UI를 만들 수 있음 


위의 1~3번 특징 때문에 버그가 일어날 확률도 줄고 적은 코드로 UI 구현이 가능해서 생산성이 빨라지고 유지보수하기 쉽다.

<br/><br/>


### 마무리

---

Compose라는 것에 대해 처음 알게 됐을때 정말 충격이었다.

그동안 알던 방식이 아닌 방식으로 UI를 그리는 방식이 있다는 것을 알고

기존에 알고 있던 틀들이 다 깨지는 기분이었다.

그동안 프로젝트를 몇번 하는동안 xml과 view간의 상속 코드를 짜는 과정에서 앞서 설명한 문제점들을 다 겪어본적이 있어서 더 신선한 충격이었던 것 같다.

또 이번 사이드 프로젝트만 끝나면 평소에 생각해두었던 아이디어로 만들고 싶은 앱이 하나 더 있었기에

그 앱을 만들때 코틀린 언어로 Compose를 사용하여 앱을 개발할 생각이다.

얼른 Compose를 유용하게 다룰 수 있는 실력이 되었으면 좋겠다.







