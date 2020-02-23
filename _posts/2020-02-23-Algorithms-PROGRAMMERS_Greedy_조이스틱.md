---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-조이스틱"

excerpt: "#PROGRAMMERS #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Greedy

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
```

예를 들어 아래의 방법으로 JAZ를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
```

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

##### 제한 사항

- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

##### 입출력 예

| name                   | return |
| ---------------------- | ------ |
| JEROEN                 | 56     |
| JAN                    | 23     |
| ABABAAAAAAABA          | 11     |
| XXAAAAAAAAAAAAAXX      | 16     |
| BBAABB                 | 8      |
| ABAABAAAAAAAAAAAABAAAB | 20     |



## 코드

```java
class Solution {
    public int solution(String name) {
        int answer = 0;
        StringBuilder nameStr = new StringBuilder();
        for (int i = 0; i < name.length(); i++) nameStr.append(name.charAt(i));
        StringBuilder tmpStr = new StringBuilder();
        for (int i = 0; i < name.length(); i++) tmpStr.append("A");

        int index = 0;
        while (true) {
            answer += Math.min(name.charAt(index) - 'A', 'Z' - name.charAt(index) + 1);
            nameStr.setCharAt(index, 'A');
            int front = index;
            int back = index;
            int frontA = 0;
            int backA = 0;

            if (nameStr.toString().equals(tmpStr.toString())) break;

            while (nameStr.charAt((front + 1) % name.length()) == 'A') {
                front = (front + 1) % name.length();
                frontA++;
            }
            front = (front + 1) % name.length();

            while (nameStr.charAt((back + name.length() - 1) % name.length()) == 'A') {
                back = (back + name.length() - 1) % name.length();
                backA++;
            }
            back = (back + name.length() - 1) % name.length();

            if (frontA > backA) {
                index = back;
                answer += backA+1;
            } else {
                index = front;
                answer += frontA+1;
            }
        }
        return answer;
    }
}
```

- 이 문제에서 핵심은 **위아래** 움직이는 것 / **좌우**로 움직이는 것 이다.

- name문자열의 문자를 보며, "A"로 바꾸고 모두 "AAAAAAA"가 되면 끝으로 하였다.

- ABABAAAAAAABA 은 1-3-11순으로 바꿔야 한다.

  > ABABAAAAAAABA
  > AAABAAAAAAABA
  > AAAAAAAAAAABA
  > AAAAAAAAAAAAA

- 따라서 문자열 비교를 위해,

  ```java
  StringBuilder nameStr = new StringBuilder();
  for (int i = 0; i < name.length(); i++) nameStr.append(name.charAt(i));//"ABABAAAAAAABA"
  
  StringBuilder tmpStr = new StringBuilder();
  for (int i = 0; i < name.length(); i++) tmpStr.append("A");//"AAAAAAAAAAAAA"
  ```

  nameStr에서 A가 아닌 것을 'A'로 바꾸면서 tmpStr과 같은지 비교한다.

- 먼저 **위아래** 조이스틱을 계산해 준다.

  ```java
  answer += Math.min(name.charAt(index) - 'A', 'Z' - name.charAt(index) + 1);
  //name.charAt(index) - 'A' : ABCD순으로 갈 때
  //'Z' - name.charAt(index) + 1 : ZYXW순으로 갈 때
  nameStr.setCharAt(index, 'A');
  //현재 것을 'A'로 바꿔준다.
  ```

- 현재 문자를 'A'로 바꾼 후 전체 다 보았는지 검사해준다.

  ```java
  if (nameStr.toString().equals(tmpStr.toString())) break;
  ```

  tmpStr = "AAAAAAAAAAAAA"로 같은지 확인하고, 같으면 전체 문자를 다 바꿔준 것이므로 끝내준다.

- 현재 문자를 바꾸고 **좌 우** 중에서 A가 아닌 가장 가까운 문자를 찾아야 한다.

  ```java
  while (nameStr.charAt((front + 1) % name.length()) == 'A') {
    front = (front + 1) % name.length();
    frontA++;
  }
  front = (front + 1) % name.length();
  
  while (nameStr.charAt((back + name.length() - 1) % name.length()) == 'A') {
    back = (back + name.length() - 1) % name.length();
    backA++;
  }
  back = (back + name.length() - 1) % name.length();
  ```

  - 위에 while문을 오른쪽 방향, 아래 while문은 왼쪽 방향이다.
  - index를 움직이면서 A의 수도 세어야한다.

  👉 여기 코드 뭔가 맘에 안든다. 다시 짜봐야 겠다. 우선 답은 나왔으니..

- 그 뒤, A가 더 적은 쪽 방향으로 가야한다.

  ```java
  if (frontA > backA) {
    index = back;
    answer += backA+1;
  } else {
    index = front;
    answer += frontA+1;
  }
  ```

  적은 방향으로 A의 수를 누적하고, index를 변경해준다.

  

##### 느낀점

> 왜 level2인지 모를 정말 어려운 문제ㅠ😭 
>
> 효율적인 면에서 나는 앞뒤로 다 비교를 해봐서 좋은 코드인지 모르겠다. 어떤 사람이 푼 것을 보면, 무슨 식을 만들어서 풀던데 그거는 도저히 이해 못하겠어서ㅠ 다른 방법도 생각해봐야겠다.
>
> 코로나 조심😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42860
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_조이스틱.java
>
