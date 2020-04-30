---

title:  "[ALGORITHM]KAKAO_2019 개발자 겨울 인턴십-호텔방배정"

excerpt: "#PROGRAMMERS #SW #KAKAO"



categories:

- ALGORITHM

tags:

- ALGORITHM

- KAKAO

- PROGRAMMERS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

스노우타운에서 호텔을 운영하고 있는 스카피는 호텔에 투숙하려는 고객들에게 방을 배정하려 합니다. 호텔에는 방이 총 k개 있으며, 각각의 방은 1번부터 k번까지 번호로 구분하고 있습니다. 처음에는 모든 방이 비어 있으며 스카피는 다음과 같은 규칙에 따라 고객에게 방을 배정하려고 합니다.

1. 한 번에 한 명씩 신청한 순서대로 방을 배정합니다.
2. 고객은 투숙하기 원하는 방 번호를 제출합니다.
3. 고객이 원하는 방이 비어 있다면 즉시 배정합니다.
4. 고객이 원하는 방이 이미 배정되어 있으면 원하는 방보다 번호가 크면서 비어있는 방 중 가장 번호가 작은 방을 배정합니다.

예를 들어, 방이 총 10개이고, 고객들이 원하는 방 번호가 순서대로 [1, 3, 4, 1, 3, 1] 일 경우 다음과 같이 방을 배정받게 됩니다.

| 원하는 방 번호 | 배정된 방 번호 |
| -------------- | -------------- |
| 1              | 1              |
| 3              | 3              |
| 4              | 4              |
| 1              | 2              |
| 3              | 5              |
| 1              | 6              |

전체 방 개수 k와 고객들이 원하는 방 번호가 순서대로 들어있는 배열 room_number가 매개변수로 주어질 때, 각 고객에게 배정되는 방 번호를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

#### **[제한사항]**

- k는 1 이상 1012 이하인 자연수입니다.
- room_number 배열의 크기는 1 이상 200,000 이하입니다.
- room_number 배열 각 원소들의 값은 1 이상 k 이하인 자연수입니다.
- room_number 배열은 모든 고객이 방을 배정받을 수 있는 경우만 입력으로 주어집니다.
  - 예를 들어, k = 5, room_number = [5, 5] 와 같은 경우는 방을 배정받지 못하는 고객이 발생하므로 이런 경우는 입력으로 주어지지 않습니다.

------

##### **[입출력 예]**

| k    | room_number   | result        |
| ---- | ------------- | ------------- |
| 10   | [1,3,4,1,3,1] | [1,3,4,2,5,6] |

##### **입출력 예에 대한 설명**

**입출력 예 #1**

문제의 예시와 같습니다.

첫 번째 ~ 세 번째 고객까지는 원하는 방이 비어 있으므로 즉시 배정받을 수 있습니다. 네 번째 고객의 경우 1번 방을 배정받기를 원했는데, 1번 방은 빈 방이 아니므로, 1번 보다 번호가 크고 비어 있는 방 중에서 가장 번호가 작은 방을 배정해야 합니다. 1번 보다 번호가 크면서 비어있는 방은 [2번, 5번, 6번...] 방이며, 이중 가장 번호가 작은 방은 2번 방입니다. 따라서 네 번째 고객은 2번 방을 배정받습니다. 마찬가지로 5, 6번째 고객은 각각 5번, 6번 방을 배정받게 됩니다.



## 나의 코드

```java
import java.util.*;
public class Solution {
    public long[] solution(long k, long[] room_number) {
        LinkedHashSet<Long> result = new LinkedHashSet<>();//중복이 안되는데, 순서가 필요하므로 LinkedHashSet사용
        for (long number : room_number){
            if (!result.contains(number)){//예약 안되어 있는 방이면 예약
                result.add(number);
            }else {
                for (long i = number+1; i < k; i++){//원하는 방보다 큰 방, 비어있는 것 찾아서.
                    if (!result.contains(i)){
                        result.add(i);
                        break;
                    }
                }
            }
        }
        long[] answer = new long[room_number.length];
        int cur = 0;
        for (long key : result){
            answer[cur++] = key;
            if (cur == room_number.length) break;
        }
        return answer;
    }
}
```

- 어렵지 않게 풀었다. 구현은 한 15분? 근데 효율성에서 점수가 0점이다.
- 가장 의심 가는 부분은 `고객이 원하는 방이 이미 배정되어 있으면 원하는 방보다 번호가 크면서 비어있는 방 중 가장 번호가 작은 방을 배정합니다` 부분이다.
- 일단 위 코드의 설명은 주석으로 달아놨다.



```java
import java.util.*;
public class Solution {
    static HashMap<Long, Long> room_map = new HashMap<>();
    public static long[] solution(long k, long[] room_number) {
        long[] answer = new long[room_number.length];
        for (int i = 0; i < room_number.length; i++){
            answer[i] = getParent(room_number[i]);
        }
        return answer;
    }

    public static long getParent(long room){
        if (!room_map.containsKey(room)){//기존에 방이 존재하지 않을 경우,
            room_map.put(room, room+1);//만약 이 room번호가 입력 받으면 room+1을 준다.
            return room;//그 방 준다.
        }else {//이미 그 방이 존재할 경우,
            room_map.put(room, getParent(room_map.get(room)));//타고 타고 올라가는 것이다.
            return room_map.get(room);
        }
    }
}
```

- 다른 사람의 해설을 보고 풀었다. 생각했던 데로 방을 추천해주는 곳에서 효율성이 있었다.

- 놀랍게도 Union-Find 알고리즘을 사용하는 것이다. 몇번을 풀었던 알고리즘인데, 이런데서 활용되는 신기했다...그리고,,,,,,흑 슬퍼졌다.

  > 다만 지금 했던거와 달랐던 것은, 지금까지 Union-Find는 배열을 만들어줘서 했는데, 이 경우에 10^12사이즈의배열을 만들지 못해서 HashMap으로 해주었다.
  >
  > 또한, 기존은 부모를 찾는 것이라면, 이것은 바로 오른쪽 값을 찾는다는 점에서 다르다.

- 기존 Union-Find

  ```java
  int root[MAX_SIZE];
  for (int i = 0; i < MAX_SIZE; i++)
      parent[i] = i;
  
  int find(int x) {
      if (root[x] == x) {
          return x;
      } else {
          return find(root[x]);
      }
  }
  
  void union(int x, int y){
      x = find(x);
      y = find(y);
      root[y] = x;
  }
  ```

  HashMap을 이용한 Union-Find

  ```java
  public static long getParent(long room){
    if (!room_map.containsKey(room)){
      room_map.put(room, room+1);
      return room;
    }else {
      room_map.put(room, getParent(room_map.get(room)));
      return room_map.get(room);
    }
  }
  ```

  

### 🔗 링크

너무 충격적이 풀이 방법이다. 역시 난 왕초보다...잘해내고 싶다ㅠㅠ

- 문제: https://programmers.co.kr/learn/courses/30/lessons/64063
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/4.KAKAO/Q_호텔방배정.java