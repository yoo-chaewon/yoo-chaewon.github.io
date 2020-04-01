---

title:  "[ALGORITHM]BOJ1922_네트워크연결"

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



## 문제

도현이는 컴퓨터와 컴퓨터를 모두 연결하는 네트워크를 구축하려 한다. 하지만 아쉽게도 허브가 있지 않아 컴퓨터와 컴퓨터를 직접 연결하여야 한다. 그런데 모두가 자료를 공유하기 위해서는 모든 컴퓨터가 연결이 되어 있어야 한다. (a와 b가 연결이 되어 있다는 말은 a에서 b로의 경로가 존재한다는 것을 의미한다. a에서 b를 연결하는 선이 있고, b와 c를 연결하는 선이 있으면 a와 c는 연결이 되어 있다.)

그런데 이왕이면 컴퓨터를 연결하는 비용을 최소로 하여야 컴퓨터를 연결하는 비용 외에 다른 곳에 돈을 더 쓸 수 있을 것이다. 이제 각 컴퓨터를 연결하는데 필요한 비용이 주어졌을 때 모든 컴퓨터를 연결하는데 필요한 최소비용을 출력하라. 모든 컴퓨터를 연결할 수 없는 경우는 없다.

## 입력

첫째 줄에 컴퓨터의 수 N (1 ≤ N ≤ 1000)가 주어진다.

둘째 줄에는 연결할 수 있는 선의 수 M (1 ≤ M ≤ 100,000)가 주어진다.

셋째 줄부터 M+2번째 줄까지 총 M개의 줄에 각 컴퓨터를 연결하는데 드는 비용이 주어진다. 이 비용의 정보는 세 개의 정수로 주어지는데, 만약에 a b c 가 주어져 있다고 하면 a컴퓨터와 b컴퓨터를 연결하는데 비용이 c (1 ≤ c ≤ 10,000) 만큼 든다는 것을 의미한다.

### 출력

모든 컴퓨터를 연결하는데 필요한 최소비용을 첫째 줄에 출력한다.

### 예제 입력 1 

```
6
9
1 2 5
1 3 4
2 3 2
2 4 7
3 4 6
3 5 11
4 5 3
4 6 8
5 6 8
```

### 예제 출력 1

```
23
```

### 힌트

이 경우에 1-3, 2-3, 3-4, 4-5, 4-6을 연결하면 주어진 output이 나오게 된다.



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
        int N = Integer.parseInt(bufferedReader.readLine());
        int M = Integer.parseInt(bufferedReader.readLine());
        parent = new int[N+1];
        for (int i = 1; i <= N ; i++) parent[i] = i;

        PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new Comparator<Edge>() {
            @Override
            public int compare(Edge o1, Edge o2) {
                return o1.cost-o2.cost;
            }
        });

        for (int i = 0; i < M; i++){
            String[] input = bufferedReader.readLine().split(" ");
            priorityQueue.add(new Edge(Integer.parseInt(input[0]), Integer.parseInt(input[1]), Integer.parseInt(input[2])));
        }
        int result = 0;
        while (!priorityQueue.isEmpty()){
            Edge cur = priorityQueue.poll();
            if (!isSameParent(cur.v1, cur.v2)){
                result += cur.cost;
                union(cur.v1, cur.v2);
            }
        }
        System.out.println(result);
    }

    public static void union(int a, int b){
        a = findParent(a);
        b = findParent(b);
        if (a != b){
            parent[a] = b;
        }
    }

    public static boolean isSameParent(int a, int b){
        if (findParent(a) == findParent(b)) return true;
        else return false;
    }

    public static int findParent(int a){
        if (a == parent[a]) return a;
        else return findParent(parent[a]);
    }
}

class Edge{
    int v1;
    int v2;
    int cost;
    Edge(int v1, int v2, int cost){
        this.v1 = v1;
        this.v2 = v2;
        this.cost = cost;
    }
}
```

[최소스패닝트리](https://yoo-chaewon.github.io/algorithm/Algorithms-BOJ1197_최소스패닝트리/)

☝️ 이거랑 걍 싺따!!!똑같은 문제

##### 느낀점

> 문제 : https://www.acmicpc.net/problem/1922
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1922_네트워크연결.java

