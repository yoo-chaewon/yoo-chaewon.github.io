---

title:  "[ALGORITHM]PROGRAMMERS/Hash-완주하지못한선수"

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

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

##### 입출력 예

| participant                             | completion                       | return |
| --------------------------------------- | -------------------------------- | ------ |
| [leo, kiki, eden]                       | [eden, kiki]                     | leo    |
| [marina, josipa, nikola, vinko, filipa] | [josipa, filipa, marina, nikola] | vinko  |
| [mislav, stanko, mislav, ana]           | [stanko, ana, mislav]            | mislav |

##### 입출력 예 설명

예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

## 코드

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> hashMap = new HashMap<>();
        for (int i = 0; i < participant.length; i++){
            hashMap.put(participant[i], hashMap.getOrDefault(participant[i], 0 )+1);
        }

        for (int i = 0 ; i < completion.length; i++){
            hashMap.put(completion[i], hashMap.get(completion[i]) -1);
        }

        for (String key : hashMap.keySet()){
            if (hashMap.get(key) > 0 ) answer = key;
        }
        return answer;
    }
}
```

- Hash문제 카테고리를 인식해서인지 `HashMap`을 이용해서 풀었다.

- HashMap에 사람의 이름과 동명이인이 있을 수 있으므로, 사람수도 같이 저장하였다.

- 해시에는 동일한 키가 들어가지 않으므로, value에 사람 수를 저장했다. 

  > 예를 들어 "leo" 가 2명이면, key - "leo" / value - 2 가 된다.

  ```java
  for (int i = 0; i < participant.length; i++){
    hashMap.put(participant[i], hashMap.getOrDefault(participant[i], 0 )+1);
  }
  ```

- 위에 코드는 `getOrDefault()` 메소드를 사용하였다.

  - 동일한 키는 저장되지 않기 때문에, 만약 key가존재하면 그 존재하는 key에 +1을 한다.

- Hash에 다 저장이 되면, completion에 있는 이름들을 hash의 value를 1씩 빼준다.

  ```java
  for (int i = 0 ; i < completion.length; i++){
    hashMap.put(completion[i], hashMap.get(completion[i]) -1);
  }
  ```

- 마지막으로, hash를 전체 보면서 value가 1이상 인것을 찾는다.

  ```java
  for (String key : hashMap.keySet()){
    if (hashMap.get(key) > 0 ) answer = key;
  }
  ```

  `단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.`라는 조건이있기 때문에 answer의 답은 한개가 된다. ~

##### 느낀점

> 코로나 조심😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42576
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Hash_완주하지%20못한%20선수.java
>
