---
layout: single
title:  "모바일앱 개발일기 #12 Firebase 데이터 가져오기"

---

#### 2021-09-25

오늘은 파이어베이스에 저장된 데이터를 불러오는 작업을 해보겠다.

불러올때 상황마다 코드가 다른데 한번 보도록 하겠다.

 <br/><br/>

먼저 한개의 데이터만을 가져올 때이다.


```java
databaseReference.child("feed_data").child("status").addValueEventListener(new ValueEventListener() {
                    @Override
                    public void onDataChange(@NonNull DataSnapshot dataSnapshot) {
                        String value = dataSnapshot.getValue(String.class);
                        uname= value;

                    }

                    @Override
                    public void onCancelled(@NonNull DatabaseError databaseError) {
                        //Log.e("MainActivity", String.valueOf(databaseError.toException())); // 에러문 출력
                    }
                });
```

 <br/><br/>

그 다음은 참조되는 부분 아래로 값이 여러개 있을 때이다.

```java
databaseReference.child("feed_data").child(Gname).addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(@NonNull DataSnapshot dataSnapshot) {
                intakegroup group = dataSnapshot.getValue(intakegroup.class);
                
                //각각의 값 받아오기 get어쩌구 함수들은 intakegroup.class에서 지정한것
                intakedata = group.getintakedata();
                intakedate = group.getintakedate();
                
                //텍스트뷰에 받아온 문자열 대입하기
                txt_data.setText(intakedata);
                txt_date_tv.setText(intakedate);
            }

            @Override
            public void onCancelled(@NonNull DatabaseError databaseError) {
                //Log.e("MainActivity", String.valueOf(databaseError.toException())); // 에러문 출력
            }
        });
```

미리 intakegroup.class에서 데이터를 지정해주고 그곳에 알맞게 값을 받아와준다.

그리고 지정한 Textview에 보이게 하는 형식으로 예시를 짜보았다.

 <br/><br/>

이처럼 Firebase에서 개별의 데이터와 여러개의 데이터를 받아오는 것을 해보았다.
