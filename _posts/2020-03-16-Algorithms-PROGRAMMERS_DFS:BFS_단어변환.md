---

title:  "[ALGORITHM]PROGRAMMERS-DFS/BFS-단어변환"

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

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```

예를 들어 begin이 hit, target가 cog, words가 [hot,dot,dog,lot,log,cog]라면 hit -> hot -> dot -> dog -> cog와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

##### 입출력 예

| begin | target | words                          | return |
| ----- | ------ | ------------------------------ | ------ |
| hit   | cog    | [hot, dot, dog, lot, log, cog] | 4      |
| hit   | cog    | [hot, dot, dog, lot, log]      | 0      |

##### 입출력 예 설명

예제 #1
문제에 나온 예와 같습니다.

예제 #2
target인 cog는 words 안에 없기 때문에 변환할 수 없습니다.



## 코드

```java
class Solution {
    static int answer = Integer.MAX_VALUE;
    static String mtarget;
    static String[] mwords;
    static boolean[] visited;
    static boolean flag = false;

    public static int solution(String begin, String target, String[] words) {
        mtarget = target;
        mwords = words;
        visited = new boolean[words.length];
        DFS(begin, 0);

        if (flag) return answer;
        else return 0;
    }

    public static void DFS(String cur, int count) {
        if (cur.equals(mtarget)) {
            answer = Math.min(count, answer);
            flag = true;
            return;
        }

        for (int i = 0; i < mwords.length; i++) {
            if (!visited[i]) {
                int tmp = 0;
                for (int j = 0; j < mtarget.length(); j++) {
                    if (cur.charAt(j) == mwords[i].charAt(j)) tmp++;
                }
                if (tmp >= mtarget.length()-1) {
                    visited[i] = true;
                    DFS(mwords[i], count + 1);
                    visited[i] = false;
                }
            }
        }
    }
}
```

- 이 문제는 각 단어를 노드로 삼는 그래프를 생성해서 DFS를 통해 풀어야 하는 문제이다.
- 문제의 조건에 맞는 단어끼리 연결을 해서 간선을 만들어 begin부터 target노드까지 최소 거리를 구하면 된다.

> begin = “hit”
>
> target = “hhh”
>
> wodrs = [“hhh”,“hht”]
>
> return = 2

위와 같은 case의 경우에 hit -> hht -> hhh 로 가야하기 때문에, 방문여부를 파악해야 한다.

👉 단지 처음부터 보기만 하면 앞에 있는 것을 못가기 때문이다.

- 그래서, visited변수를 만들어 주었다.

  ```java
  static boolean[] visited;
  ```

- 탐색은 word배열에 있는 전부를 해야 한다, 단 이때 방문하지 않은 것만해줘야 한다.

- 이때 조건이 현재 단어와 다음 이동할 단어는 딱 ! 한자기 단어만 달라야 한다.

  ```java
  for (int i = 0; i < mwords.length; i++) {
    if (!visited[i]) {//방문 하지 않은 단어만,
      //글자수 비교를 위해 tmp변수 사용
      int tmp = 0;
      for (int j = 0; j < mtarget.length(); j++) {
        if (cur.charAt(j) == mwords[i].charAt(j)) tmp++;//현재 단어와 이동할 단어의 같은 글자수를 세준다.
      }
      if (tmp >= mtarget.length()-1) {//1글자만 다른 경우 그 노드로 이동한다.
        visited[i] = true;
        DFS(mwords[i], count + 1);//다음 노드 이동
        visited[i] = false;
      }
    }
  }
  ```

  👉 여기서 예제가 단어 3글자여서 2글자 이상 같으면으로 했다가 계속 틀렸었다.

  `tmp >= mtarget.length()-1` 로  해줘야한다 !

- 종료 조건은 현재 단어가 target이랑 같을 경우다,

  ```java
  if (cur.equals(mtarget)) {
    answer = Math.min(count, answer);
    flag = true;
    return;
  }
  ```

- 만약 변환될 수 없는 경우는 위 조건에 한번도 안들어오는 경우다,

  그래서 flag변수를 사용하였다.

##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/43163
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/BFS:DFS_단어변환.java