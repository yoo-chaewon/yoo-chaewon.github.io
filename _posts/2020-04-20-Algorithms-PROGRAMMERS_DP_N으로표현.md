---

title:  "[ALGORITHM]PROGRAMMERS-DP_N으로 표현"

excerpt: "#PROGRAMMERS #DP"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- DP


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

##### 입출력 예

| N    | number | return |
| ---- | ------ | ------ |
| 5    | 12     | 4      |
| 2    | 11     | 3      |

##### 입출력 예 설명

예제 #1
문제에 나온 예와 같습니다.

예제 #2
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.



## 코드

```java
public class Solution {
    public static void main(String[] args) throws Exception {
        System.out.println(solution(5, 12));
        System.out.println(solution(2, 11));
    }

    static int answer = Integer.MAX_VALUE;
    public static int solution(int N, int number) {
        int size = N <= 2 ? 5 : 4;
        int[] map = new int[size];
        int myN = N;
        for (int i = 0; i < size; i++){
            map[i] = myN;
            myN = 10 * myN + N;
        }
        DFS(map, number, 0, 0, 0);
        if (answer == Integer.MAX_VALUE) return -1;
        return answer;
    }

    public static void DFS(int[] arr, int number, int depth, int result, int nCount){
        if (nCount > 8)  return;

        if (number == result){
            answer = Math.min(answer, nCount);
            return;
        }

        for (int i = 0; i < arr.length; i++){
            DFS(arr, number, depth+1, result + arr[i], nCount + i+1);
            DFS(arr, number, depth+1, result - arr[i], nCount + i+1);
            DFS(arr, number, depth+1, result * arr[i], nCount + i+1);
            DFS(arr, number, depth+1, result / arr[i], nCount + i+1);
        }
    }
}
```

- 뭔가 연산이 반복되는? 것이 있기 때문에 DP의 태그인 만큼 DP로 접근하고 싶었다.

  👉 하지만 도저히 모르겠어서 DFS로 접근했다.
  
- 먼저 입력받은 N으로 만들 수 있는 숫자들을 map에 저장했다.

  ```java
  int size = N <= 2 ? 5 : 4;
  int[] map = new int[size];
  int myN = N;
  for (int i = 0; i < size; i++){
    map[i] = myN;
    myN = 10 * myN + N;
  }
  ```

  > number는 1 이상 32,000 이하입니다.

  이와 같은 조건이 있기 때문에 11,111 이랑 22,222는 되지만 3부터 9까지는 9,999밖에 안된다. 

  따라서 2보다 작거나 같으면 길이를 5개, 아닌경우 길이를 4로 해주었다.

  그리고 for문을 돌면서 0번부터 2, 22, 222, 2222, 22222를 넣어주었다.

  **👉 즉 i 번에는 i+1개의 숫자가 있는 것이다.**

- DFS는 

  ```java
  public static void DFS(int[] arr, int number, int depth, int result, int nCount){
    if (nCount > 8)  return;
    if (number == result){
      answer = Math.min(answer, nCount);
      return;
    }
    
    for (int i = 0; i < arr.length; i++){
      DFS(arr, number, depth+1, result + arr[i], nCount + i+1);
      DFS(arr, number, depth+1, result - arr[i], nCount + i+1);
      DFS(arr, number, depth+1, result * arr[i], nCount + i+1);
      DFS(arr, number, depth+1, result / arr[i], nCount + i+1);
    }
  }
  ```

  > 최솟값이 8보다 크면 -1을 return 합니다.

  라는 조건이 있으므로 8이상이 되면 더이상 탐색할 필요가 없어서 return해주었다.

  - 그리고 사칙연산을 모두 한번씩 시행했다.

    👉 nCount의 최대가 8이므로 엄청 깊숙히 탐색하지는 않을 것이다 !

  





##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/42895
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/DP_N으로표현.java
>