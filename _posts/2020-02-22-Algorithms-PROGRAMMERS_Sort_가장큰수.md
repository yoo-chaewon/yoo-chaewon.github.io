---

title:  "[ALGORITHM]PROGRAMMERS/Sort-H-가장큰수"

excerpt: "#PROGRAMMERS #Sort"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Sort

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

##### 입출력 예

| numbers           | return  |
| ----------------- | ------- |
| [6, 10, 2]        | 6210    |
| [3, 30, 34, 5, 9] | 9534330 |



## 코드

```java
import java.util.*;
class Solution {
    public String solution(int[] numbers) {
        StringBuilder answer = new StringBuilder();
        String[] numberStr = new String[numbers.length];
        for (int i = 0; i < numbers.length; i++){
            numberStr[i] = String.valueOf(numbers[i]);
        }
        Arrays.sort(numberStr, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return (o2+o1).compareTo(o1+o2);
            }
        });
        if (numberStr[0].equals("0")) return "0";

        for (String str : numberStr) answer.append(str);

        return answer.toString();
    }
}
```

- 처음은 문자열 정렬로 구하려고 했다.

  ```
  [3, 30, 34, 5, 9] 을 int로 정렬하면
  3 5 9 30 34
  [3, 30, 34, 5, 9] 을 String으로 정렬하면
  3 30 34 5 9
  //이것을 반대로 정렬해서 9 5 34 30 3 이렇게 사용해야 한다.
  ```

  위와 같이 된다. 왜냐하면 String은 사전순 정렬이 되기 때문이다.

- 사전순 역순으로 정렬을 해야지 더 큰 수로 나오기 때문에 

  [3, 30, 34, 5, 9] 👉9 5 34 30 3  이 될것이다.

  하지만 30 과 3 은 303 보다 330이 더 클것이다. 따라서 자리수 계산도 중요해진다.

  ```java
  Arrays.sort(numberStr, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
      return (o2+o1).compareTo(o1+o2);
    }
  });
  ```

  따라서 이와 같이 303 330 이렇게 다 만들어 보면서 정렬을 하였다.

- 마지막으로 10번 테스트를 자꾸 통과하지 못해 찾아보니 0, 0이 들어올 경우 최대 값은 0인데, 00으로 출력되는 문제가 있었다. 따라서

  ```java
  if (numberStr[0].equals("0")) return "0";
  ```

  이렇게 해주었다.

  

##### 느낀점

> 코로나 너무 무섭다😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42746
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Sort_가장큰수.java
>
