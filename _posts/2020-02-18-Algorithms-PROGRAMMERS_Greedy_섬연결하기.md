---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-섬연결하기"

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

n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

**제한사항**

- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 `((n-1) * n) / 2`이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

**입출력 예**

| n    | costs                                     | return |
| ---- | ----------------------------------------- | ------ |
| 4    | [[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]] | 4      |

**입출력 예 설명**

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.

![image.png](https://grepp-programmers.s3.amazonaws.com/files/production/13e2952057/f2746a8c-527c-4451-9a73-42129911fe17.png)

## 코드

```java
import java.util.Arrays;

class Solution {
    public static int[] parent;
    public static int[] level;
    public static int solution(int n, int[][] costs) {
        int answer = 0;
        parent = new int[n];
        level = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;

        Arrays.sort(costs, ((o1, o2) -> Integer.compare(o1[2],o2[2])));

        for (int i = 0; i < costs.length; i++){
            int a = costs[i][0];
            int b = costs[i][1];

            if (findParent(a) != findParent(b)){
                answer += costs[i][2];
                Merge(a, b);
            }
        }
        return answer;
    }

    public static void Merge(int a, int b){
        a = findParent(a);
        b = findParent(b);
        if (a == b) return;
        if (level[a] < level[b]){
            int tmp = a;
            a = b;
            b = tmp;
        }
        parent[b] = a;
        if (level[a] == level[b]) level[a]++;
    }

    public static int findParent(int a){
        if (parent[a] == a) return a;
        return parent[a] = findParent(parent[a]);
    }
}
```

- **디스조인트-셋** 문제와 아주 비슷하다.

- 처음에는 정렬을 해준 후 visited만 체크를 해주었는데, 1-2 3-4인 경우 2-3인 경우를 계산해주지 않고 끝나는 경우가 있었다. 그래서 트리 형식으로 생각을 하였다.

- 우선 cost값으로 정렬을 해주었다.

  ```java
  Arrays.sort(costs, ((o1, o2) -> Integer.compare(o1[2],o2[2])));
  ```

- 그 다음 간선 두개가 이미 연결되어 있지 않은 경우, cost를 더해주고, 연결을 했다.

  ```java
  if (findParent(a) != findParent(b)){
    answer += costs[i][2];
    Merge(a, b);
  }
  ```

- 두 점간의 연결은

  ```java
  public static void Merge(int a, int b){
    //가장 상위 노드끼리 merge해줘야 하므로 findParent를 하였다.
    a = findParent(a);
    b = findParent(b);
    
    if (a == b) return;
    // 이미 연결되어 있는 경우를 뜻한다. 아마 여기는 들어오는 것은 없을 것이다.if (findParent(a) != findParent(b))이 조건문 때문
    
    if (level[a] < level[b]){
      int tmp = a;
      a = b;
      b = tmp;
    }
    parent[b] = a;
    if (level[a] == level[b]) level[a]++;
  }
  ```



- **람다식 sort** 기억하자!

  ```java
  Arrays.sort(costs, new Comparator<int[]>() {
    @Override
    public int compare(int[] o1, int[] o2) {
      return Integer.compare(o1[2], o2[2]);
    }
  });
  
  //👇
  Arrays.sort(costs, ((o1, o2) -> Integer.compare(o1[2],o2[2])));
  ```

  

##### 느낀점

어느새 순열늪에 빠져서 자꾸 순열부터 짠다. 그리고 시간초과가 와장창 나서 25점 받고,,생각하자 생각하자,,,해서,,,,일단 정렬해보고 그다음 Tree형태를 떠올려 저번에 푼 디스조인트셋처럼 풀었다.. 생각좀 하고 풀자!!!

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42861
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_섬연결하기.java
>

