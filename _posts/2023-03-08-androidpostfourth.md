---
layout: single
title:  "[Android] 안드로이드 LiveData와 DataBinding 같이 사용하기"
categories: Android
published: true
---

오늘은 얼마전에 다루었던 LiveData와 DataBinding을 함께 사용하는 방법에 대해서 알아볼 것이다. 

LiveData와 DatabBinding을 함께 사용할 경우 이 둘의 효율을 더 증가시킬 수 있다.

이유는 DataBinding을 이용해 View에 LiveData를 Binding 시키면 LiveData의 값이 변경될 때 View의 데이터가 자동으로 바뀌기 때문에 소스코드를 이용한 데이터 셋팅같은 코드를 줄일 수 있다.

이렇게 된다면 코드를 작성할 때 훨씬 편하게 작성할 수 있게되고 데이터의 변경만 신경쓰면 된다.

LiveData와 DataBinding이 아직 뭔지 잘 모르겠다면 아래 포스팅을 참고하면 된다.

[안드로이드 LiveData 기본 사용법](https://nam8399.github.io/android/androidpostfirst)

[안드로이드 DataBinding 기본 사용법](https://nam8399.github.io/android/androidpostthird)


# LiveData와 DataBinding을 함께 사용하는 방법

---


## app 수준의 Build.gradle 파일 수정

```gradle
android {

    ...
 
    dataBinding {
        enabled = true
    }
}
```

위와 같이 android 부분에 dataBinding을 추가해준다.



## DataBinding을 사용 할 xml 파일 수정

**activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="activity"
            type="com.imaec.bindingadapterex.MainActivity" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/text_view"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{activity.liveText}"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toTopOf="@id/btn_change1"
            app:layout_constraintVertical_chainStyle="packed"/>

        <Button
            android:id="@+id/btn_change1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Change Text1"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/text_view"
            app:layout_constraintBottom_toTopOf="@id/btn_change2"/>
      
        <Button
            android:id="@+id/btn_change2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Change Text2"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/btn_change1"
            app:layout_constraintBottom_toBottomOf="parent"/>

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```


위와 같이 기존 xml 레이아웃 형식을 layout 태그로 감싸준다.
  
TextView의 text가 자동 변경됨을 확인하기 위해 Button도 추가해주었다.

variable에는 MainActivity를 추가해줌으로서 해당 클래스의 변수 및 객체를 사용할 수 있도록 해준다.

Textview에는 activity의 liveText를 데이터바인딩 해준다.



## Activity에서 LiveData 변경


**MainActivity.kt**


```kotlin
private lateinit var binding: ActivityMainBinding

val liveText = MutableLiveData<String>()
```

binding과 liveText를 선언 및 정의해준다.

```kotlin
binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
binding.apply {
    lifecycleOwner = this@MainActivity  // **중요** binding에 LifeCycleOwner을 지정해줘야 LiveData가 실시간으로 변화
    activity = this@MainActivity        // xml 파일에 선언한 activity

    btnChange1.setOnClickListener {
        liveText.value = "Hello LiveData!"
    }
    btnChange2.setOnClickListener {
        liveText.value = "Hello DataBinding!"
    }
}

liveText.value = "Hello DataBinding!"
```

onCreate() 안에서 위처럼 binding을 정의해준다.

lifecycleOwner와 activity를 지정해준뒤 버튼을 누르면 LiveData를 이용해서 정의한 text.value를 변경시켜주도록 한다.

그러면 버튼을 누를때마다 Text가 바뀌게 된다.

위 코드를 보면 xml에서 정의한 Textview의 text값을 변경해주는 코드는 없지만 버튼을 누를때마다 TextView의 값이 바뀌는 것을 볼 수 있다.

btnChange1 누를 시 Hello LiveData!로 변경, btnChange2 누를 시 Hello DataBinding! 으로 변경

간단한 예제이지만 LiveData와 DataBinding을 함께 사용함으로서 얻는 장점을 잘 보여주는 코드라고 생각한다.

예제에는 TextView만 있지만 ImageView나 EditText 등에서도 사용할 수 있고 본인은 실제 작업중인 사이드 프로젝트에서도 EditText의 값을 DataBinding과 LiveData를 함께 이용해 사용중이다.


<br/><br/>
# 마무리


---

LiveData나 DataBinding 같은 라이브러리들을 사용하면서 가장 중요하는 것은 사용하는 이유에 대해 명확히 아는 것이라고 생각한다.

개발하다보면 이러한 부분을 망각한채 사용하는 경우도 많은데 그럴때마다 라이브러리를 사용하는 이유와 사용함으로서 얻을 수 있는 장점들을 충분히 고려한채 사용하는 습관을 가지는게 중요할 것 같다.
