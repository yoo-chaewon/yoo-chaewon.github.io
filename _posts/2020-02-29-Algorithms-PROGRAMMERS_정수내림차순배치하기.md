---

title:  "[ALGORITHM]PROGRAMMERS/정수내림차순배치하기"

excerpt: "#PROGRAMMERS"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

##### 제한 조건

- `n`은 1이상 8000000000 이하인 자연수입니다.

##### 입출력 예

| n      | return |
| ------ | :----: |
| 118372 | 873211 |



## 코드

```java
import java.util.Arrays;
class Solution {
  public long solution(long n) {
    long answer = 0;
    String str = String.valueOf(n);
    int[] arr = new int[str.length()];
    for (int i = 0; i < str.length(); i++){
      arr[i] = str.charAt(i)-'0';
    }
    Arrays.sort(arr);
    for (int i = arr.length-1 ; i >= 0; i--){
      answer *= 10;
      answer += arr[i];
    }
    return answer;
  }
}
```

- 먼저 문자열의 숫자들을 배열에 넣어주었다 👉 숫자들을 정렬해주기 위해

- 다음 배열을 Arrays.sort를 이용하여 정렬해주었다.

- 다음 answer에 *10씩 해가면서 가장 큰수부터 더해 갔다.

  > 즉, 배열에 873211이 있으면
  >
  > 0*10 + 8 = 8
  >
  > 8*10 + 7 = 87
  >
  > 87 * 10 + 3 = 873

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/12933
>
