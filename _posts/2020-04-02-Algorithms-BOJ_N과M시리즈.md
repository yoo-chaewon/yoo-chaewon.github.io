---

title:  "[ALGORITHM]BOJ_N과 M"

excerpt: "#BOJ #순열 #조합 #N과 M"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



### 👩‍💻 N과 M(1) - 순열

> [문제](https://www.acmicpc.net/problem/15649)

##### 자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- ##### 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

```java
import java.io.InputStreamReader;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int N = scanner.nextInt();
        int M = scanner.nextInt();

        int[] arr = new int[N + 1];
        for (int i = 1; i <= N; i++) arr[i] = i;
        boolean[] visited = new boolean[N + 1];

        NM(arr, M, visited, 0, "");
    }

    public static void NM(int[] arr, int M, boolean[] visited, int depth, String result) {
        if (depth == M) {
            System.out.println(result);
            return;
        }

        for (int i = 1; i < arr.length; i++) {
            if (!visited[i]) {
                visited[i] = true;
                NM(arr, M, visited, depth + 1, result + arr[i] + " ");
                visited[i] = false;
            }
        }
    }
}
```

```
>>input : 4 2
1 2
1 3
1 4
2 1
2 3
2 4
3 1
3 2
3 4
4 1
4 2
4 3
```



### 👩‍💻 N과 M(2) - 조합

> [문제](https://www.acmicpc.net/problem/15650)

- ##### 자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

  - ##### 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

  - ##### 고른 수열은 오름차순이어야 한다.

```java
import java.io.InputStreamReader;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int N = scanner.nextInt();
        int M = scanner.nextInt();

        int[] arr = new int[N + 1];
        for (int i = 1; i <= N; i++) arr[i] = i;

        NM(arr, M, 1, 0, "");
    }

    public static void NM(int[] arr, int M, int start, int depth, String result) {
        if (depth == M) {
            System.out.println(result);
            return;
        }

        for (int i = start; i < arr.length; i++) {
            NM(arr, M, i + 1, depth + 1, result + arr[i] + " ");
        }
    }
}
```

```
>> input: 4 2
1 2
1 3
1 4
2 3
2 4
3 4
```



### 👩‍💻 N과 M(3) - 중복순열

> [문제](https://www.acmicpc.net/problem/15651)

- ##### 자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

  - ##### 1부터 N까지 자연수 중에서 M개를 고른 수열

  - ##### 같은 수를 여러 번 골라도 된다.

```java
import java.io.InputStreamReader;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int N = scanner.nextInt();
        int M = scanner.nextInt();

        int[] arr = new int[N + 1];
        for (int i = 1; i <= N; i++) arr[i] = i;
        boolean[] visited = new boolean[N + 1];
        NM(arr, M, visited, 0, "");
    }

    public static void NM(int[] arr, int M, boolean[] visited, int depth, String result) {
        if (depth == M) {
            System.out.println(result);
            return;
        }

        for (int i = 1; i < arr.length; i++) {
            visited[i] = true;
            NM(arr, M, visited, depth + 1, result + arr[i] + " ");
            visited[i] = false;
        }
    }
}
```

```
>> input: 4 2
1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
4 1
4 2
4 3
4 4
```



### 👩‍💻 N과 M(4) - 중복조합

> [문제](https://www.acmicpc.net/problem/15652)

- ##### 자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

  - ##### 1부터 N까지 자연수 중에서 M개를 고른 수열

  - ##### 같은 수를 여러 번 골라도 된다.

  - ##### 고른 수열은 비내림차순이어야 한다.

    - ##### 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

```java
import java.io.InputStreamReader;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int N = scanner.nextInt();
        int M = scanner.nextInt();

        int[] arr = new int[N + 1];
        for (int i = 1; i <= N; i++) arr[i] = i;

        NM(arr, M, 1, 0, "");
    }

    public static void NM(int[] arr, int M, int start, int depth, String result) {
        if (depth == M) {
            System.out.println(result);
            return;
        }

        for (int i = start; i < arr.length; i++) {
            NM(arr, M, i, depth + 1, result + arr[i] + " ");
        }
    }
}
```

```
input >> 4 2
1 1
1 2
1 3
1 4
2 2
2 3
2 4
3 3
3 4
4 4
```

