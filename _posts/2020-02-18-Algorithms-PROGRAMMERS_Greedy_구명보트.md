---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-구명보트"

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

무인도에 갇힌 사람들을 구명보트를 이용하여 구출하려고 합니다. 구명보트는 작아서 한 번에 최대 **2명**씩 밖에 탈 수 없고, 무게 제한도 있습니다.

예를 들어, 사람들의 몸무게가 [70kg, 50kg, 80kg, 50kg]이고 구명보트의 무게 제한이 100kg이라면 2번째 사람과 4번째 사람은 같이 탈 수 있지만 1번째 사람과 3번째 사람의 무게의 합은 150kg이므로 구명보트의 무게 제한을 초과하여 같이 탈 수 없습니다.

구명보트를 최대한 적게 사용하여 모든 사람을 구출하려고 합니다.

사람들의 몸무게를 담은 배열 people과 구명보트의 무게 제한 limit가 매개변수로 주어질 때, 모든 사람을 구출하기 위해 필요한 구명보트 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
- 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.

##### 입출력 예

| people           | limit | return |
| ---------------- | ----- | ------ |
| [70, 50, 80, 50] | 100   | 3      |
| [70, 80, 50]     | 100   | 3      |



## 코드

```java
import java.util.*;
class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;
        int startIndx = 0;
        int endIndx = people.length-1;

        Arrays.sort(people);

        while (true){
            if (people[startIndx] + people[endIndx] > limit){
                endIndx--;
                answer++;
            }else {
                startIndx++;
                endIndx--;
                answer++;
            }
            if (startIndx > endIndx){
                break;
            }
        }
        return answer;
    }
}
```

- 우선 나는 정렬을 먼저 해주었다.

- 그리고 앞 뒤로 보면서, 이 둘의 합이 limit를 넘는지 확인했다.

  - 합이 limit를 넘을 경우 뒤에 있는 사람은 혼자 타야한다

    > 정렬을 해줘야 했기 때문에 이 사람은 결국 누구와도 같이 타지 못한다.
    >
    > 따라서 answer++을 해주고,
    >
    > endIndex--을 해준다.

  - 합이 limit를 넘지 않는 경우 제일 가벼운 사람과 제일 무거운 사람 같이 탈수 있다.

    > 즉 startIndex 와 endIndex는 같이 탈 수 있다.
    >
    > 따라서 answer++을 해주고,
    >
    > startIndex++, endIndex--해준다.

- startIndex 보다 endIndex가 큰 경우 끝난ㄷ.

- startIndex == endIndex인 경우는, 위에 if else문 둘 중 아무거나 들어가 anwer++이 될 것이고,

  결국 if(start > end)조건문에 걸려서 끝이나게 될 것이다.



##### 느낀점

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42885
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_구명보트.java
>

