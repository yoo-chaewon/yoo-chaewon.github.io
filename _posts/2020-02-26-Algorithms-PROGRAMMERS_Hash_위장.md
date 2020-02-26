---

title:  "[ALGORITHM]PROGRAMMERS/Hash-위장"

excerpt: "#PROGRAMMERS #Hash"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Hash

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

##### 입출력 예

| clothes                                                      | return |
| ------------------------------------------------------------ | ------ |
| [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
| [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |

##### 입출력 예 설명

예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```



## 코드

```java
import java.util.HashMap;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        HashMap<String, Integer> hashMap = new HashMap<>();

        for (int i = 0; i < clothes.length; i++) {
            hashMap.put(clothes[i][1], hashMap.getOrDefault(clothes[i][1], 0) + 1);
        }

        for (String key : hashMap.keySet()){
            answer *= (hashMap.get(key)+1);
        }

        return answer-1;
    }
}
```

- 이것은 그냥 `조합의 경우의 수`를 구하는 문제이다.

  ✔️ 순서가 있는 경우 `순열` , 순서가 없는 경우 `조합`

  > [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] 이 있을 때,
  >
  > 1. yellow_hat
  > 2. blue_sunglasses
  > 3. green_turban
  > 4. yellow_hat + blue_sunglasses
  > 5. green_turban + blue_sunglasses
  >
  > 👉 이렇게 5개의 경우가 나온다. 
  >
  > Headergear : 2개, eyewear : 1개 **=** (2+1) * (1+1) -1 = 5 개가 나온다.
  >
  > 즉, **(a+1)(b+1)(c+1) = a + b + c + ab + bc + abc + 1** 이기 때문에 각각 +1씩해주고 마지막에 -1을 한다.

##### 느낀점

> 자나깨나 코로나 조심😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42578
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Hash_위장.java
>
