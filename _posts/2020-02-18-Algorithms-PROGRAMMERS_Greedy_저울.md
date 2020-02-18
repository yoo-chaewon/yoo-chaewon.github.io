---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-저울"

excerpt: "#PROGRAMMERS #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- GREEDY

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

하나의 양팔 저울을 이용하여 물건의 무게를 측정하려고 합니다. 이 저울의 양팔의 끝에는 물건이나 추를 올려놓는 접시가 달려 있고, 양팔의 길이는 같습니다. 또한, 저울의 한쪽에는 저울추들만 놓을 수 있고, 다른 쪽에는 무게를 측정하려는 물건만 올려놓을 수 있습니다.

![image0.png](https://grepp-programmers.s3.amazonaws.com/files/production/f73e61d4de/f4abf5ff-1956-4e49-bd4a-d3d24619bbf0.png)

저울추가 담긴 배열 weight가 매개변수로 주어질 때, 이 추들로 측정할 수 없는 양의 정수 무게 중 최솟값을 return 하도록 solution 함수를 작성해주세요.

예를 들어, 무게가 각각 [3, 1, 6, 2, 7, 30, 1]인 7개의 저울추를 주어졌을 때, 이 추들로 측정할 수 없는 양의 정수 무게 중 최솟값은 21입니다.

##### 제한 사항

- 저울추의 개수는 1개 이상 10,000개 이하입니다.
- 각 추의 무게는 1 이상 1,000,000 이하입니다.

##### 입출력 예

| weight                 | return |
| ---------------------- | ------ |
| [3, 1, 6, 2, 7, 30, 1] | 21     |

##### 입출력 예 설명

문제에 나온 예와 같습니다.



## 코드

```java
import java.util.*;
class Solution {
    public int solution(int[] weight) {
         Arrays.sort(weight);
        int answer = 1;
        for (int i = 0; i < weight.length; i++){
            if (answer < weight[i]) break;
            answer+= weight[i];
        }
        return answer;
    }
}
```

- 먼저 입력된 배열을 **정렬**한다.

  그러면 [1, 1, 2, 3, 6, 7, 30]이 된다.

- 6까지 탐색을 했다치면 1+1+2+3+6=13까지 무게를 잴 수 있다.

  그리고 이 추들로 13까지 모두 만들어 낼 수 있다.

  [1, 2, 2+1, 1+1+2, 2+3, 6, 6+1, 2+6, 3+6, 1+1+2+6,,,,,,,]이런식으로.

- 즉, 따라서 값을 누적한 값이 현재 추보다 작은 경우 그 누적값이 정답이 된다.

> 추들의 무게의 합 + 1 = 추들이 측정할 수 없는 최소 무게



##### 느낀점

저번에 풀었던 문제여서 개쉽게 풀었다. 10초컷은 구라~

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42886
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_저울.java
>

