---

title:  "[ALGORITHM]PROGRAMMERS/Sort-H-Index"

excerpt: "#PROGRAMMERS #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Sort

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`가 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

##### 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.



## 코드

```java
import java.util.*;
class Solution {
    public int solution(int[] citations) {
         Arrays.sort(citations);
        int answer = 0;
        for (int i = 0; i < citations.length; i++){
            if (citations[i] >= citations.length-i) {
                answer = Math.max(answer , citations.length-i);
            }
        }
        return answer;
    }
}
```

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`가 이 과학자의 H-Index입니다. 👉 여기서 3번 이상 이용된 논문이 5편이상이면, 결과는 5를 return해줘야 한다.

[3, 0, 6, 1, 5]

라 하면 우선 정렬을 해준다. [0, 1, 3, 5, 6]

| 0번이상 | 5    |
| ------- | ---- |
| 1번이상 | 4    |
| 3번이상 | 3    |
| 5번이상 | 2    |
| 6번이상 | 1    |

그러면 이런식이 된다.

> 문제를 읽을 수록 이해가 안간다. 그냥 표보고 코드보고 이해하자

```java
for (int i = 0; i < citations.length; i++){
  if (citations[i] >= citations.length-i) {
    answer = Math.max(answer , citations.length-i);
  }
}
```



##### 느낀점

> 문제자체가 정확하지 않아서 헤맸다. 그거 말고는 쉬운 문제였다. 너무 주어진 예시만 보고 문제를 이해하려기 보다는 문제를 제대로 읽고 이해하도록 해야겠다. 
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42747
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Sort_H-Index.java
>
