---

title:  "[ALGORITHM]BOJ5052_전화번호목록"

excerpt: "#BOJ #Hash"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- Hash

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

전화번호 목록이 주어진다. 이때, 이 목록이 일관성이 있는지 없는지를 구하는 프로그램을 작성하시오.

전화번호 목록이 일관성을 유지하려면, 한 번호가 다른 번호의 접두어인 경우가 없어야 한다.

예를 들어, 전화번호 목록이 아래와 같은 경우를 생각해보자

- 긴급전화: 911
- 상근: 97 625 999
- 선영: 91 12 54 26

이 경우에 선영이에게 전화를 걸 수 있는 방법이 없다. 전화기를 들고 선영이 번호의 처음 세 자리를 누르는 순간 바로 긴급전화가 걸리기 때문이다. 따라서, 이 목록은 일관성이 없는 목록이다. 

### 입력

첫째 줄에 테스트 케이스의 개수 t가 주어진다. (1 ≤ t ≤ 50) 각 테스트 케이스의 첫째 줄에는 전화번호의 수 n이 주어진다. (1 ≤ n ≤ 10000) 다음 n개의 줄에는 목록에 포함되어 있는 전화번호가 하나씩 주어진다. 전화번호의 길이는 길어야 10자리이며, 목록에 있는 두 전화번호가 같은 경우는 없다.

### 출력

각 테스트 케이스에 대해서, 일관성 있는 목록인 경우에는 YES, 아닌 경우에는 NO를 출력한다.



### 예제 입력 1

```
2
3
911
97625999
91125426
5
113
12340
123440
12345
98346
```

### 예제 출력 1 

```
NO
YES
```

##### 

## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(bufferedReader.readLine());

        while (t-- > 0) {
            int size = Integer.parseInt(bufferedReader.readLine());
            String[] arr = new String[size];
            for (int i = 0; i < size; i++){
                 arr[i] = bufferedReader.readLine();
            }
            if (CheckContain(arr, size)) System.out.println("YES");
            else System.out.println("NO");
        }
    }

    public static boolean CheckContain(String[] arr, int size){
        for (int i = 0; i < size-1; i++){
            for (int j = i+1; j < size; j++){
                if (arr[j].startsWith(arr[i]) || arr[i].startsWith(arr[j])){
                    return false;
                }
            }
        }
        return true;
    }
}
```

- **시간 초과**가 난 코드이다.

- 모든 단어들을 일일이 다 비교를 했다.

  ```java
  public static boolean CheckContain(String[] arr, int size){
    for (int i = 0; i < size-1; i++){
      for (int j = i+1; j < size; j++){
        if (arr[j].startsWith(arr[i]) || arr[i].startsWith(arr[j])){
          return false;
        }
      }
    }
    return true;
  }
  ```

- 예를들면

  ```
  911
  97625999
  91125426
  
  인 경우,
  911 과 97625999 비교, 911와 91125426 비교.
  97625999와 91125426를 비교한다.
  ```

  이 경우 아무래도 시간복잡도가 높아 시간초과가 난거 같다.

  

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(bufferedReader.readLine());

        while (t-- > 0) {
            int size = Integer.parseInt(bufferedReader.readLine());
            String[] arr = new String[size];
            for (int i = 0; i < size; i++){
                arr[i] = bufferedReader.readLine();
            }
            Arrays.sort(arr);

            if (CheckContain(arr, size)) System.out.println("YES");
            else System.out.println("NO");
        }
    }

    public static boolean CheckContain(String[] arr, int size){
        for (int i = 0; i < size-1; i++){
            String cur = arr[i];
            String next = arr[i+1];
            if (cur.startsWith(next) || next.startsWith(cur)) return false;
        }
        return true;
    }
}
```

- 다시 짜본 코드는 이러하다.

- 일일이 비교를 하는 것이 아니라, 비교를 하기 전에 **사전순으로 정렬**을 하였다.

- ```
  //input
  91125426
  12340
  123440
  911
  
  //String 정렬 👉 사전순 정렬
  12340
  123440
  911
  91125426
  
  //int 정렬 👉 정수의 크기 오름차순
  911
  12340
  123440
  91125426
  ```

- 사전순으로 정렬을 하게 되면, 다음 것만 확인을 하면 된다.

- 따라서,

- ```java
  public static boolean CheckContain(String[] arr, int size){
    for (int i = 0; i < size-1; i++){
      String cur = arr[i];
      String next = arr[i+1];
      if (cur.startsWith(next) || next.startsWith(cur)) return false;
    }
    return true;
  }
  ```

  



#### 메소드 비교

> equal()
>
> contain()
>
> startWith()

##### 느낀점

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/43165#
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/DFS:BFS_타겟넘버.java

