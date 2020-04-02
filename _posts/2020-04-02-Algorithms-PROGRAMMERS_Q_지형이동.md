---

title:  "[ALGORITHM]PROGRAMMERS-Q_지형이동"

excerpt: "#PROGRAMMERS #Summer/Winter Coding"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

N x N 크기인 정사각 격자 형태의 지형이 있습니다. 각 격자 칸은 1 x 1 크기이며, 숫자가 하나씩 적혀있습니다. 격자 칸에 적힌 숫자는 그 칸의 높이를 나타냅니다. 

이 지형의 아무 칸에서나 출발해 모든 칸을 방문하는 탐험을 떠나려 합니다. 칸을 이동할 때는 상, 하, 좌, 우로 한 칸씩 이동할 수 있는데, 현재 칸과 이동하려는 칸의 높이 차가 height 이하여야 합니다. 높이 차가 height 보다 많이 나는 경우에는 사다리를 설치해서 이동할 수 있습니다. 이때, 사다리를 설치하는데 두 격자 칸의 높이차만큼 비용이 듭니다. 따라서, 최대한 적은 비용이 들도록 사다리를 설치해서 모든 칸으로 이동 가능하도록 해야 합니다. 설치할 수 있는 사다리 개수에 제한은 없으며, 설치한 사다리는 철거하지 않습니다.

각 격자칸의 높이가 담긴 2차원 배열 land와 이동 가능한 최대 높이차 height가 매개변수로 주어질 때, 모든 칸을 방문하기 위해 필요한 사다리 설치 비용의 최솟값을 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- land는 N x N크기인 2차원 배열입니다. 
- land의 최소 크기는 4 x 4, 최대 크기는 300 x 300입니다. 
- land의 원소는 각 격자 칸의 높이를 나타냅니다. 
- 격자 칸의 높이는 1 이상 10,000 이하인 자연수입니다. 
- height는 1 이상 10,000 이하인 자연수입니다.

------

##### 입출력 예

| land                                                         | height | result |
| ------------------------------------------------------------ | ------ | ------ |
| [[1, 4, 8, 10], [5, 5, 5, 5], [10, 10, 10, 10], [10, 10, 10, 20]] | 3      | 15     |
| [[10, 11, 10, 11], [2, 21, 20, 10], [1, 20, 21, 11], [2, 1, 2, 1]] | 1      | 18     |

##### 입출력 예 설명

입출력 예 #1

각 칸의 높이는 다음과 같으며, 높이차가 3 이하인 경우 사다리 없이 이동이 가능합니다.

![land_ladder_5.png](https://grepp-programmers.s3.amazonaws.com/files/production/c08b7af3db/5efe34cb-1e69-4474-8e0f-b6929184ebdd.png)

위 그림에서 사다리를 이용하지 않고 이동 가능한 범위는 같은 색으로 칠해져 있습니다. 예를 들어 (1행 2열) 높이 4인 칸에서 (1행 3열) 높이 8인 칸으로 직접 이동할 수는 없지만, 높이가 5인 칸을 이용하면 사다리를 사용하지 않고 이동할 수 있습니다. 

따라서 다음과 같이 사다리 두 개만 설치하면 모든 칸을 방문할 수 있고 최소 비용은 15가 됩니다.

- 높이 5인 칸 → 높이 10인 칸 : 비용 5
- 높이 10인 칸 → 높이 20인 칸 : 비용 10

입출력 예 #2

각 칸의 높이는 다음과 같으며, 높이차가 1 이하인 경우 사다리 없이 이동이 가능합니다.

![land_ladder3.png](https://grepp-programmers.s3.amazonaws.com/files/production/5bfffc0d72/af5db829-8ea1-4f4c-a5a8-ed11e029d135.png)

위 그림과 같이 (2행 1열) → (1행 1열), (1행 2열) → (2행 2열) 두 곳에 사다리를 설치하면 설치비용이 18로 최소가 됩니다.

## 코드

```java
import java.util.Comparator;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;

class Solution {
    public static void main(String[] args) {
        System.out.println(solution(new int[][]{{1, 4, 8, 10}, {5, 5, 5, 5}, {10, 10, 10, 10}, {10, 10, 10, 20}}, 3));
//        System.out.println(solution(new int[][]{{10, 11, 10, 11}, {2, 21, 20, 10}, {1, 20, 21, 11}, {2, 1, 2, 1}}, 1));
    }

    static int[][] group;
    static int groupCount = 0;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[] parent;
    public static int solution(int[][] land, int height) {
        group = new int[land.length][land.length];

        // Make Group
        for (int i = 0; i < land.length; i++) {
            for (int j = 0; j < land.length; j++) {
                if (group[i][j] == 0) {
                    groupCount++;
                    group[i][j] = groupCount;
                    makeGroup(height, land, j, i);
                }
            }
        }

//        Print();
        PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new Comparator<Edge>() {
            @Override
            public int compare(Edge o1, Edge o2) {
                return o1.cost - o2.cost;
            }
        });

        parent = new int[groupCount + 1];
        for (int i = 1; i <= groupCount; i++) parent[i] = i;
        //Make Tree
        for (int i = 0; i < land.length; i++) {
            for (int j = 0; j < land.length; j++) {
                for (int k = 0; k < 4; k++) {
                    int nextX = j + dx[k];
                    int nextY = i + dy[k];
                    if (nextX < 0 || nextX >= land.length || nextY < 0 || nextY >= land.length) continue;
                    if (group[i][j] != group[nextY][nextX]) {
                        priorityQueue.add(new Edge(new Dot(j, i, land[i][j]), new Dot(nextX, nextY, land[nextY][nextX])));
                    }
                }
            }
        }

        int sum = 0;
        while (!priorityQueue.isEmpty()) {
            Edge cur = priorityQueue.poll();
            if (!isParent(group[cur.v1.y][cur.v1.x], group[cur.v2.y][cur.v2.x])) {
                sum += cur.cost;
                Union(group[cur.v1.y][cur.v1.x], group[cur.v2.y][cur.v2.x]);
            }
        }

        return sum;
    }

    public static void Union(int a, int b) {
        a = findParent(a);
        b = findParent(b);

        if (a < b){
            parent[b] = a;
        }else {
            parent[a] = b;
        }
    }

    public static boolean isParent(int a, int b) {
        if (findParent(a) == findParent(b)) return true;
        else return false;
    }

    public static int findParent(int a) {
        if (a == parent[a]) return a;
        else return findParent(parent[a]);
    }

    public static void makeGroup(int height, int[][] land, int x, int y) {
        Queue<Dot> queue = new LinkedList<>();
        queue.add(new Dot(x, y, land[y][x]));

        while (!queue.isEmpty()) {
            Dot cur = queue.poll();
            for (int i = 0; i < 4; i++) {
                int nextX = cur.x + dx[i];
                int nextY = cur.y + dy[i];
                if (nextX < 0 || nextX >= land.length || nextY < 0 || nextY >= land.length
                        || group[nextY][nextX] != 0 || Math.abs(cur.value - land[nextY][nextX]) > height)
                    continue;
                group[nextY][nextX] = groupCount;
                queue.add(new Dot(nextX, nextY, land[nextY][nextX]));
            }
        }
    }

    public static void Print() {
        for (int i = 0; i < group.length; i++) {
            for (int j = 0; j < group.length; j++) {
                System.out.print(group[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println();
    }
}

class Dot {
    int x, y, value;
    Dot(int x, int y, int value) {
        this.x = x;
        this.y = y;
        this.value = value;
    }
}

class Edge {
    Dot v1, v2;
    int cost;

    Edge(Dot v1, Dot v2) {
        this.v1 = v1;
        this.v2 = v2;
        this.cost = Math.abs(v1.value - v2.value);
    }
}
```

- 이 문제는 BFS/DFS + MST의 조합이었다 !

  > MST 공부 내용 👇
  >
  > - [최소신장트리](https://yoo-chaewon.github.io/algorithm/Algorithm-최소신장트리(MST,-Minimum-Spanning-Tree)/)
  > - [BOJ_최소스패닝트리](https://yoo-chaewon.github.io/algorithm/Algorithm-최소신장트리(MST,-Minimum-Spanning-Tree)/)
  > - [네트워크 연결](https://yoo-chaewon.github.io/algorithm/Algorithms-BOJ1922_네트워크연결/)

- 우선 동일한 이동 범위를 같이 그룹핑을 해주기 위해 나는 BFS로 구현해주었다.

  ```java
  for (int i = 0; i < land.length; i++) {
    for (int j = 0; j < land.length; j++) {
      if (group[i][j] == 0) {
        groupCount++;
        group[i][j] = groupCount;
        makeGroup(height, land, j, i);
      }
    }
  }
  ```

  ```java
  public static void makeGroup(int height, int[][] land, int x, int y) {
    Queue<Dot> queue = new LinkedList<>();
    queue.add(new Dot(x, y, land[y][x]));
    
    while (!queue.isEmpty()) {
      Dot cur = queue.poll();
      for (int i = 0; i < 4; i++) {
        int nextX = cur.x + dx[i];
        int nextY = cur.y + dy[i];
        if (nextX < 0 || nextX >= land.length || nextY < 0 || nextY >= land.length
                          || group[nextY][nextX] != 0 || Math.abs(cur.value - land[nextY][nextX]) > height)
          continue;
        group[nextY][nextX] = groupCount;
        queue.add(new Dot(nextX, nextY, land[nextY][nextX]));
      }
    }
  }
  ```

  첫번째 예시라면, 

  ```
  1 1 1 1 
  1 1 1 1 
  2 2 2 2 
  2 2 2 3 
  ```

  이러한 결과가 나올 것이다.

- 이제 이 그룹을 한 노드라고 생각하고 MST알고리즘을 사용하여 연결해주어야한다.

  1. 우선 최소 비용순으로 정렬하기 위해, PriorityQueue를 사용해 주었다.

     ```java
     PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new Comparator<Edge>() {
       @Override
       public int compare(Edge o1, Edge o2) {
         return o1.cost - o2.cost;
       }
     });
     ```

  2. 다음, PriorityQueue에 간선들을 넣기 위해 서로 다른 그룹이 연결된 간선들을 찾는다.

     ```java
     //Make Tree
     for (int i = 0; i < land.length; i++) {
       for (int j = 0; j < land.length; j++) {
         for (int k = 0; k < 4; k++) {
           int nextX = j + dx[k];
           int nextY = i + dy[k];
           if (nextX < 0 || nextX >= land.length || nextY < 0 || nextY >= land.length)
             continue;
           if (group[i][j] != group[nextY][nextX]) {
             priorityQueue.add(new Edge(new Dot(j, i, land[i][j]), new Dot(nextX, nextY, land[nextY][nextX])));
           }
         }
       }
     }
     ```

     (0,0) 에서부터 상,하,좌,우를 보면서 다른 그룹과 연결되어 있는 곳을 찾아 Edge에 저장한다.

     ✔️ Edge는 아래와 같이 두 노드 그리고 간선의 비용이 저장되어 있다.

     ```java
     class Edge {
       Dot v1, v2;
       int cost;
       Edge(Dot v1, Dot v2) {
         this.v1 = v1;
         this.v2 = v2;
         this.cost = Math.abs(v1.value - v2.value);
       }
     }
     ```

  3. 이제 Tree를 최소 비용으로 만든다.

     ```java
     int sum = 0;
     while (!priorityQueue.isEmpty()) {
       Edge cur = priorityQueue.poll();
       if (!isParent(group[cur.v1.y][cur.v1.x], group[cur.v2.y][cur.v2.x])) {
         sum += cur.cost;
         Union(group[cur.v1.y][cur.v1.x], group[cur.v2.y][cur.v2.x]);
       }
     }
     ```

     이것은 크루스칼 알고리즘이랑 같으니 설명은 생략하겠다 ~





##### 기록 

간만에 성취감 잔뜩 느낀 문제 ~

> 문제: https://programmers.co.kr/learn/courses/30/lessons/62050
>
> 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Q_지형이동.java
>