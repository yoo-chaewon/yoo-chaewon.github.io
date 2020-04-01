---

title:  "[ALGORITHM]BOJ1197_최소스패닝트리"

excerpt: "#BOJ #MST #최소신장트리"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- MST

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



⭐️ 이 구현 방법은 그냥 외워놓자 ~ ⭐️

## 문제

그래프가 주어졌을 때, 그 그래프의 최소 스패닝 트리를 구하는 프로그램을 작성하시오.

최소 스패닝 트리는, 주어진 그래프의 모든 정점들을 연결하는 부분 그래프 중에서 그 가중치의 합이 최소인 트리를 말한다.

### 입력

첫째 줄에 정점의 개수 V(1 ≤ V ≤ 10,000)와 간선의 개수 E(1 ≤ E ≤ 100,000)가 주어진다. 다음 E개의 줄에는 각 간선에 대한 정보를 나타내는 세 정수 A, B, C가 주어진다. 이는 A번 정점과 B번 정점이 가중치 C인 간선으로 연결되어 있다는 의미이다. C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않는다.

그래프의 정점은 1번부터 N번까지 번호가 매겨져 있다. 최소 스패닝 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어진다.

### 출력

첫째 줄에 최소 스패닝 트리의 가중치를 출력한다.



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Comparator;
import java.util.PriorityQueue;

public class Main {
    static int[] parent;
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] VE = bufferedReader.readLine().split(" ");
        int V = Integer.parseInt(VE[0]);
        int E = Integer.parseInt(VE[1]);
        parent = new int[V+1];
        for (int i = 1; i <= V; i++ ) parent[i] = i;

        PriorityQueue<Edge> queue = new PriorityQueue<>(new Comparator<Edge>() {
            @Override
            public int compare(Edge o1, Edge o2) {
                return o1.cost-o2.cost;
            }
        });

        while (E-- > 0 ) {
            String[] input = bufferedReader.readLine().split(" ");
            queue.add(new Edge(Integer.parseInt(input[0]), Integer.parseInt(input[1]), Integer.parseInt(input[2])));
        }

        int sum = 0;
        while (!queue.isEmpty()){
            Edge cur = queue.poll();
            if (!isSameParent(cur.v1, cur.v2)){
                sum += cur.cost;
                union(cur.v1, cur.v2);
            }
        }
        System.out.println(sum);
    }

    public static void union(int x, int y){
        x = findParent(x);
        y = findParent(y);

        if (x != y){
            parent[y] = x;
        }
    }

    public static int findParent(int x){
        if (x == parent[x]) return x;
        else return findParent(parent[x]);
    }

    public static boolean isSameParent(int x, int y){
        if (findParent(x) == findParent(y)) return true;
        else return false;
    }
}

class Edge  {
    int v1;
    int v2;
    int cost;
    Edge(int v1, int v2, int cost) {
        this.v1 = v1;
        this.v2 = v2;
        this.cost = cost;
    }
}
```

### 크루스칼 알고리즘

- 최소 비용 신장 트리를 찾는 알고리즘
- **가장 적은 비용으로 모든 노드를 연결하기 위해 사용하는 알고리즘**
- 최소 스패닝 트리(MST, Minimum spanning Tree)를 찾음으로서 간선의 가중치의 합이 최솟값이 되도록 하는 알고리즘이라고도 할 수 있다.
  - 스패닝 트리: 그래프에서 일부 간선을 선택해서 만든 트리
  - 최소 스패닝 트리: 스패닝 트리 중에 선택한 간선의 가중치의 합이 최소인 트리
- 변의 개수 E(간선), 꼭지점의 개수 V(노드)라고 하면 이 알고리즘은 O(Elong V)의 시간복잡도를 갖는다.
- 유니온 파인드 개념이 구현하는데 사용된다.

#### 순서

> 1. 모든 간선들을 거리(비용, 가중치)를 기준으로 오름차순 정렬한다.
> 2. 정렬된간선을 순서대로 선택한다.
> 3. 선택한 간선의 두 정점이 연결되어 있지 않으면, 해당 두 정점을 연결시킨다.



### 👆저 방법대로 풀었다.

- 우선 모든 간선들을 가중치를 기준으로 오름차순을 정렬하였다.

  - 이를 위해서 간선들을 저장할 class를 만들고 cost를 기준으로 오름차순 정렬을 위해 `PriorityQueue`를 사용하였다.

    ```java
    class Edge  {
        int v1;
        int v2;
        int cost;
        Edge(int v1, int v2, int cost) {
            this.v1 = v1;
            this.v2 = v2;
            this.cost = cost;
        }
    }
    ```

    ```java
    PriorityQueue<Edge> queue = new PriorityQueue<>(new Comparator<Edge>() {
      @Override
      public int compare(Edge o1, Edge o2) {
        return o1.cost-o2.cost;
      }
    });
    
    while (E-- > 0 ) {
      String[] input = bufferedReader.readLine().split(" ");
      queue.add(new Edge(Integer.parseInt(input[0]), Integer.parseInt(input[1]), Integer.parseInt(input[2])));
    }
    ```

- 이후 priorityQueue를 보면서 두 연결되지 않은 노드를 연결해 준다.

  ```java
  int sum = 0;
  while (!queue.isEmpty()){
    Edge cur = queue.poll();
    if (!isSameParent(cur.v1, cur.v2)){
      sum += cur.cost;
      union(cur.v1, cur.v2);
    }
  }
  ```

  1. 이때 두 노드가 연결 되어있는지는 `isSameParent` 부모님이 같니 ? 함수를 사용한다.

     ```java
     public static boolean isSameParent(int x, int y){
       if (findParent(x) == findParent(y)) return true;
       else return false;
     }
     ```

     - 부모님이 같은지 확인하기 위해서는 부모님을 찾아야 한다.

       `findParnet` 함수를 통해 부모님을 찾는다.

       ```java
       public static int findParent(int x){
         if (x == parent[x]) return x;
         else return findParent(parent[x]);
       }
       
       //처음 부모 초기화는 본인으로 해놨을 것인다 ~~
       for (int i = 1; i <= V; i++ ) parent[i] = i;
       ```

  2. 부모님이 같지 않을 경우 두 노드를 연결해 준다.

     동시에 sum에 간선의 값을 연결해 준다.

     연결은 `union` 을 갖고 한다.

     ```java
     public static void union(int x, int y){
       x = findParent(x);
       y = findParent(y);
       if (x != y){
         parent[y] = x;
       }
     }
     ```

     연결을 해주면 이 둘은 부모가 같아지는 것이다 !!



##### 느낀점

아무쪼록 많이 쓰이는 알고리즘이니까 자구 구현해봅시다~~~

> 문제 : https://www.acmicpc.net/problem/1197
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1197_최소스패닝트리.java

