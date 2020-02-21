---

title:  "[ALGORITHM]PROGRAMMERS/Greedy-큰수만들기"

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

어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 `number의 자릿수` 미만인 자연수입니다.

##### 입출력 예

| number     | k    | return |
| ---------- | ---- | ------ |
| 1924       | 2    | 94     |
| 1231234    | 3    | 3234   |
| 4177252841 | 4    | 775841 |



## 코드 ver1-순열

```java
import java.util.ArrayList;
import java.util.Collections;

class Solution {
    static ArrayList<String> arr = new ArrayList<>();

    public static String solution(String number, int k) {
        String answer = "";
        DFS(0, 0, number.length() - k, number, "");

        Collections.sort(arr);
        return arr.get(arr.size()-1);
    }

    public static void DFS(int start, int depth, int size, String number, String result) {
        if (depth == size) {
            arr.add(result);
            return;
        }

        for (int i = start; i < number.length(); i++) {
            DFS(i + 1, depth + 1, size, number, result + number.charAt(i));
        }
    }
}
```

- 처음은 딱 생각 난 것은 **모든 순열 구하고 가장 큰 수 구하면 되겠다 !** 였다.
- 이것은 정말로 최악의 코드 였다. 모든 순열을 다 해보는 이 작업는 시간초과 공간초과 파티였다.
- 그래서 머리를 좀 더 쓰기로 했다.



## 코드 ver 2-뒤에 문자 비교

```java
import java.util.ArrayList;

class Solution {
    public String solution(String number, int k) {
       StringBuilder answer = new StringBuilder();
        ArrayList<Character> arrayList = new ArrayList<>();

        for (int i = 0; i < number.length(); i++) {
            arrayList.add(number.charAt(i));
        }

        for (int i = 0; i < k; i++) {
            for (int j = 0; j < arrayList.size() - 1; j++) {
                if (arrayList.get(j) < arrayList.get(j + 1)) {
                    arrayList.remove(j);
                    break;
                }
            }
        }

        for (int i = 0; i < number.length() - k; i++) answer.append(arrayList.get(i));
        return answer.toString();
    }
}
```

결과를 보자면 이러하다. 저 10번 문제가 도대체 뭔지 모르겠다 !!!!!!!

```
테스트 1 〉통과 (1.10ms, 52.3MB)
테스트 2 〉통과 (1.11ms, 50.8MB)
테스트 3 〉통과 (1.06ms, 50.2MB)
테스트 4 〉통과 (2.38ms, 50.7MB)
테스트 5 〉통과 (4.12ms, 52.3MB)
테스트 6 〉통과 (47.87ms, 52.6MB)
테스트 7 〉통과 (101.85ms, 56.8MB)
테스트 8 〉통과 (559.95ms, 58.4MB)
테스트 9 〉통과 (129.13ms, 81.8MB)
테스트 10 〉실패 (시간 초과)
테스트 11 〉통과 (0.96ms, 52.7MB)
테스트 12 〉통과 (1.00ms, 50.4MB)
```

- 일단 모든 순열을 만들어서 찾는 것은 무식한 방법이라는 것을 알고 다시 생각했다.

- 가장 큰 수를 구하려면 가장 앞에 있는 숫자가 가장 커야한다.

- 즉, 앞에서부터 작은 숫자들을 지워나가면 된다.

  ```java
  for (int i = 0; i < k; i++) {
    for (int j = 0; j < arrayList.size() - 1; j++) {
      if (arrayList.get(j) < arrayList.get(j + 1)) {
        arrayList.remove(j);
        break;
      }
    }
  }
  ```

  현재 숫자와 다음숫자를 비교해서 다음숫자가 크면, 현재 숫자를 지워주는 셈이다.

  하지만 821에서 2개를 지워야 한다면, 8이 정답이 될 것이다. 하지만 위의 알고리즘상 for문을 끝내도 821상태일 것이다.

- 즉 저 for 문을 끝내면 내림차순으로 정렬이 되어 있을 것이다.

  ```java
  for (int i = 0; i < number.length() - k; i++) answer.append(arrayList.get(i));
  return answer.toString();
  ```

  따라서 이런식으로, 원하는 갯수만큼 앞에서 숫자를 가져와서 결과를 만들어 내면 된다.



## 코드 ver 3 - 범위정해줘서 가장 큰 수 뽑음

```java
class Solution {
    public String solution(String number, int k) {
        StringBuilder answer = new StringBuilder();
        int startIndex = 0;
        int length = number.length() - k;
        int endIndex = number.length()-length+1;

        for (int i = 0 ; i < number.length()-k; i++){
            int max = Integer.MIN_VALUE;
            int index = 0;
            for (int j = startIndex; j < endIndex+i; j++){
                if (max < number.charAt(j)-'0'){
                    max = number.charAt(j)-'0';
                    index = j;
                }
            }
            answer.append(max);
            startIndex = index+1;
        }
        return answer.toString();
    }
}
```

- 두번째 방법에서는 숫자들을 제거하면서 가장 큰 수를 뽑았다면, 이 문제에서는 범위 내에서 가장 큰 숫자들을 뽑는 방법이다 (새로운 발상이다 👉 아이디어 from 미진)

- 예시를 들면서 설명을 하겠다.

  > 입력: 4177252841 / k : 4
  >
  > 라면, 만들어야 하는 문자의 길이는 number.legth - 4 = 6이다.
  >
  > 따라서, 
  >
  > ```java
  > for (int i = 0 ; i < number.length()-k; i++){//이만큼 반복한다.
  > }
  > ```

- 문자를 뽑는 기준은,(설명 어려워서 막막)

  > 4177252841 에서 우선 1개를 뽑아야한다. 총 6개를 뽑아야 하기 때문에 가장 큰것을 8을 뽑는다면 그 다음꺼에서 뽑을 게 없어진다. 따라서 
  >
  > startIndex와 endIndex로 범위를 정해준다. 
  >
  > 처음에는 0에서부터 number.length - 뽑아야할길이 +1이다. 즉 (0 <= x < 5) 다.

- | 0 <= x < 5  | Max: 7  , index:2     |
  | ----------- | --------------------- |
  | 3 <= x < 6  | Max: **7**  , index:3 |
  | 4 <= x < 7  | Max: **5**  , index:5 |
  | 6 <= x < 8  | Max: **8**  , index:7 |
  | 8 <= x < 9  | Max: **4**  , index:8 |
  | 9 <= x < 10 | Max: **1**  , index:9 |

  코드를 보면

  ```java
  class Solution {
      public String solution(String number, int k) {
          StringBuilder answer = new StringBuilder();
          int startIndex = 0;
          int length = number.length() - k;
          int endIndex = number.length()-length+1;
  
          for (int i = 0 ; i < number.length()-k; i++){
              int max = Integer.MIN_VALUE;
              int index = 0;
            
            //endIndex+i인 이유는 한글자씩 선택될때마다 범위가 줄기 때문이다.
              for (int j = startIndex; j < endIndex+i; j++){ 
                //범위내에서 가장 큰 수와 그 수의 인덱스 저장한다.
                  if (max < number.charAt(j)-'0'){
                      max = number.charAt(j)-'0';
                      index = j;
                  }
              }
              answer.append(max);
              startIndex = index+1;//다음 범위의 시작은 가장큰수 뽑은거 다음부터다!
          }
          return answer.toString();
      }
  }
  ```

  

##### 느낀점

> 스터디를 하다보니 다양한 방법들로 풀 수 있어서 좋았다. 도대체 난 왜 시간초과가 나는지 몰랐는데 새로운 아이디어로 푸니 시간초과가 사라졌다. 뿌듯했다.
>
> ‼️ 그리고 String + 을 하면 매번 내부에서 StringBuilder 가 불린다고 한다. 그러니까 한번 StringBuilder를 생성해서 append로 붙여주면, 나중에 toString해서 한번만 해도 된다 .!
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42883
>
> 저장소 
>
> - https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_큰수만들기_순열_시간초과.java
> - https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_큰수만들기_91.7_시간초과.java
> - https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Greedy_큰수만들기.java

