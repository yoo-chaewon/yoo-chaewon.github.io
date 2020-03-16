---

title:  "[ALGORITHM]PROGRAMMERS-DFS/BFS-여행경로"

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

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 ICN 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

##### 입출력 예

| tickets                                                     | return                         |
| ----------------------------------------------------------- | ------------------------------ |
| [[ICN, JFK], [HND, IAD], [JFK, HND]]                        | [ICN, JFK, HND, IAD]           |
| [[ICN, SFO], [ICN, ATL], [SFO, ATL], [ATL, ICN], [ATL,SFO]] | [ICN, ATL, ICN, SFO, ATL, SFO] |

##### 입출력 예 설명

예제 #1

[ICN, JFK, HND, IAD] 순으로 방문할 수 있습니다.

예제 #2

[ICN, SFO, ATL, ICN, ATL, SFO] 순으로 방문할 수도 있지만 [ICN, ATL, ICN, SFO, ATL, SFO] 가 알파벳 순으로 앞섭니다.



## 코드

```java
import java.util.*;
class Solution {
    static ArrayList<String> results = new ArrayList<>();
    public static String[] solution(String[][] tickets) {
        Arrays.sort(tickets, new Comparator<String[]>() {
            @Override
            public int compare(String[] o1, String[] o2) {
                return o1[1].compareTo(o2[1]);
            }
        });

        for (int i = 0; i < tickets.length; i++) {
            String start = tickets[i][0];
            String end = tickets[i][1];
            if (start.equals("ICN")) {
                boolean[] visited = new boolean[tickets.length];

                visited[i] = true;
                DFS(tickets, visited, end, 1, "ICN");
                visited[i] = false;
            }
        }
        return results.get(0).split(" ");
    }

    public static void DFS(String[][] ticket, boolean[] visited, String start, int depth, String result) {
        if (depth  == ticket.length) {
            results.add(result + " " + start);
            return;
        }

        for (int i = 0; i < ticket.length; i++) {
            if (!visited[i] && ticket[i][0].equals(start)) {

                visited[i] = true;
                DFS(ticket, visited, ticket[i][1], depth + 1, result + " " + ticket[i][0]);
                visited[i] = false;

            }
        }
    }
}
```

- 네트워크랑 비슷한 문제다.

- start 와 end지점이 있고, end는 다시 start가 되어 여행 경로를 구해주면 되는 문제다.

- 우선, 알파벳 순으로 정렬이라는 조건이 있으므로 나는 먼저 목적지를 알파벳 순으로 정렬해주었다.

  ```java
  Arrays.sort(tickets, new Comparator<String[]>() {
    @Override
    public int compare(String[] o1, String[] o2) {
      return o1[1].compareTo(o2[1]);
    }
  });
  ```

- 시작점은 항상 ICN 이므로, 이 경우 탐색을 시작한다.

  ```java
  for (int i = 0; i < tickets.length; i++) {
    String start = tickets[i][0];
    String end = tickets[i][1];
    if (start.equals("ICN")) {
      boolean[] visited = new boolean[tickets.length];
  
      visited[i] = true;
      DFS(tickets, visited, end, 1, "ICN");
      //visited[i] = false;
    }
  }
  ```

  👉 visited는 행에 대한 방문이다. 만약 1번째 ICN -> SFO 를 방문했다면 다음번에는 이 라인을 방문하지 않게 함을 표시하는 것이다.

  👉 무조건 탐색 하는 경로가 모두를 탐색한다는 보장이 없다. 따라서 ICN이 출발지인 경우 모두 탐색을 해줘야 한다.

- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다. 라는 조건은

  모든 루트를 실행한다는 것으로 결과의 길이는 경로들 +1일 것이다.

  따라서

  ```java
  if (depth  == ticket.length) {
    results.add(result + " " + start);
    return;
  }
  ```

  종료조건을 ticket전체를 다 봤을 때고, 결과에 마지막 출발지인 start를 더해준다.

- 탐색은

  ```java
  for (int i = 0; i < ticket.length; i++) {
    if (!visited[i] && ticket[i][0].equals(start)) {
      visited[i] = true;
      DFS(ticket, visited, ticket[i][1], depth + 1, result + " " + ticket[i][0]);
      visited[i] = false;
    }
  }
  ```

  방문하지 않은 ticket중에, 도착지로 준 매개변수가 start와 같은 경우에 탐색을 한다.





##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/43164
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/BFS:DFS_여행경로%20_ver2.java