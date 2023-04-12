---
layout: single
title:  "[Android] 안드로이드 Media Player 재생, 일시정지, 정지 - 코틀린"
categories: Android
published: true
---

오늘은 안드로이드에서 음악을 재생하는 방법과 각 재생, 일시정지, 정지 실행 방법에 대해서 알아볼 것이다.

# MediaPlayer

---

음악을 재생하는 작업을 하기 위해서는 MediaPlayer 클래스를 사용하면 된다.

이 클래스를 사용해서 사운드나 동영상을 재생할 수 있다.

## 음악 재생

먼저 res 폴더의 raw 폴더를 생성한 후 등록할 음악 파일을 올려준다.

그 이후에 하단과 같이 작성해주면 된다.

```kotlin
fun playmusic() {
        mediaPlayer = MediaPlayer.create(this, R.raw.myretro_citypop)
        mediaPlayer?.start()
    }
```

생각보다 굉장히 간단하다. 


## 음악 정지

```kotlin
fun stopmusic() {
        mediaPlayer?.stop()
    }
```

음악 정지도  stop 메서드를 통해서 한번에 실행할 수 있다.


## 음악 일시정지

일시정지 같은 경우에는 일시정지 버튼을 누르는 타이밍의 시간을 알아야한다.

해당 타이밍은 currentPosition을 통해서 알 수 있다.

```kotlin
 fun pausemusic() {
        mediaPlayer?.pause()
        position_music = mediaPlayer?.currentPosition
    }
```

position_music이라는 변수에 일시정지 버튼을 클릭하는 순간 currentPosition 값이 저장된다.

해당 position을 이용해서 다시 play 버튼을 눌렀을 때 정지한 위치에서 다시 시작되도록 할 수 있다.


```kotlin
 fun playmusic() {
        mediaPlayer = MediaPlayer.create(this, R.raw.myretro_citypop)
        if (position_music != 0) {
            position_music?.let { mediaPlayer?.seekTo(it) }
        }
        mediaPlayer?.start()
    }
```

위처럼 position_music의 값이 0이 아닐 경우에 seekTo라는 함수를 통해서 아까 정지했던 위치로 다시 갈 수 있도록 하였다.

seekTo는 곡의 특정 위치로 이동할 수 있도록 해준다.

위 메서드를 사용하여 처음부터 시작 등의 여러 가지 기능들을 구현할 수 있다.


위와 같이 재생, 일시정지, 정지의 각각의 메서드들을 동작하고자 하는 버튼과 연결해서 사용하면 된다.

