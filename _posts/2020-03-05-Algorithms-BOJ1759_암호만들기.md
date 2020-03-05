---

title:  "[ALGORITHM]BOJ1759_암호만들기"

excerpt: "#BOJ #DFS #백트래킹"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- DFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

바로 어제 최백준 조교가 방 열쇠를 주머니에 넣은 채 깜빡하고 서울로 가 버리는 황당한 상황에 직면한 조교들은, 702호에 새로운 보안 시스템을 설치하기로 하였다. 이 보안 시스템은 열쇠가 아닌 암호로 동작하게 되어 있는 시스템이다.

암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한 개의 모음과 최소 두 개의 자음으로 구성되어 있다고 알려져 있다. 또한 정렬된 문자열을 선호하는 조교들의 성향으로 미루어 보아 암호를 이루는 알파벳이 암호에서 증가하는 순서로 배열되었을 것이라고 추측된다. 즉, abc는 가능성이 있는 암호이지만 bac는 그렇지 않다.

새 보안 시스템에서 조교들이 암호로 사용했을 법한 문자의 종류는 C가지가 있다고 한다. 이 알파벳을 입수한 민식, 영식 형제는 조교들의 방에 침투하기 위해 암호를 추측해 보려고 한다. C개의 문자들이 모두 주어졌을 때, 가능성 있는 암호들을 모두 구하는 프로그램을 작성하시오.

### 입력

첫째 줄에 두 정수 L, C가 주어진다. (3 ≤ L ≤ C ≤ 15) 다음 줄에는 C개의 문자들이 공백으로 구분되어 주어진다. 주어지는 문자들은 알파벳 소문자이며, 중복되는 것은 없다.

### 출력

각 줄에 하나씩, 사전식으로 가능성 있는 암호를 모두 출력한다.

### 예제 입력 1

```
4 6
a t c i s w
```

### 예제 출력 1

```
acis
acit
aciw
acst
acsw
actw
aist
aisw
aitw
astw
cist
cisw
citw
istw
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {
    static int L;
    static int C;
    static char[] map;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] LC = bufferedReader.readLine().split(" ");
        L = Integer.parseInt(LC[0]);
        C = Integer.parseInt(LC[1]);

        map = new char[C];
        String[] input = bufferedReader.readLine().split(" ");
        for (int i = 0; i < C; i++) {
            map[i] = input[i].charAt(0);
        }

        Arrays.sort(map);

        boolean[] visited = new boolean[C];

        DFS(0, 0, 0, 0, "");


    }

    public static void DFS(int start, int depth, int ja, int mo, String result) {
        if (depth == L) {
            if (mo >= 1 && ja >= 2) {
                System.out.println(result);
            }
            return;
        }

        for (int i = start; i < C; i++) {
            char cur = map[i];
            switch (cur) {
                case 'a': case 'e': case 'i': case 'o': case 'u':
                    DFS(i + 1, depth + 1, ja, mo + 1, result + map[i]);
                    break;
                default:
                    DFS(i + 1, depth + 1, ja + 1, mo, result + map[i]);
                    break;
            }
        }
    }
}
```

- 로또 문제와 비슷한 문제다. 

- 다만 다른 것은 자음과 모음의 수를 세어서 조건에 부합하도록 해야한다.

- 먼저 암호의 조건을 보면,

  - 암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한 개의 모음과 최소 두 개의 자음으로 구성되어 있다고 알려져 있다. 

    👉 DFS의 결과를 내어줄 때,

    ```java
    if (depth == L) {
      if (mo >= 1 && ja >= 2) {
        System.out.println(result);
      }
      return;
    }
    ```

    조건에 맞는 결과만 출력해주었다.

    👉 자음과 모음 세어주는 코드

    ```java
    char cur = map[i];
    switch (cur) {
      case 'a': case 'e': case 'i': case 'o': case 'u':
        DFS(i + 1, depth + 1, ja, mo + 1, result + map[i]);
        break;
      default:
        DFS(i + 1, depth + 1, ja + 1, mo, result + map[i]);
        break;
    }
    ```

    모음일 경우, 모음의 수를 +1해주었고, 자음일 경우 자음의 수를 +1해주었다.

  - 또한 정렬된 문자열을 선호하는 조교들의 성향으로 미루어 보아 암호를 이루는 알파벳이 암호에서 증가하는 순서로 배열되었을 것이라고 추측된다. 즉, abc는 가능성이 있는 암호이지만 bac는 그렇지 않다.

    👉 문자들을 정렬해주었다.

    ```java
     Arrays.sort(map);
    ```

- 순열 내는 코드는 따로 적지 않겠다 ～





##### 느낀점

> 문제 : https://www.acmicpc.net/problem/1759
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1759_암호만들기.java

