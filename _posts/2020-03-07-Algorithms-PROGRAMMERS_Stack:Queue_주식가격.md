---

title:  "[ALGORITHM]PROGRAMMERS/Stack/Queue-주식가격"

excerpt: "#PROGRAMMERS #Stack #Queue"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Stack/Queue

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

##### 입출력 예

| prices          | return          |
| --------------- | --------------- |
| [1, 2, 3, 2, 3] | [4, 3, 1, 1, 0] |

##### 입출력 예 설명

- 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
- 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
- 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
- 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
- 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.



## 코드

```java
import java.util.*;
class Solution {
    public int[] solution(int[] prices) {
        ArrayList<Integer> result = new ArrayList<>();

        for (int i = 0; i < prices.length-1; i++){
            int count = 0;
            for (int j = i+1; j <prices.length; j++){
                count++;
                if (prices[i] > prices[j]) break;
            }
            result.add(count);
        }
        result.add(0);

        int[] answer = new int[result.size()];
        for (int i = 0; i < result.size(); i++){
            answer[i] = result.get(i);
        }
        return answer;
    }
}
```

- `탑` 이랑 거의 비슷한 문제였다.
- 왜 스택이랑 큐로 분류되어 있는지 모르겠다. 👉 스터디 시간에 친구들과 이야기 나눠봐야겠다 ~

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42584
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Stack:Queue_주식가격.java
>
