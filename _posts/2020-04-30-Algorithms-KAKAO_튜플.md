---

title:  "[ALGORITHM]KAKAO_2019 개발자 겨울 인턴십-튜플"

excerpt: "#PROGRAMMERS #SW #KAKAO"



categories:

- ALGORITHM

tags:

- ALGORITHM

- KAKAO

- PROGRAMMERS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

- (a1, a2, a3, ..., an)

튜플은 다음과 같은 성질을 가지고 있습니다.

1. 중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
2. 원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
3. 튜플의 원소 개수는 유한합니다.

원소의 개수가 n개이고, **중복되는 원소가 없는** 튜플 `(a1, a2, a3, ..., an)`이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

- {{a1}, {a1, a2}, {a1, a2, a3}, {a1, a2, a3, a4}, ... {a1, a2, a3, a4, ..., an}}

예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}

와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}
- {{2, 1, 3, 4}, {2}, {2, 1, 3}, {2, 1}}
- {{1, 2, 3}, {2, 1}, {1, 2, 4, 3}, {2}}

는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

#### **[제한사항]**

- s의 길이는 5 이상 1,000,000 이하입니다.
- s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
- 숫자가 0으로 시작하는 경우는 없습니다.
- s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
- s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
- return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.

------

##### **[입출력 예]**

| s                                 | result       |
| --------------------------------- | ------------ |
| `"{{2},{2,1},{2,1,3},{2,1,3,4}}"` | [2, 1, 3, 4] |
| `"{{1,2,3},{2,1},{1,2,4,3},{2}}"` | [2, 1, 3, 4] |
| `"{{20,111},{111}}"`              | [111, 20]    |
| `"{{123}}"`                       | [123]        |
| `"{{4,2,3},{3},{2,3,4,1},{2,3}}"` | [3, 2, 4, 1] |

##### **입출력 예에 대한 설명**

##### **입출력 예 #1**

문제 예시와 같습니다.

##### **입출력 예 #2**

문제 예시와 같습니다.

##### **입출력 예 #3**

(111, 20)을 집합 기호를 이용해 표현하면 {{111}, {111,20}}이 되며, 이는 {{20,111},{111}}과 같습니다.

##### **입출력 예 #4**

(123)을 집합 기호를 이용해 표현하면 {{123}} 입니다.

##### **입출력 예 #5**

(3, 2, 4, 1)을 집합 기호를 이용해 표현하면 {{3},{3,2},{3,2,4},{3,2,4,1}}이 되며, 이는 {{4,2,3},{3},{2,3,4,1},{2,3}}과 같습니다.



## 나의 코드

```java
import java.util.*;
class Solution {
    public int[] solution(String s) {
        ArrayList<String> arrayList = new ArrayList<>();
        String temp = "";
        for (int i = 1; i < s.length() - 1; i++) {
            if (s.charAt(i) == '{') {
                temp = "";
            } else if (s.charAt(i) == '}') {
                arrayList.add(temp);
            } else {
                temp += s.charAt(i);
            }
        }

        Collections.sort(arrayList, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.split(",").length - o2.split(",").length;
            }
        });

        LinkedHashSet<Integer> result = new LinkedHashSet<>();
        for (String cur : arrayList){
            String[] cur_list = cur.split(",");
            for (int j = 0; j < cur_list.length; j++){
                if (!result.contains(cur_list[j])) result.add(Integer.parseInt(cur_list[j]));
            }
        }

        int[] answer = new int[result.size()];
        int index = 0;
        for (int key : result){
            answer[index++] = key;
            if (index == result.size()) break;
        }
        return answer;
    }
}
```

- 우선 나는 집합의 개수가 가장 작은 것 부터 정렬을 하고 싶었다.

  > `"{{1,2,3},{2,1},{1,2,4,3},{2}}"`
  >
  > 인 경우 {2}, {2,1}, {1,2,3},{1,2,4,3}
  >
  > 그러면, 이제 앞에서부터 확정을 짓는 것이다. (말로 설명 못하겠다..)

- 아무튼 입력이 문자열로 주어져서, 이것을 원하는데로 정렬하는게 까다로웠던 것 같다.

  ```java
  ArrayList<String> arrayList = new ArrayList<>();
  String temp = "";
  //괄호를 제외하고, {2,1} 이면 => 2,1 이렇게 뽑아내는 코드이다.
  for (int i = 1; i < s.length() - 1; i++) {
    if (s.charAt(i) == '{') {
      temp = "";
    } else if (s.charAt(i) == '}') {
      arrayList.add(temp);
    } else {
      temp += s.charAt(i);
    }
  }
  
  Collections.sort(arrayList, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
      return o1.split(",").length - o2.split(",").length;
    }
  });
  //2,1 과 2,1,3이 있을때 앞의 길이는 2, 뒤의 길이는 3 
  //길이로 오름차순을 한 것이다.
  ```

- 다음 기존 집합에 존재 하면 안되는데, 순서가 필요하기 때문에 LinkedHashSet을 사용하였다.

  ```java
  LinkedHashSet<Integer> result = new LinkedHashSet<>();
  for (String cur : arrayList){
    String[] cur_list = cur.split(",");
    for (int j = 0; j < cur_list.length; j++){
      if (!result.contains(cur_list[j])) result.add(Integer.parseInt(cur_list[j]));
      //결과에 존재하지 않으면 넣어준다 !
    }
  }
  ```



📌 자르는 코드 다른 사람것, 공부해볼것 !

```java
String[] arr = s.replaceAll("[{]", " ").replaceAll("[}]", " ").trim().split(" , ");
```

> 진짜 무릎을 팍 쳤다. 내가 딱 원하는 것을 한줄에 구현했네,,,,,,,,,



### 🔗 링크

- 문제: https://programmers.co.kr/learn/courses/30/lessons/64065
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/4.KAKAO/Q_튜플.java