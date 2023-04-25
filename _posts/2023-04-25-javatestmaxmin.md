---
layout: single
title:  "[Java] 프로그래머스 최댓값과 최솟값 - Java"
categories: Java
---

오늘도 자바에서 최댓값과 최솟값이라는 코딩테스트 문제를 푼 것에 대해서 정리해보려고 한다.



### 문제 설명

---

문자열 s에는 공백으로 구분된 숫자들이 저장되어 있습니다. 

str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 "(최소값) (최대값)"형태의 문자열을 반환하는 함수, solution을 완성하세요.

예를들어 s가 "1 2 3 4"라면 "1 4"를 리턴하고, "-1 -2 -3 -4"라면 "-4 -1"을 리턴하면 됩니다.


<br/><br/>
### 제한 조건


---


* s에는 둘 이상의 정수가 공백으로 구분되어 있습니다.


<br/><br/>
### 입출력 예


---

|s|return|
|-----|---|
|"1 2 3 4"|"1 4"|
|"-1 -2 -3 -4"|"-4 -1"|
|"-1 -1"|"-1 -1"|



<br/><br/>
### 나의 풀이

---

```java
class Solution {
    public String solution(String s) {
        String[] answer = s.split(" ");
        int max = 0;
        int min = 0;
        
        for(int i=0 ; i<answer.length; i++) {
            if (i==0) {
                max = maxCalculate(Integer.parseInt(answer[i]), Integer.parseInt(answer[i+1]));
                min = minCalculate(Integer.parseInt(answer[i]), Integer.parseInt(answer[i+1]));
            } else if(i<answer.length-1) {
                max = maxCalculate(max, Integer.parseInt(answer[i+1]));
                min = minCalculate(min, Integer.parseInt(answer[i+1]));
            }
        }
        
        return min + " " + max;
    }
    
    private int maxCalculate(int a, int b) {
        if(a < b) {
            return b;    
        } else {
            return a;
        }
    }
    
     private int minCalculate(int a, int b) {
        if(a < b) {
            return a;    
        } else {
            return b;
        }
    }
}
```


