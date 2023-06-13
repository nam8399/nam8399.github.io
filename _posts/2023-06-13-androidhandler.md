---
layout: single
title:  "[Android] Handler란? Handler와 Looper"
categories: Android
published: true
---

오늘은 안드로이드의 Handler와 Looper가 무엇인지, 어느 때에 사용하는지에 대해서 알아보도록 할 것이다.

사실 앱 개발하면서 굉장히 많이 사용했던 Handler인데 이에 대해서 자세하게 한번 다뤄보고자 쓰게 되었다.

<br/><br/>

## Handler에 대해 알아보기 전에..

---

안드로이드 앱 개발을 어느정도 했다면 안드로이드 앱에서 사용자에게 보여지는 모든 UI의 작업은 Main Thread에서만 진행된다는 것을 알고있을 것이다.

혹시라도 모르는 사람들을 위해 UI 작업을 Main Thread에서만 진행하는 이유에 대해서 간단하게 말하자면 하나의 뷰에 여러 군데에서 동시다발적으로 접근했을 때의 문제점을 없애기 위해서이다.

만약 UI 작업을 모든 곳에서 접근할 수 있다고 가정한다면 Main Thread와 Worker Thread에서 동시에 하나의 뷰를 변경하려고 했을 시에 앱 상에서는 우선순위를 모르기 때문에 문제가 생기게 된다.

이러한 문제들을 방지하기 위해 Main Thread에서만 UI에 접근할 수 있도록 설계되어있다.

그렇다면 Worker Thread에서 UI를 업데이트 할 수 있는 방법은 없는 것일까??

이럴때 사용하는 것이 바로 Handler이다.

<br/><br/>

## Handler란?

Handler(핸들러)란, Worker thread 에서 Main thread 로 메시지를 전달해주는 역할을 하는 클래스이다.

핸들러는 핸들러 객체를 만든 스레드와 해당 스레드의 Message Queue에 바인딩된다.

여기서 Message queue는 핸들러가 전달하는 message를 보관하는 FIFO(First In First Out) 방식의 큐이다. 

다른 스레드(Worker Thread)에게 메시지를 전달하려면 메시지를 전달하려는 스레드에서 생성한 핸들러의 post() 와 sendMessage() 등의 함수를 사용해야한다. 

그래야 수신대상 Message Queue에 메시지가 저장되기 때문이다.

Message Queue에 저장된 Message나 Runnable은 Looper가 들어온 순서대로(FIFO 방식이기에) 꺼내서 핸들러에게 전달해준다.

그렇다면 핸들러는 handlerMessage() 메서드를 이용하여 Looper에게서 받은 Message나 Runnable을 처리하게 되는 것이다.

이게 전체적인 Handler 동작의 흐름이라고 볼 수 있는데 아직 구성요소에 대해서 익숙하지 않을 수 있으니 구성요소에 대해서 한번 알아보도록 해보자.

<br/><br/>

## Handler의 구성요소

위와 같은 핸들러 동작을 하기 위해서는 필수로 존재해야 할 핸들러의 구성요소가 있다.

### 메시지(Message)

스레드 통신 시 핸들러를 사용하여 데이터를 보내기 위해선 데이터 종류를 식별할 수 있는 식별자과 실질적인 데이터를 저장한 객체, 그리고 추가 정보를 전달할 객체가 필요하다.

이말인 즉슨 전달할 데이터를 한 곳에 저장하는 역할을 하는 클래스가 필요하다는 것이다.

이 클래스가 바로 Message 클래스이다.

하나의 데이터를 보내기 위해서는 한 개의 Message 인스턴스가 필요하며 데이터들을 담은 Message 객체를 핸들러로 보내면 해당 객체는 핸들러와 연결된 메시지 큐(Message Queue)에 쌓이게 된다.

### 메시지 큐(Message Queue)

메시지 큐(Message Queue)는 이름 그대로 위에서 언급한 Message 객체를 큐(Queue) 형태로 관리하는 자료 구조를 말한다.

큐(Queue)라는 이름답게 FIFO(First In First Out) 방식으로 동작하기 때문에, 메시지는 큐에 들어온 순서에 따라 차례대로 저장된다. (First In)

그리고 가장 먼저 들어온 Message 객체부터 순서대로 처리된다. (First Out)

### 루퍼 (Looper)

아까 전체적인 핸들러의 흐름을 설명할 때 나온 루퍼(Looper)이다.

위에서 언급한 메시지 큐(Message Queue)는 메시지(Message) 객체 리스트를 관리하는 클래스 일 뿐, 큐에 쌓인 메시지 처리를 위한 핸들러를 실행시키지는 않는다.

메시지 루프, 즉 메시지 큐로부터 메시지를 꺼내온 다음에 해당 메시지와 연결된 핸들러로 전달해주는 역할을 수행하는 것이 루퍼(Looper)이다.

참고로 안드로이드 앱의 Main Thread에는 Looper 객체를 사용하여 메시지 루프를 실행하는 코드가 이미 구현되어 있으며 해당 루프 안에서 메시지 큐의 메시지를 꺼내어 처리하도록 만들어져 있다.

그러므로 Main Thread에서 메시지 루프와 관련된 코드를 추가적으로 작성할 필요가 없다.

개발자는 Main Thread로 전달할 Message 객체를 구성하고 쓰레드의 메시지 큐에 연결된 핸들러를 통해 해당 메시지를 전달하기만 하면 된다.


### 핸들러 (Handler)

핸들러(Handler)는 스레드의 루퍼(Looper)와 연결된 메시지 큐로 메시지를 보내고 처리할 수 있게 만들어준다. 

Main Thread의 메시지 처리 흐름에서, 메시지 전달과 처리를 위해 개발자가 접근할 수 있는 창구 역할을 수행한다고 할 수 있다.

쓰레드와 연관된 핸들러를 얻기 위해서는, 간단하게 new 키워드를 사용하여 Handler 클래스 인스턴스를 생성하기만 하면 된다. 

그러면 새로운 Handler 인스턴스는 자동으로 해당 스레드와 메시지 큐에 연결되고, 그 시점부터 핸들러를 통해 메시지를 보내고 처리할 수 있게 된다.


<br/><br/>


## 마무리

---

오늘은 이렇게 Handler의 정의와 흐름에 대해서 간단하게 알아보았다.

참고로 하나의 스레드는 하나의 고유한 Looper와 Message Queue 만을 가질 수 있다. 

그러나 하나의 스레드에서 가질 수 있는 Handler 의 숫자는 제약이 없다.

Main thread 는 Looper를 이미 가지고 있어서 개발자가 관리하지 않아도 되지만, worker thread 에서는 Looper 를 직접 작성하고 실행시켜야 한다.

아래는 Handler를 여러개 사용하는 예시이다.

```java
private final Handler mConnectHandler = new Handler() {
  @Override
  public void handleMessage(Message msg) {
    // mConnectHandler msg
  }
}

private final Handler mDeviceHandler = new Handler() {
  @Override
  public void handleMessage(Message msg) {
    // mDeviceHandler msg
  }
}
```



이 외에도 Handler를 사용하는 다양한 방법이 있다.

본인은 주로 Worker Thread에서 UI 작업이 필요할 때 Handler를 사용하여 Runnable 객체를 전달하며 UI 작업 내용을 Override된 run 함수안에 구현하는 방식으로 사용하였다.

안드로이드 개발을 진행하며 동작들이 어떠한 흐름의 방식으로 진행되는지 이해하며 개발하면 훨씬 더 도움이 될 것이다.


