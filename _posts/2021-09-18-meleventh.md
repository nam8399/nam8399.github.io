---
layout: single
title:  "모바일앱 개발일기 #11 CountDownTimer 사용해서 버튼 활성화하기"

---

#### 2021-09-18

요즘 여러가지 바쁜 일들이 있어서 포스팅을 못올렸다. 

그동안 졸업작품도 시간날때마다 하고 학교생활하느라 바빴다.

 <br/><br/>

졸업작품을 하다가 CountDownTimer라는 것을 사용할 일이 생겼다. 

Timer는 별도의 Thread로 처리되므로 상속받는 Class를 만들어서 처리해야 한다고 한다.

```java
class TimerRest extends CountDownTimer {
    public TimerRest(long millisInFuture, long countDownInterval) {
        super(millisInFuture, countDownInterval);
    }
            
    @Override
    public void onTick(long millisUntilFinished) {
        // To-Do
    }
    
    @Override
    public void onFinish() {
 
    }
}
```

 <br/><br/>

CountDownTimer의 기본 형식이다. 

맨 처음에 CountDownTimer를 상속받는 클래스를 생성해주고 Timer가 필요한 시점에 클래스를 생성하고 시작을 했다.

TimerRest에 있는 millisInFuture는 타이머를 수행할 시간을 적어주고 countDownInterval에는 타이머를 수행할 간격을 적어준다.

onTick은 수행 간격마다 호출되는 함수인데 millsUntilFinished 에는 남은 시간이 표시된다.

마지막으로 onFinish에는 millisInFuture 시간까지 모두 종료시 호출되는 함수이다.

<br/><br/>

그렇다면 CountDownTimer를 사용해서 버튼을 활성화 및 조작하는 예시를 보도록 하겠다.

```java
new CountDownTimer(10000, 1000) {
            public void onTick(long millisUntilFinished) {
                btn_feed2.setEnabled(false);
                btn_feed2.setText("하루에 한번 사료을 다 먹었을 때 눌러주세요 (남은시간 : " + millisUntilFinished / 1000 + ")");
            }
            public void onFinish() {
                btn_feed2.setEnabled(true);
                btn_feed2.setText("오늘 사료량 측정");
            }
        }.start();
```

new CountDownTimer(10000, 1000)라고 작성하고 10초동안 1초 간격으로 실행되게 하였다.

처음에는 btn_feed2.setEnabled(false); 를 통해 버튼을 비활성화 시켰다가 millisUntilFinished로 몇초 뒤에 버튼이 활성화 되는지 실시간으로 보여주도록 하였다.

그리고 10초가 지나면 btn_feed2.setEnabled(true);를 통해 버튼을 활성화 시키고  btn_feed2.setText("오늘 사료량 측정");로 버튼에 쓰일 최종 텍스트를 설정해주었다.

 <br/><br/>

이처럼 CountDownTimer를 통하여 간단하게 버튼을 조작하는 방법을 알아보았다.
