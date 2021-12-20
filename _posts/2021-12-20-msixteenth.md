---
layout: single
title:  "모바일앱 개발일기 #16 Content Provider"

---

#### 2021-12-20

오늘은 콘텐츠 프로바이더(Content Provider)에 대해서 알아보도록 할 것이다.

콘텐츠 프로바이더는 앱 간의 데이터 공유를 목적으로 사용되는 컴포넌트이다.

예시를 보며 보도록 하자.

<br/><br/>

### 콘텐츠 프로바이더 구조

---





![image](https://user-images.githubusercontent.com/69960282/146735364-aea707e8-60d8-457d-b822-fdbe826e214f.png)

먼저 A라는 앱과 B라는 앱이 있다고 가정하자.

B라는 앱은 파일 데이터, 데이터베이스, Preference 등 여러 가지의 데이터를 가지고 있다.

이 데이터를 A 앱에서 접근할 수 있을까?

물론 없다.



파일 데이터가 외장 메모리 공간에 저장되어 있다면 가능하겠지만 여기서 말하는 데이터는 내장 메모리 공간에 저장된 데이터이므로 접근이 불가능하다.



하지만 콘텐츠 프로바이더를 이용하면 접근이 가능하다.

<br/><br/>![image](https://user-images.githubusercontent.com/69960282/146735314-1af0fcd9-7c0d-4c56-a16e-86cc58d98f47.png)

A앱에서 B앱의 데이터를 이용하려면 우선 데이터를 가지고 있는 B앱의 개발자가 콘텐츠 프로바이더를 만들어 주어야 한다. 

즉 자신의 앱에 콘텐츠 프로바이더를 만든다는 것은 자신의 데이터를 일정 정도 외부 앱에 오픈하겠다는 의미가 된다.

그리고 외부 앱인 A앱은 B앱의 데이터에 직접 접근하는 것이 아니라 B앱에서 만들어준 콘텐츠 프로바이더 함수를 이용하여 데이터를 이용하는 구조이다.



그래서 데이터에 접근할 때 외부에서 직접적으로 접근하는 것이 아닌 콘텐츠 프로바이더 함수를 통해서 접근하는 방식이기 때문에 B앱의 데이터에 대한 이용을 콘텐츠 프로바이더를 통해서 얼마든지 제어할 수 있다.

<br/><br/>

### 콘텐츠 프로바이더 작성법

---



콘텐츠 프로바이더를 작성하는 방법은 안드로이드 DBMS 프로그램과 유사하다.

콘텐츠 프로바이더 내부에서 접근하는 데이터는 파일, 데이터베이스, Preference 혹은 메리의 데이터일수도 있다.

```java
public class MyContentProvider extends ContentProvider {
    public MyContentProvider() {
    }
    @Override
    public int delete(Uri uri, String selection, String[] selectionArgs) {
        return 0;
    }
    @Override
    public String getType(Uri uri) {
        throw new UnsupportedOperationException("Not yet implemented");
    }
    @Override
    public boolean onCreated() {
        return false;
    }
    @Override
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
        return null;
    }
    @Override
    public int update(Uri uri, ContentValues, String selection, String[] selectionArgs) {
        return 0;
    }
}
```

콘텐츠 프로바이더는 ContentProvider 클래스를 상속받아 작성하며 위의 모든 함수를 재정의해야 한다.

콘텐츠 프로바이더의 생명주기 함수는 onCreate() 함수 하나만 있으며 최초에 한번만 호출된다.

그리고 나머지 함수는 외부 앱에서 필요할 때 호출된다.

이곳에서 적절한 데이터 획득, 저장, 수정, 삭제 작업을 하면 된다.

<br/><br/>

콘텐츠 프로바이더를 만들었다면 콘텐츠 프로바이더도 안드로이드 컴포넌트이기에 Manifest에 등록해서 사용해야 한다. 

근데 이 때 콘텐츠 프로바이더는 <provider>태그로 등록하는데  name 이외에 authorities라는 속성도 반드시 정의해주어야 한다. 

```xml
<Provider
          android:name=".MyContentProvider"
          android:authorities="com.example.test.Provider"
          android:enable="true"
          android:exported="true"></Provider>
```

authorities 속성값은 개발자 임의의 문자열이지만 반드시 유일해야 하며 다른 앱의 콘텐츠 프로바이더와 같은 값이 대입되면 앱 자체가 사용자 스마트폰에 설치되지 않는다.

<br/><br/>

### 콘텐츠 프로바이더 이용

---

콘텐츠 프로바이더는 안드로이드 컴포넌트이지만 intent로 실행하지 않는다.

안드로이드 컴포넌트 중 인텐트로 실행하는 컴포넌트는 액티비티와 서비스 그리고 브로드캐스트 리시버이며 콘텐츠 프로바이더는 인텐트와 전혀 상관이 없다.

도대체 같은 컴포넌트인데 왜 콘텐츠 프로바이더만 왜 인텐트로 실행하지 않는 것일까?

그 이유는 콘텐츠 프로바이더의 독특한 생명주기 때문이다.

<br/><br/>

액티비티와 서비스, 브로드캐스트 리시버는 실제 객체가 생성되는 시점이 어디선가 해당 컴포넌트를 이용하기 위해 intent를 발생시키는 순간이지만 콘텐츠 프로바이더는 시스템에서 인지하는 순간 이용하지 않더라도 미리 생성해 놓는다.

따라서 스마트폰이 부팅되면 여러 앱의 모든 콘텐츠 프로바이더가 생성된다.

<br/><br/>

외부 앱의 콘텐츠 프로바이더를 이용하는 곳에서는 이렇게 시스템에서 미리 생성해놓은 콘텐츠 프로바이더 객체를 ContentResolver를 이용해 획득하여 사용하면 된다.

ContentResolver는 콘텐츠 프로바이더로 생성된 객체들을 담고 있는 관리자 역할의 클래스로 이해하면 된다.

ContentResolver에는 모든 앱의 모든 콘텐츠 프로바이더가 등록되어 있고 이용하고자 하는 객체를 식별하기 위해 Uri 객체를 이용한다.

콘텐츠 프로바이더를 식별하기 위해 사용되는 URL은 규칙이 있다.

```
content:// com.example.test.Provider
```

URL의 프로토콜명은 content를 이용하며 host부분이 이용하고자 하는 콘텐츠 프로바이더의 식별자가 된다.

![image](https://user-images.githubusercontent.com/69960282/146735405-45921164-c113-46c1-9384-2bcb90839faa.png)

그리고 con.example.test.Provider 문자열은 콘텐츠 프로바이더가 Manifest에 등록될때 <provider>태그의 authorities 속성값이다.

Scheme는 content로 고정하고 Scheme와 host 부분은 필수적으로 넣어주어야 한다.
