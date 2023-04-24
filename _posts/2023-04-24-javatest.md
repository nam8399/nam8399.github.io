---
layout: single
title:  "[Java] 프로그래머스 JadenCase 문자열 만들기 - Java"
categories: Java
---

오늘도 자바에서 JaenCase 문자열 만들기라는 코딩테스트 문제를 푼 것에 대해서 정리해보려고 한다.



### 문제 설명

---

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 

단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)

문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.


<br/><br/>
### 제한 사항


---


* s는 길이 1 이상 200 이하인 문자열입니다.

* s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.

  * 숫자는 단어의 첫 문자로만 나옵니다.
  
  * 숫자로만 이루어진 단어는 없습니다.
 
  * 공백문자가 연속해서 나올 수 있습니다.


<br/><br/>
### 입출력 예


---

|s|return|
|-----|---|
|"3people unFollowed me"|"3people Unfollowed Me"|
|"for the last week"|"For The Last Week"|



<br/><br/>
### 나의 풀이

---

```java
class Solution {
    public String solution(String s) {
        String answer = s.toLowerCase();
        boolean space = true;
        
        String[] split = answer.split("");
        String result = "";
        for(String ss : split) {
            result += space ? ss.toUpperCase() : ss;
            space = ss.equals(" ") ? true : false;
        }
        
        return result;
    }
}
```


