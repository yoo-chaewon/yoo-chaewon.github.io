---

title:  "[ALGORITHM]PROGRAMMERS/Hash-전화번호목록"

excerpt: "#PROGRAMMERS #Hash"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Hash

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
- 각 전화번호의 길이는 1 이상 20 이하입니다.

##### 입출력 예제

| phone_book                  | return |
| --------------------------- | ------ |
| [119, 97674223, 1195524421] | false  |
| [123,456,789]               | true   |
| [12,123,1235,567,88]        | false  |

##### 입출력 예 설명

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

## 코드

```java
import java.util.*;
class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;

        Arrays.sort(phone_book);

        for (int i = 0 ; i < phone_book.length-1; i++){
            int length = Math.min(phone_book[i].length() , phone_book[i+1].length());
            if (phone_book[i].equals(phone_book[i+1].substring(0, length))
            || phone_book[i+1].equals(phone_book[i].substring(0, length))){
                return false;
            }
        }
        return answer;
    }
}
```

- 2중 for문을 돌면서 모두 비교할수도 있지만, 이것은 시간효율성에서 별로일것이라고 생각이 들었다.

- 그래서 문자열 기준 정렬을 하고, 현재꺼와 그 다음꺼만 비교를 하였다.

  > 비교를 할 때 문자열 길이는 더 짧은 것으로 해주었다.

  ```java
  int length = Math.min(phone_book[i].length() , phone_book[i+1].length());
  ```

- 문자열 비교하는 부분이다.

  ```java
  for (int i = 0 ; i < phone_book.length-1; i++){
    int length = Math.min(phone_book[i].length() , phone_book[i+1].length());
    if (phone_book[i].equals(phone_book[i+1].substring(0, length))
              || phone_book[i+1].equals(phone_book[i].substring(0, length))){
      return false;
    }
  }
  ```

  

##### 느낀점

> 코로나 조심😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42577
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Hash_전화번호목록.java
>
