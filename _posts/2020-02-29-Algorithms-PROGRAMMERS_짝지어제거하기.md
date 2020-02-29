---

title:  "[ALGORITHM]PROGRAMMERS/짝지어 제거하기"

excerpt: "#PROGRAMMERS"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

짝지어 제거하기는, 알파벳 소문자로 이루어진 문자열을 가지고 시작합니다. 먼저 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾습니다. 그다음, 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙입니다. 이 과정을 반복해서 문자열을 모두 제거한다면 짝지어 제거하기가 종료됩니다. 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성해 주세요. 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴해주면 됩니다.

예를 들어, 문자열 S = `baabaa` 라면

b *aa* baa → *bb* aa → *aa* →

의 순서로 문자열을 모두 제거할 수 있으므로 1을 반환합니다.

##### 제한사항

- 문자열의 길이 : 1,000,000이하의 자연수
- 문자열은 모두 소문자로 이루어져 있습니다.

------

##### 입출력 예

| s      | result |
| ------ | ------ |
| baabaa | 1      |
| cdcd   | 0      |

##### 입출력 예 설명

입출력 예 #1
위의 예시와 같습니다.
입출력 예 #2
문자열이 남아있지만 짝지어 제거할 수 있는 문자열이 더 이상 존재하지 않기 때문에 0을 반환합니다.



## 코드

```java
import java.util.*;

class Solution{
    public int solution(String s){
        int answer = 0;
        Stack<Character> stack = new Stack<>();
        
        for(int i = 0; i < s.length(); i++){
            if(!stack.isEmpty() && stack.peek()==s.charAt(i)) stack.pop(); 
            else stack.push(s.charAt(i));
        }
        
        if (stack.isEmpty()) answer = 1;
        else answer = 0;
        
        return answer;
    }
}
```

- 처음 이 문제를 배열로 해서 하니 시간초과가 나서 `스택` 을 사용하여 다시 짰다.

- 처음부터 문자열을 보면서, 기본적으로 스택에 문자를 넣는다.

- 스택이 비어있지 않을 경우, 맨위에 있는 것을 peek해서 현재 문자와 비교를 한다.

  > 이때 문자가 같으면 기존에 있는 것을 pop해주고,
  >
  > 다르면 현재 문자를 push한다.

- 이렇게 전체 문자를 보면 같은 문자는 사라지게 된다.

- 따라서, stack이 비어있으면 모두 제거 된 것이고, stack이 비어있지 않으면 제거되지 않은것이다.

##### 비고

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/12933
>
