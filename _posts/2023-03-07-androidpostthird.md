---
layout: single
title:  "[Android] 안드로이드 DataBinding 기본 사용법"
categories: Android
published: true
---

오늘은 안드로이드 개발자들 사이에서 이미 많이 사용중인 DataBinding에 대해서 알아볼 것이다.

Compose가 나오면서 점차 식어가는 추세지만 xml로 뷰를 그리는 방식을 사용한다면 굉장히 유용한 라이브러리다.

# DataBinding이란?

---

안드로이드에서의 DataBinding이란 이전 포스팅에서 설명했던 AAC의 한 부분으로서 **UI 요소와 데이터를 프로그램적 방식으로 연결하지 않고, 선언적 형식으로 결합할 수 있게 도와주는 라이브러리를 말한다.**

간단하게 예를 들자면

```kotlin
findViewById<TextView>(R.id.sample_text).apply {
        text = viewModel.userName
    }
```

위와 같이 TextView에 viewModel에서 가져온 userName을 선언하는 코드를

```xml
<TextView
        android:text="@{viewmodel.userName}" />
```
이렇게 레이아웃에서 직접 결합하게 할 수 있다.

이를 선언적(declarative) 레이아웃 작성이라고 한다. 

위와 같이 데이터바인딩을 사용하게 되면 파일이 더욱 단순화 되어 유지관리가 쉬워지고 메모리 누수 방지, null 위험을 방지할 수 있는 장점이 있다.


<br/><br/>
# DataBinding 기본 사용법


---



### app 수준의 Build.gradle 파일 수정

```gradle
android {

    ...
 
    dataBinding {
        enabled = true
    }
}
```

위와 같이 android 부분에 dataBinding을 추가해준다.



### DataBinding을 사용 할 xml 파일 수정

**activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/text_view"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello World!"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"/>

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

### 사용하려는 Activity에서 binding Setting

위와 같이 기존 xml 레이아웃 형식을 layout 태그로 감싸준다.
  
xmlns 선언 부분도 layout에 다 옮겨줘야 한다.

**MainActivity.kt**
  
```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
}
```

위의 과정들을 모두 진행하면 DataBinding을 사용하기 위한 준비는 끝난다.
  
이제 기본적인 사용방법을 보자

### xml 파일에서 data 정의


**activity_main.xml**

먼저 xml 파일에서 data를 정의해준다.

```xml
<data>

    <variable
        name="activity"
        type="com.imaec.databindingex.MainActivity" />
</data>
```

그 다음으로는 ACtivity에서 data를 연결해준다.

```xml
<TextView
    android:id="@+id/text_view"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{activity.text}"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintBottom_toTopOf="@id/btn_change"
    app:layout_constraintVertical_chainStyle="packed"/>
```

마지막으로는 Button을 하나 만들어서 click했을때 데이터 처리를 할 수 있도록 한다.

```xml
<Button
    android:id="@+id/btn_change"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Change Text"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/text_view"
    app:layout_constraintBottom_toBottomOf="parent"/>
```


이 소스들을 다 작성하면

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="activity"
            type="com.imaec.databindingex.MainActivity" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/text_view"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{activity.text}"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toTopOf="@id/btn_change"
            app:layout_constraintVertical_chainStyle="packed"/>

        <Button
            android:id="@+id/btn_change"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Change Text"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/text_view"
            app:layout_constraintBottom_toBottomOf="parent"/>

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

위와 같이 작성된다.

### Activity 파일 수정


**MainActivity.kt**

```kotlin
var text = "TEST"
```

```kotlin
binding.activity = this
binding.btnChange.setOnClickListener {
    text = "TEST2"
    binding.invalidateAll()
}
```

위에서 binding.activity는 xml에서 데이터에 정의해주었던 data의 이름이다.

그리고 invalidateAll() 함수의 경우 data가 변한 후 연결된 view들의 변화를 알려주는 함수이다.

최종 소스는 아래와 같다.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    var text = "TEST"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        binding.activity = this
        binding.btnChange.setOnClickListener {
            text = "TEST2"
            binding.invalidateAll()
        }
    }
}
```

위와 같이 소스를 작성하게 되면 앱을 실행할 때 TextView 부분에 TEST가 표시되고 Button을 클릭하면 TEST2로 바뀌게된다.

위는 매우 간단한 소스이기에 DataBinding의 장점에 대해서 크게 와닿지 않을 수 있다.

나중에는 LiveData와 결합하여 사용하는 등의 예제도 한번 작성해보도록 하겠다.

<br/><br/>
# 마무리


---

요즘 매일 퇴근 후에 집에와서 앱 개발 공부와 사이드 프로젝트 개발을 하는데 현재 진행중인 사이드 프로젝트에 MVVM 패턴으로 LiveData, DataBinding, ViewModel, LifeCycle 등의 AAC 라이브러리 등을 적용하면서 굉장히 큰 흥미를 느끼고 있다.

그리고 또 retrofit2와 코루틴을 결합해서 사용하고 있는데(이 부분도 나중에 포스팅 할 예정) 이러한 여러 라이브러리들을 적용 및 사용함으로서 정말 기존에 했던 앱개발들에 비해서 훨씬 많은 것들을 배우고 있는 것 같다.

앞으로도 꾸준히 공부하면서 부족한 부분들을 채워나갈 예정이고 추가적으로 이번 사이드 프로젝트가 끝나면 IOS 앱개발도 시작할 예정이다.

올해 정말 많은 성장을 이뤄내고 싶다.
