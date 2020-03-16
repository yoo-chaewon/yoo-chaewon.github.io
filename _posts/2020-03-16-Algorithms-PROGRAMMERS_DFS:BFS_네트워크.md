---

title:  "[ALGORITHM]PROGRAMMERS-DFS/BFS-네트워크"

excerpt: "#PROGRAMMERS #DFS #BFS"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- DFS/BFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

##### 제한사항

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

##### 입출력 예

| n    | computers                         | return |
| ---- | --------------------------------- | ------ |
| 3    | [[1, 1, 0], [1, 1, 0], [0, 0, 1]] | 2      |
| 3    | [[1, 1, 0], [1, 1, 1], [0, 1, 1]] | 1      |

##### 입출력 예 설명

예제 #1
아래와 같이 2개의 네트워크가 있습니다.
![image0.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/5b61d6ca97/cc1e7816-b6d7-4649-98e0-e95ea2007fd7.png)

예제 #2
아래와 같이 1개의 네트워크가 있습니다.
![image1.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/7554746da2/edb61632-59f4-4799-9154-de9ca98c9e55.png)



## 코드

```java
class Solution {
    static int size;
    static boolean[] visited;
    public static int solution(int n, int[][] computers) {
        size = n;
        int answer = 0;
        visited = new boolean[n];

        for (int i = 0; i < size; i++){
            if (!visited[i]){
                answer++;
                DFS(computers, i);
            }
        }
        return answer;
    }

    public static void DFS(int[][] computers, int upper){
        visited[upper] = true;
        for (int i = 0; i < size; i++){
            if (computers[upper][i] == 1 && !visited[i]){
                DFS(computers, i);
            }
        }
    }
}
```

- 이 문제는 숫자를 하나하나 보면서, 연결 요소를 다 확인하면 된다.

- 먼저 숫자 n크기의 visited배열을 만들어 주고, 방문 하지 않았으면 탐색을 시작한다.

  ```java
  for (int i = 0; i < size; i++){
    if (!visited[i]){
      answer++;
      DFS(computers, i);
    }
  }
  ```

- 탐색은 연결되어 있고, 방문하지 않은 모든 곳을 하는 것이다.

  ```java
  public static void DFS(int[][] computers, int upper){
    visited[upper] = true;
    for (int i = 0; i < size; i++){
      if (computers[upper][i] == 1 && !visited[i]){
        DFS(computers, i);
      }
    }
  }
  ```

  

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/43162
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/BFS:DFS_네트워크.java
>
