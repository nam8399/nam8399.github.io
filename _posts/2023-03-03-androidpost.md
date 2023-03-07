---
layout: single
title:  "[Android] 안드로이드 LiveData 기본 사용법"
categories: Android
---

오늘은 안드로이드 JetPack 라이브러리 중 하나인 LiveData에 대해서 작성해보았다.

장점도 굉장히 많고 다른 라이브러리(DataBinding, ViewModel 등)들과 함께 사용할 경우 훨씬 더 유용한 LiveData의 개념과 사용법에 대해서 알아보자

# LiveData란?


---

LiveData는 데이터의 변경을 관찰할 수 있는 Data Holder 클래스이다.

관찰이라고 하면 Observable이 생각날텐데 일반적인 Observable과는 다르게 LiveData는 안드로이드의 LifeCycle, 즉 생명주기를 알고있다.

Activity, Fragment, Service 등과 같은 안드로이드 컴포넌트의 생명주기를 인식하며 그에 따라 LiveData는 사용자에게 보여지는 활성상태일때만 데이터를 업데이트 한다.

이때 활성상태는 Started, Resume 상태를 말하며 생명주기가 Destroyed가 되면 관찰자를 삭제할 수 있다.

또한 LiveData 객체는 Observer 객체와 함께 사용되고 LiveData가 가지고 있는 데이터에 어떠한 변화가 일어날 경우 LiveData는 등록된 Observer 객체에 변화를 알려주며 Observer의 onChanged() 메서드가 실행되게 된다.



<br/><br/>
# LiveData가 생명주기를 인식하는 법


---

안드로이드의 생명주기를 알고 있는 LifeCycleOwner를 통해 인식할 수 있다.

LifeCycleOwner는 메서드가 getLifeCycle() 뿐인 단일 메서드 인터페이스 클래스이며 Activity나 Fragment에서 이를 상속하고 있다.

즉 LiveData의 Observer 메소드의 LifeCycleOwner를 Activity나 Fragment를 변수로써 사용한다면 각 화면 별 생명주기에 따라 LiveData는 자신의 임무를 수행한다.




<br/><br/>
# LiveData의 장점


---

### 1. UI 데이터 상태의 일치 보장

앱 데이터 및 라이프 사이클이 변경될 때 마다 observer을 통해 데이터를 변경할 수 있다.

### 2. 메모리 누출 없음
 
연결된 수명 주기가 끝나면 자동으로 삭제된다.

### 3. 중지된 활동으로 인한 비정상 종료 없음

관찰자의 수명 주기가 비활성화 상태이면 관찰자는 어떤 Live Data 이벤트도 받지 않는다.

### 4. 수명주기를 수동으로 처리하지 않음

수명 주기의 변경을 자동으로 인식함으로 수동으로 처리하지 않는다.

그러므로 UI 컴포넌트는 그저 관련 있는 데이터를 **관찰**하기만 하면 된다.

### 5. 최신 데이터 유지

수명 주기가 비활성화 일 경우 다시 활성화가 될 때 새로운 데이터를 받는다.

디바이스가 세로에서 가로로 변경되는 경우에도 LiveData는 회전하기 전의 최신 상태를 즉시 받아온다.

-> 예전과 같이 데이터를 유지하기 위해 SharedPreference를 사용하지 않아도 됨


### 6. 자원 공유 가능

LiveData를 상속하여 자신만의 LiveData클래스를 구현할 수 있고 싱글톤 패턴을 이용하여 시스템 서비스를 둘러싸면(Wrap) 앱 어디에서나 자원을 공유 할 수 있다.
 
<br/><br/>
# LiveData 사용시 주의할 점


---

Generic을 사용해 관찰하고자 하는 데이터의 타입(Type)을 갖는 LiveData 인스턴스를 생성한다.

LiveData 클래스의 observe() 메소드를 사용해 Observer 객체를 LiveData 객체에 "결합" 한다. 이때 observe() 메소드는 LifecycleOwner 객체를 필요로 하며 보통은 Activity를 전달한다.

LiveData에 저장된 데이터에 어떠한 변화가 일어난 경우 결합된 LifecycleOwner에 의해서 상태가 active(활성)인 한 모든 데이터에 대해 Trigger가 발생한다.

Observer 객체를 생성한다. 생성시 LiveData가 들고있는 데이터가 변화가 일어났을 때 수행해야 할 로직이 들어있는 onChanged() 메서드를 정의해야 한다.

보통은 액티비티나 프래그먼트 같은 UI Controller 내에서 해당 메서드를 생성한다.

observeForever(Observer)를 통해 LifeCycleOwner 없이 Observer를 생성하여 등록할 순 있지만, 이 경우에는 Observer는 항상 active(활성) 상태이므로 데이터 변화를 항상 전달 받는다. 단, removeObserver(Observer) 메소드를 통해 Observer를 제거 할 수 있다.


<br/><br/>

# LiveData 기본 사용법

---

먼저 build.gradle에서 종속성을 확인해준다.

```gradle
implementation 'androidx.appcompat:apcompat:1.3.0'
```

그다음 LiveData를 사용할 곳에서 LiveData를 정의해준다.

```kotlin
private var liveText: MutableLiveData<String> = MutableLiveData()
```

LiveData는 abstract class이기에 LiveData Class를 상속받은 MutableLiveData를 사용한다.

**LiveData : get만 가능**

**MutableLiveData : set/get 가능**

그 후 LiveData에 Observer를 달아준다.

```kotlin
// LiveData value의 변경을 감지하고 호출
liveText.observe(this, Observer {
    // it로 넘어오는 param은 LiveData의 value
})
```

여기서 첫 번째 매개변수 this는 LifeCycleOwner인 MainActivity이다.

두 번째 매개변수 Observer Callback은 LiveData(liveText)의 value의 변경을 감지하고 호출되는 부분이다.

**actovity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/text_test"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@id/btn_change"
        app:layout_constraintVertical_chainStyle="packed"/>

    <Button
        android:id="@+id/btn_change"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ADD 1"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/text_test"
        app:layout_constraintBottom_toBottomOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```


**MainActivty.kt**

```kotlin
class MainActivity : AppCompatActivity() {

    private var liveText: MutableLiveData<String> = MutableLiveData()
    private var count = 0 // button을 누르면 증가 될 숫자

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // LiveData의 value의 변경을 감지하고 호출
        liveText.observe(this, Observer {
            // it로 넘어오는 param은 LiveData의 value
            text_test.text = it
        })

        btn_change.setOnClickListener {
            // liveText의 value를 변경
            // liveText 자체를 변경시키면 안됨
            liveText.value = "Hello World! ${++count}"
        }
    }
}

```
 
위와 같이 코드를 작성하고 실행을 하게 되면 버튼을 누를때마다 Text값의 숫자가 늘어나게 된다.







<br/><br/>
# 마무리


---

오늘은 LiveData의 간단한 사용법을 통해 어떠한 기능인지 정도로만 알아보았는데 LiveData의 경우 초반에 말했듯이 ViewModel이나 DataBinding 등과 함께 써야 시너지가 좋기에 나중에 이 부분에 대해서 기회가 된다면 한번 더 다뤄보도록 하겠다.

사실 이번에 LiveData를 알게되고 여러 예제들을 통해 LiveData를 사용하는 여러 방법들을 접해보면서 정말 많은 흥미를 느꼈다.

예전과 같이 일일히 생명주기에 맞춰서 데이터를 결합하는 코드를 작성하지 않아도 되고 View와 ViewModel간의 관계에서도 분리된 상태에서 유연하게 데이터를 셋팅할 수 있는 모습을 보고 굉장히 신선했다.

사실 나온지 어느정도 됐는데 이제야 제대로 알게된것도 반성하게 된다..

현재 진행중인 사이드 프로젝트에도 적용해볼 예정이고 아직 미숙하기에 차근차근 써가면서 더 깊게 이해해보도록 하겠다.
