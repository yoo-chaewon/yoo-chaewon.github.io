---

title:  "[ALGORITHM]PROGRAMMERS/Stack/Queue-기능개발"

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

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

##### 제한 사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

##### 입출력 예

| progresses | speeds   | return |
| ---------- | -------- | ------ |
| [93,30,55] | [1,30,5] | [2,1]  |

##### 입출력 예 설명

첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다. 

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.



## 코드

```java
import java.util.*;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        ArrayList<Integer> result = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < progresses.length; i++){
            int day = (100-progresses[i]) / speeds[i];
            if ((100-progresses[i]) % speeds[i] > 0) day++;
            queue.add(day);
        }

        int count = 1;
        int cur = queue.poll();
        while (true){
            if (cur >= queue.peek()){
                count++;
                queue.poll();
            }else {
                result.add(count);
                count = 1;
                cur = queue.poll();
            }

            if (queue.isEmpty()){
                result.add(count);
                break;
            }
        }
        int[] answer = new int[result.size()];
        for (int i = 0; i < result.size(); i++) answer[i] = result.get(i);

        return answer;
    }
}
```

- 우선 나는 완료될때까지 필요한 시간을 큐에 넣어주었다.

  > - **progresses**: [93,30,55]
  > - **speeds** : [1,30,5]
  >
  > 라면, (100-97)/1 = 7 , (100-30)/30 + 1 = 3, (100-45)/5 =  9

  ```java
   for (int i = 0; i < progresses.length; i++){
     int day = (100-progresses[i]) / speeds[i];
     if ((100-progresses[i]) % speeds[i] > 0) day++;
     queue.add(day);
   }
  ```

  ‼️ 이때 2.5일. 걸리면 3일로 해줘야 하므로, `(100-progresses[i]) % speeds[i] > 0` 나머지가 있을 경우의 조건을 추가해주었다.

- 그리고, 큐에서 하나씩 빼면서 현재 max값과 비교해준다.

  > max보다 작으면 그냥 count+1해주고, max보다 큰 값이 나오는 경우 결과를 저장해준다.

  ```java
  while (true){
    if (cur >= queue.peek()){//다음꺼가 작거나 같은 경우
      count++;
      queue.poll();//그냥 빼버린다~
    }else {//큰 경우, 이제 새로 시작해야한다.
      result.add(count);
      count = 1;
      cur = queue.poll();
    }
  
    if (queue.isEmpty()){
      result.add(count);
      break;
    }
  }
  ```

  

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42586
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Stack:Queue_기능개발.java
>
