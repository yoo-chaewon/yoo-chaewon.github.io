---

title:  "[ALGORITHM]PROGRAMMERS/Stack/Queue-다리를지나는트럭"

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

트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

##### 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |



## 코드

```java
import java.util.*;
class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 0;
        int weight_sum = 0;
        Queue<Integer> truckQueue = new LinkedList<>();
        Queue<Integer> bridge = new LinkedList<>();

        for (int truck : truck_weights) truckQueue.add(truck);
        for (int i = 0 ; i < bridge_length; i++)bridge.add(0);

        while (!bridge.isEmpty()) {
            int cur = bridge.poll();
            answer++;
            weight_sum -= cur;

            if (!truckQueue.isEmpty()) {
                if (weight_sum + truckQueue.peek() > weight) {
                    bridge.add(0);
                } else {
                    weight_sum += truckQueue.peek();
                    bridge.add(truckQueue.poll());
                }
            }
        }
        return answer;
    }
}
```

- 뭔가 변수를 조절해가며 풀 수 있는 문제라고 생각하였다.
- 하지만, `큐` 를 사용해서 풀어보았다 ~



- 먼저 다리에 해당하는 큐를 만들고 그 길이 만큼 0을 넣는다.

  ```java
  Queue<Integer> bridge = new LinkedList<>();
  for (int i = 0 ; i < bridge_length; i++)bridge.add(0);
  ```

- 그리고 기다리고 있는 트럭들 큐를 만들었다.

  ```java
  Queue<Integer> truckQueue = new LinkedList<>();
  for (int truck : truck_weights) truckQueue.add(truck);
  ```

- 그리고 다리큐가 비었을 경우(차가 다 지나감~)끝내준다.

- Weight_sum이라는 변수는 다리 위의 차들의 무게의 총 합이다. 

  - 따라서 차가 하나 빠져나가면 -해주고, 차가 들어가면 +해준다.

- ```java
  while (!bridge.isEmpty()) {
    int cur = bridge.poll();//매번 맨 앞에 있는것을 뺀다
    answer++;//시간초 +1
    weight_sum -= cur;//다리에 있는 자동차 무게 합에서 현재 뺀자동차 값을 뺀다 (0일수도 있으니 상관X)
    
    if (!truckQueue.isEmpty()) {//대기 트럭이 존재할 경우 ~
      if (weight_sum + truckQueue.peek() > weight) {//대기에 있는 차 맨앞것이 다리에 들어갔을 때 무게제한을 넘으면 차가 들어가면 안된다 !
        bridge.add(0);
      } else {//차가 들어가도 되는 경우
        weight_sum += truckQueue.peek();//차가 다리에 들어가니까 자동차 무게 합에 + 해주고
        bridge.add(truckQueue.poll());//다리에 넣는다 !
      }
    }
  }
  ```

  



##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42583
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Stack:Queue_다리를지나는트럭.java
>
