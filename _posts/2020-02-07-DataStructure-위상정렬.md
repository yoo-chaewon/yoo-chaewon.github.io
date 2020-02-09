---

title:  "[Data structure]위상정렬(Topological Sort)"

excerpt: "#자료구조 #Java #위상정렬"



categories:

- Data structure

tags:

- Data structure

- 위상정렬

- Java

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



https://www.acmicpc.net/problem/1766

이 문제를 풀려고 하는데

시간초과가 나서 분류를 보니 `힙`  `위상정렬`이라서 먼저 **위상정렬**을 공부해보고자 한다.



# 위상정렬(Topology Sort)

- `위상정렬(Topology Sort)` 은 `순서가 정해져 있는 작업` 을 차례대로 수행해야 할 때 그 순서를 결정해주기 위해 사용하는 알고리즘이다. 

  ‼️‼️‼️‼️이거 진짜 중요 ! **순서 정해주는 문제**일때는 위상정렬 생각하자‼️‼️‼️‼️

- ##### 즉, 어떤 일을 하는 순서를 찾는 알고리즘 !

  👉 방향그래프에 존재하는 각 정점들의 선행순서를 위배하지 않으면서 모든 정점을 나열함.

- 답이 여러개 나올 수 있다. 즉, 하나의 방향 그래프에서 여러 위상정렬이 가능하다.

- 위상정렬은 `DAG(Directed Acyclic Graph)` 에만 적용 가능함. 

  👉 사이클이 발생하지 않는 방향 그래프

  ​		왜? 위상 정렬은 시작점이 존재해야하는데 사이클이 있으면 시작점을 찾을 수 없기 때문.



### 위상정렬 해결책

- 현재 그래프는 위상 정렬 가능한지.
- 위상정렬이 가능하다면 그 결과가 무엇인지

👉 이 두가지를 알아낼 수 있음.

> `큐`을 이용한 해결법
>
> - 진입차수가 0 인 정점을 큐에 삽입한다.
> - 큐에서 원소를 꺼내 연결된 모든 간선을 제거한다.
> - 간선 제거 이후에 진입차수가 0이 된 정점을 큐에 삽입한다.
> - 큐가 빌때까지 2-3번 과정을 반복한다. 모든 원소를 방문하기 전에 큐가 빈다면 사이클이 존재하는 것이고, 모든 원소를 방문했다면 큐에서 꺼낸 순서가 위상정렬의 결과이다.
>
> ```java
> ArrayList<List<Integer>> nodeArray = new ArrayList<List<Integer>>();
> int[] count = new int[N];//자신을 가리키는 정점 저장 배열.
> Queue<Integer> queue = new LinkedList<Integer>();
> 
> //for :진입차수가 0인 정점 큐에 삽입
> //while(!queue.isEmpty()) 모든 간선 제거/ 진입차수 0이된 정점 큐에 삽입
> ```



## 문제

- 문제집: https://www.acmicpc.net/problem/1766

  ```java
  import java.io.BufferedReader;
  import java.io.InputStreamReader;
  import java.util.*;
  
  public class Main {
      public static void main(String[] args) throws Exception {
          BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
          String[] NM = bufferedReader.readLine().split(" ");
          int N = Integer.parseInt(NM[0]);
          int M = Integer.parseInt(NM[1]);
  
          int[] count = new int[N+1];
          ArrayList<Integer>[] arrayList = new ArrayList[N+1];
          PriorityQueue<Integer> queue = new PriorityQueue<>();
  
          for (int i = 0; i < N+1; i++) {
              arrayList[i] = new ArrayList<>();
          }
  
          for (int i = 0; i < M; i++) {
              String[] input = bufferedReader.readLine().split(" ");
              arrayList[Integer.parseInt(input[0])].add(Integer.parseInt(input[1]));
              count[Integer.parseInt(input[1])]++;
          }
  
          for (int i = 1; i < N+1; i++){
              if (count[i] == 0) queue.add(i);
          }
  
          while (!queue.isEmpty()){
              int cur = queue.poll();
              System.out.print(cur + " ");
  
              for (int i = 0; i < arrayList[cur].size(); i++){
                  count[arrayList[cur].get(i)]--;
                  if (count[arrayList[cur].get(i)] == 0){
                      queue.add(arrayList[cur].get(i));
                  }
              }
          }
      }
  }
  ```

  - 위상정렬 문제로 접근을 하였다.

  - 변수를 설명하자면,

    ```java
    //자신을 가르키는 간선의 수를저장하기 위한 배열.
    int[] count = new int[N+1];
    //연결리스트를 저장하기 위한 배열.
    ArrayList<Integer>[] arrayList = new ArrayList[N+1];
    //간선이 0인 주를 저장하는 큐. 
    //이 문제의 조건에서 '가능하면 쉬운 문제부터 풀어야 한다.' 라는 조건이 있으므로,
    //오름차순으로 정렬되는 PriorityQueue로해주었다.
    PriorityQueue<Integer> queue = new PriorityQueue<>();
    ```

  - 위상정렬을 푸는 순서대로 문제를 해결하면 된다.

    1. 연결리스트에 저장 및 간선 수 저장

       ```java
       for (int i = 0; i < M; i++) {
         String[] input = bufferedReader.readLine().split(" ");
         arrayList[Integer.parseInt(input[0])].add(Integer.parseInt(input[1]));
         count[Integer.parseInt(input[1])]++;
       }
       ```

    2. 간선의 수가 0인 배열 큐에 넣어주기

       ```java
       for (int i = 1; i < N+1; i++){
         if (count[i] == 0) queue.add(i);
       }
       ```

    3. 큐가 empty가 아닐때까지, 큐에서 꺼낸 간선들이 연결 되어 있는것을 간선을 한개씩 제거해주면서 큐에 넣음.

       ```java
       while (!queue.isEmpty()){
         int cur = queue.poll();
         System.out.print(cur + " ");
         
         for (int i = 0; i < arrayList[cur].size(); i++){
           count[arrayList[cur].get(i)]--;
           if (count[arrayList[cur].get(i)] == 0){
             queue.add(arrayList[cur].get(i));
           }
         }
       }
       ```

       


- 추가 문제

  > 줄세우기: 백준2252
  >
  > 작업: 백준2056
  >
  > 게임개발: 백준1516