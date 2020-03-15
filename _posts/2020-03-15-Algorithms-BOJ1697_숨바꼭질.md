---

title:  "[ALGORITHM]BOJ1697_숨바꼭질"

excerpt: "#BOJ"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- BFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.

### 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

### 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

### 예제 입력 1

```
5 17
```

### 예제 출력 1

```
4
```

### 힌트

수빈이가 5-10-9-18-17 순으로 가면 4초만에 동생을 찾을 수 있다.



## 나의 코드

```java
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    static int MAX = 100000;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int N = scanner.nextInt();
        int K = scanner.nextInt();
        int[] map = new int[MAX + 1];

        Queue<Integer> queue = new LinkedList<>();
        queue.add(N);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            if (cur == K) {
                System.out.println(map[cur]);
                return;
            }

            int[] nextN = new int[3];
            nextN[0] = cur + 1;
            nextN[1] = cur - 1;
            nextN[2] = cur * 2;

            for (int i = 0; i < 3; i++) {
                if (nextN[i] < 0 || nextN[i] > MAX || map[nextN[i]] != 0) continue;
                map[nextN[i]] = map[cur] + 1;
                queue.add(nextN[i]);
            }
        }
    }
}

```

> 이문제를 보고 한번에 `BFS`문제다 ! 라는 것을 알아야 한다.
>
> 이 문제를 3번째 푸는 거지만, 매번 DFS로 먼저 시도를 한다.
>
> 최단거리는 BFS 다 !!!!!!!! 제발 기억하자 채똥쓰



##### 느낀점

> 문제 : https://www.acmicpc.net/problem/1697
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1697_숨바꼭질_0315.java

