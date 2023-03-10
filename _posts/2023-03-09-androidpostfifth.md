---
layout: single
title:  "[Android] 안드로이드 LiveData와 ViewModel 같이 사용하기"
categories: Android
published: true
---

오늘은 어제에 이어서 LiveData와 ViewModel을 함께 사용하는 방법에 대해서 알아볼 것이다. 

일단 View와 ViewModel의 관계에 대해서 간단하게 설명하자면 View는 ViewModel 객체를 멤버로 가지고 있지만 ViewModel은 View의 객체를 가지고 있지 않는다.

즉 ViewModel은 View의 존재를 몰라야하고 MVVM 패턴의 특징중 하나라고 볼 수 있다.



[안드로이드 LiveData 기본 사용법](https://nam8399.github.io/android/androidpostfirst)

[안드로이드 LiveData와 DataBinding 같이 사용하기](https://nam8399.github.io/android/androidpostfourth/)


# LiveData와 ViewModel 함께 사용하는 방법

---

위에서 말한 것처럼 ViewModel은 View의 존재를 모르는데 ViewModel에서 View의 함수를 호출하거나 View의 내용을 변경하거나, 아니면 Context나 Activity 객체의 함수를 호출해야 할 때는 어떻게 해야할까?

AndroidViewModel을 상속하여 Context를 이용하는 방법도 있지만 오늘 알아볼 해답은 View가 ViewModel의 특정 데이터를 Observing하고 있다가 그 데이터가 변경될 때 View의 로직을 수행하는 것이다.

아래에서 한번 간단한 예제을 통해 알아보자



## 간단 사용법

View가 ViewModel의 멤버중 하나인 number를 Observing 하고 있다면 그 뒤 ViewModel이 number = 1과 같이 number를 변경하게 될 때 View가 ViewModel.number의 변경사항을 알아채고 미리 설정해둔 로직을 수행하게 된다.

말보다는 아래 예시를 보면 이해하기 쉬울 것이다.



```kotlin
class MyViewModel:ViewModel{
    val number = MutableLiveData<Int>() // LiveData의 값을 변경하기 위해 MutableLiveData 사용

    fun changeNumber(num:Int){
        number.postValue(num)
    }
}

class MyView:View{
    val myViewModel = MyViewModel()

    init {
        myViewModel.number.observe(this, Observer {
            my_text_view.text = "number is $it"
        })
    }

    fun changeViewModelNumber(){
        myViewModel.changeNumber(num)
    }
}
```

위처럼 작성하면 MyView.changeViewModelNumber()를 호출할 때 ViewModel의 number가 postValue를 통해 바뀌게 되고 이를 myViewModel.number.observe(this, Observer{})를 통해 Observing하고 있던 뷰가 감지하여 my_text_view의 text를 변경하게 된다.

참고로 postValue와 setValue의 차이점에 대해서 간단하게 말하면

setValue는 메인쓰레드에서 LiveData의 값을 변경해주고 postValue는 백그라운드에서 값을 변경해주는 차이가 있다.

상황에 맞게 코드를 작성하면 된다.

<br/><br/>
# 마무리


---

사이드 프로젝트에 위 라이브러리들을 적용하면서 계속 사용하다보니 정말 간편하고 효율성이 좋음을 많이 느낄 수 있었다.

그리고 그동안 ViewModel에 관한 포스팅을 한적이 없었기에 이 주제에 대해서도 조만간 다뤄보도록 하겠다.

