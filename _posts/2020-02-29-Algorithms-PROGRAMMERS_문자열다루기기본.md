---

title:  "[ALGORITHM]PROGRAMMERS/문자열다루기기본"

excerpt: "#PROGRAMMERS #문자열"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- 문자열

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요. 예를 들어 s가 a234이면 False를 리턴하고 1234라면 True를 리턴하면 됩니다.

##### 제한 사항

- `s`는 길이 1 이상, 길이 8 이하인 문자열입니다.

##### 입출력 예

| s    | return |
| ---- | ------ |
| a234 | false  |
| 1234 | true   |

## 코드

```java
import java.util.*;
class Solution {
  public boolean solution(String s) {
      boolean answer = true;
      
      if(s.length() != 4 && s.length() != 6) return false;
      for(int i = 0; i < s.length(); i++ ){
          if(!('0' <= s.charAt(i) && s.charAt(i) <= '9')) return false;
      }
      
      return answer;
  }
}
```

- 먼저 4혹은 6인 문자열이 아닌 것은 false 해주었다.
- 그리고 문자열 전체를 보면서 숫자가 아닌 것이 있으면 false 해주었다.
- 나머지는 true가 될것이다.

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/12918
>
