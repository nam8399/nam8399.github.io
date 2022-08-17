---
layout: single
title:  "모바일앱 개발일기 #17 디자인패턴 MVC와 MVP의 차이, MVP 패턴이란?"
categories: 모바일앱개발일기
---

오랜만에 회사에서 글을 써본다.

우리 회사에서는 MVP 디자인 패턴을 사용하는데 이 패턴에 대해서도 한번 정리해보고자 글을 작성했다.

일단 MVP 디자인패턴과 유사해 보이는 MVC에 대해서 간단하게 알아보자.

<br/><br/>

### MVC

---


먼저 MVC 패턴에 대해서 간단하게 설명해보면

MODEL : 데이터 관리

View : 유저에게 보여주는 화면

Controller : 사용자의 요청을 인식하여 Model에서 요청에 맞는 데이터를 가져오고  View에 적용
<br/><br/>

##### 안드로이드에서의 MVC

안드로이드에서 MVC를 매칭시켜보면

View : Activty(View, Fragment)

Controller : Activity(Button.setOnClickListener)

Model : Model

<br/><br/>

View와 Controller 모두 Activity에서 처리할 수 있다는 것을 알 수 있다.

당연히 View와 Controller가 하나의 클래스에서 이루어지기 때문에 길이가 길어지고, 가독성이 떨어지는 복잡한 코드가 될 수가 있다.

그래서 대안으로 나온게 MVP패턴이다.

<br/><br/>

### MVP

---

MVC의 MV는 같지만 Controller대신 Presenter가 들어간 것이 MVP패턴이다.

Presenter : Contrller와 역할이 비슷하지만 Interface를 사용한다는 것의 차이가 있다. View에서 전달된 이벤트에 따라 Model에서 데이터 요청후 전달하는 중간 역할 담당.

##### MVC와 MVP 차이

둘의 차이를 보면 MV까지는 똑같고 C와 P만 다른 것을 볼 수 있는데

MVC 패턴에서는 모델이 View 와 바로 연결될 수 있는 것에 비해 MVP 패턴에서는 무조건 Presenter를 이용해서 연결해야 하는 것을 볼 수 있다.

안드로이드에서 MVC구조는 사실상 액티비티나 프래그먼트에 컨트롤러와 뷰에 관한 코드를 전부 집어넣어서 MVC패턴이라 하기도 애매하고 코드가 복잡해진다는 단점이 있다

그에 비해 MVP는 Presenter를 만들어 모델과 뷰를 분리해주고 Presenter를 통해 모델과 뷰를 소통하게 해주므로 코드가 더 깔끔해지고 유지보수 하기 좋게 해주는 효과가 생긴다.

MVP구조의 장점은 그림에서 MVC구조와 비교하면 알 수 있듯이 뷰와 모델의 의존성이 사라진다는 것이다.

단점은 View와 Model 사이의 의존성은 해결되었지만, View와 Presenter 사이의 의존성이 높아졌기 때문에 어플리케이션이 복잡해 질 수록 View와 Presenter 사이의 의존성이 강해지는 단점이 있다.


<br/><br/>

### MVP 패턴 예제

---

##### Presenter


```java
//Contract Interface
//View와 Presenter를 연결하기 위한 Interface
package com.example.mvp.Presenter;

public interface Contract {
    interface View{
        void showResult(int answer);      //값을 보여줄 View 메소드 선언
    }
    interface Presenter{
        void addNum(int num1, int num2);  //결과 값 구하기 위한 메소드 선언
    }
}
```

```java
//MainPresenter class
//Model과 View를 연결하여 동작을 처리해
package com.example.mvp.Presenter;

import com.example.mvp.Model.MainModel;

//Presenter줌
public class MainPresenter implements Contract.Presenter {
    Contract.View view;
    MainModel mainModel;
    public MainPresenter(Contract.View view){
        this.view = view;                   //Activty View정보 가져와 통신
        mainModel = new MainModel(this);    //Model 객체 생성
    }
    
    //Presenter를 상속하고 addNum 구현
    @Override
    public void addNum(int num1, int num2) {
        view.showResult(num1 + num2);
    }
}
```
<br/><br/>
##### View

```java
//MainActivity class
package com.example.mvp.View;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import com.example.mvp.Presenter.Contract;
import com.example.mvp.Presenter.MainPresenter;
import com.example.mvp.R;

//View
public class MainActivity extends AppCompatActivity implements Contract.View {
    private EditText number1;		//입력할 EditText
    private EditText number2;		//입력할 EditText
    private Button sumButton;
    private Contract.Presenter presenter;	//presenter와 통신하기 위해 객체 생성
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        presenter = new MainPresenter(this);
        init();
    }

    private void init(){
        sumButton = (Button)findViewById(R.id.sum);
        number1 = (EditText)findViewById(R.id.number1);
        number2 = (EditText)findViewById(R.id.number2);
        
        //버튼 클릭
        sumButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            	//결과 값 계산
                presenter.addNum(Integer.parseInt(number1.getText().toString()),
                        Integer.parseInt(number2.getText().toString()));
            }
        });
    }
    @Override
    public void showResult(int answer) {
        ((TextView)findViewById(R.id.result)).setText(Integer.toString(answer));
    }
}
```

<br/><br/>
##### Model

```java
//MainModel class
//데이터 관리를 해줄 클래스
package com.example.mvp.Model;

import com.example.mvp.Presenter.Contract;

//Model
public class MainModel {
    Contract.Presenter presenter;
    public MainModel(Contract.Presenter presenter){
        this.presenter = presenter;
    }
    public saveData(int data){
    	//처리 로직
    }
}
```


Contract : View 와 Presenter의 연결 수단이라 생각하면 편하다. 내부 인터페이스로 View와 Presenter가 있는데 각각 자신의 계층에서 사용할 수 있는 함수들이다. View쪽은 Contract.View,  Presenter는 Contract.Presenter를 상속받아 사용한다. 추가로 설명하면 View쪽은 자신의 Contract.View를 Presenter의 생성자로 넘겨주는데 Presenter는 로직을 처리하고 뷰에 변경사항이 필요하면 넘겨진 생성자 즉 뷰의 함수를 사용하여 뷰에게 화면을 어떻게 바꿔줘 하고 요청하게 되는 형식이다. 

Presenter : 로직처리를 담당한다. 그리고 Model과 연결되어있다. 로직처리 및 외부 API 또는 로컬 DB와 통신하고 결과에 따라 View의 변경을 요청하는 역할을 한다.

### 마무리

---

요즘 회사에서 정신 없이 일만 하다보니 시간이 금방 지나간 것 같다.

그동안 매일 퇴근하고 시간내서 따로 사이드 프로젝트로 작업하던 커뮤니케이션 앱이 드디어 어제 배포됐다. (물론 아직도 손볼 곳이 많다..)

이번 배포된 앱만 안정화를 어느정도 시킨다면 본격적으로 코틀린 공부를 시작하고 코틀린을 사용해서 앱도 하나 만들 생각이다.

올해 안을 목표로 능숙하게 자바와 코틀린을 사용해서 앱을 개발할 수 있도록 하고 안드로이드 지식을 더 쌓고 싶다.

그리고 시간나면 외주도 받아볼 생각이다. 

하고싶은건 많은데 부족한 점도 많으니 지금까지처럼 매일 퇴근하고 남는 시간을 이용해서 더 성장하도록 할 것이다.

