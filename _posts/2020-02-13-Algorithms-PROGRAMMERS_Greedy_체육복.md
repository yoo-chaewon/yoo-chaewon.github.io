---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-체육복"

excerpt: "#PROGRAMMERS #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- GREEDY

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

##### 입출력 예

| n    | lost   | reserve   | return |
| ---- | ------ | --------- | ------ |
| 5    | [2, 4] | [1, 3, 5] | 5      |
| 5    | [2, 4] | [3]       | 4      |
| 3    | [3]    | [1]       | 2      |

##### 입출력 예 설명

예제 #1
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.



## 나의 코드

```java
import java.util.*;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
       int answer = 0;
        int[] arr = new int[n];
        Arrays.fill(arr, 1);

        for (int i = 0; i < lost.length; i++){
            arr[lost[i]-1]--;
        }

        for (int i = 0; i < reserve.length; i++){
            arr[reserve[i]-1]++;
        }

        for (int i = 0 ; i <n; i++){
            if (i != 0 &&  arr[i] == 2 && arr[i-1] ==0){
                arr[i]--;
                arr[i-1]++;
            }
            if (i != n-1 && arr[i] == 2 && arr[i+1] == 0){
                arr[i]--;
                arr[i+1]++;
            }
        }

        for (int a :arr){
            if (a >0 ) answer++;
        }

        return answer;
    }
}
```

- 일단은 체육복 학생별로 체육복 배열을 만든다.

  기본적으로 체육복은 1개씩 갖고 있고, 잃어버리는 사람은 0개, 여벌있는 사람은 2개를 갖고 있다.

  ```java
  Arrays.fill(arr, 1);
  
  for (int i = 0; i < lost.length; i++){
    arr[lost[i]-1]--;
  }
  
  for (int i = 0; i < reserve.length; i++){
    arr[reserve[i]-1]++;
  }
  ```

- 나는 모든 학생들을 앞에서 보면서 앞, 뒤로 확인한다.

  ```java
  for (int i = 0 ; i <n; i++){
    if (i != 0 &&  arr[i] == 2 && arr[i-1] ==0){
      arr[i]--;
      arr[i-1]++;
    }
    if (i != n-1 && arr[i] == 2 && arr[i+1] == 0){
      arr[i]--;
      arr[i+1]++;
    }
  }
  ```

  이때, **앞부터 보고 뒤를 봐야한다**.

  이유는 **앞에서부터 보기 때문에** 만약에 앞에 없는 것은, 그 아이는 앞으로 어디에서도 받아 올 수 없는 것이다.

  그러니까 앞에아이부터 주고, 뒤에 아이는 어차피 뒤에서 받아올 가능성 조차 있는 애니까 일딴 앞사람 부터 준다.

  👉 이 이유 생각 해내는데 진짜 힘들었다........



##### 느낀점

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42862
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/고득점Kit/Greedy_체육복.java

