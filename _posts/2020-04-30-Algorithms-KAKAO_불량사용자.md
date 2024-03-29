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

###### 문제 설명

개발팀 내에서 이벤트 개발을 담당하고 있는 무지는 최근 진행된 카카오이모티콘 이벤트에 비정상적인 방법으로 당첨을 시도한 응모자들을 발견하였습니다. 이런 응모자들을 따로 모아 `불량 사용자`라는 이름으로 목록을 만들어서 당첨 처리 시 제외하도록 이벤트 당첨자 담당자인 프로도 에게 전달하려고 합니다. 이 때 개인정보 보호을 위해 사용자 아이디 중 일부 문자를 '*' 문자로 가려서 전달했습니다. 가리고자 하는 문자 하나에 '*' 문자 하나를 사용하였고 아이디 당 최소 하나 이상의 '*' 문자를 사용하였습니다.
무지와 프로도는 불량 사용자 목록에 매핑된 응모자 아이디를 `제재 아이디` 라고 부르기로 하였습니다.

예를 들어, 이벤트에 응모한 전체 사용자 아이디 목록이 다음과 같다면

| 응모자 아이디 |
| ------------- |
| frodo         |
| fradi         |
| crodo         |
| abc123        |
| frodoc        |

다음과 같이 불량 사용자 아이디 목록이 전달된 경우,

| 불량 사용자 |
| ----------- |
| fr*d*       |
| abc1**      |

불량 사용자에 매핑되어 당첨에서 제외되어야 야 할 제재 아이디 목록은 다음과 같이 두 가지 경우가 있을 수 있습니다.

| 제재 아이디 |
| ----------- |
| frodo       |
| abc123      |

| 제재 아이디 |
| ----------- |
| fradi       |
| abc123      |

이벤트 응모자 아이디 목록이 담긴 배열 user_id와 불량 사용자 아이디 목록이 담긴 배열 banned_id가 매개변수로 주어질 때, 당첨에서 제외되어야 할 제재 아이디 목록은 몇가지 경우의 수가 가능한 지 return 하도록 solution 함수를 완성해주세요.

#### **[제한사항]**

- user_id 배열의 크기는 1 이상 8 이하입니다.
- user_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
  - 응모한 사용자 아이디들은 서로 중복되지 않습니다.
  - 응모한 사용자 아이디는 알파벳 소문자와 숫자로만으로 구성되어 있습니다.
- banned_id 배열의 크기는 1 이상 user_id 배열의 크기 이하입니다.
- banned_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
  - 불량 사용자 아이디는 알파벳 소문자와 숫자, 가리기 위한 문자 '*' 로만 이루어져 있습니다.
  - 불량 사용자 아이디는 '*' 문자를 하나 이상 포함하고 있습니다.
  - 불량 사용자 아이디 하나는 응모자 아이디 중 하나에 해당하고 같은 응모자 아이디가 중복해서 제재 아이디 목록에 들어가는 경우는 없습니다.
- 제재 아이디 목록들을 구했을 때 아이디들이 나열된 순서와 관계없이 아이디 목록의 내용이 동일하다면 같은 것으로 처리하여 하나로 세면 됩니다.

------

##### **[입출력 예]**

| user_id                                           | banned_id                                | result |
| ------------------------------------------------- | ---------------------------------------- | ------ |
| `["frodo", "fradi", "crodo", "abc123", "frodoc"]` | `["fr*d*", "abc1**"]`                    | 2      |
| `["frodo", "fradi", "crodo", "abc123", "frodoc"]` | `["*rodo", "*rodo", "******"]`           | 2      |
| `["frodo", "fradi", "crodo", "abc123", "frodoc"]` | `["fr*d*", "*rodo", "******", "******"]` | 3      |

##### **입출력 예에 대한 설명**

##### **입출력 예 #1**

문제 설명과 같습니다.

##### **입출력 예 #2**

다음과 같이 두 가지 경우가 있습니다.

| 제재 아이디 |
| ----------- |
| frodo       |
| crodo       |
| abc123      |

| 제재 아이디 |
| ----------- |
| frodo       |
| crodo       |
| frodoc      |

##### **입출력 예 #3**

다음과 같이 세 가지 경우가 있습니다.

| 제재 아이디 |
| ----------- |
| frodo       |
| crodo       |
| abc123      |
| frodoc      |

| 제재 아이디 |
| ----------- |
| fradi       |
| crodo       |
| abc123      |
| frodoc      |

| 제재 아이디 |
| ----------- |
| fradi       |
| frodo       |
| abc123      |
| frodoc      |



## 나의 코드

```java
import java.util.*;

public class Solution {
    static HashSet<String> result_set;
    public static int solution(String[] user_id, String[] banned_id) {
        result_set = new HashSet<>();
        boolean[] visited = new boolean[user_id.length];
        Permutation(user_id, banned_id, visited, 0, "");
        return result_set.size();
    }

    public static void Permutation(String[] user_id, String[] banned_id, boolean[] visited, int depth, String result) {
        if (depth == banned_id.length) {
            if (Check(user_id, banned_id, result)) {
                String[] temp = result.split("");
                Arrays.sort(temp);
                StringBuilder tmpStr = new StringBuilder("");
                for (String str : temp) {
                    tmpStr.append(str);
                }
                result_set.add(tmpStr.toString());
            }
            return;
        }

        for (int i = 0; i < user_id.length; i++) {
            if (!visited[i]) {
                visited[i] = true;
                Permutation(user_id, banned_id, visited, depth + 1, result + i + "");
                visited[i] = false;
            }
        }
    }

    public static boolean Check(String[] user_id, String[] banned_id, String result) {
        for (int i = 0; i < result.length(); i++) {
            String id = user_id[result.charAt(i) - '0'];
            String banned = banned_id[i];
            for (int j = 0; j < id.length(); j++) {
                if (id.length() != banned.length()) return false;
                if (banned.charAt(j) != '*' && id.charAt(j) != banned.charAt(j)) return false;
            }
        }
        return true;
    }
}
```

- 이거 제일 오래 걸린 문제이다. 처음에는 조합의 공식을 사용하는 줄 알아서 그것을 찾으려다가 시간이 오래 걸렸다.

  👉 벗뜨, 공식을 구하지 못했다. 아직도 있을거 같아서 미련남지만 버리자.

- 다음 고민은 순열로 뽑아야하는지 조합으로 뽑아야 하는지 고민을 많이 했다.

  > 하지만, banned_id와 비교하는 과정에서 
  >
  > Banned_id는 고정으로 되어 있고,
  >
  > ["*rodo", "*rodo", "* * * * * *"]  // banned_id
  >
  > ["frodo", "crodo", "abc123"]  // id에서 3개 뽑음
  >
  >  👉 순서가 다르게 뽑힐 경우에는 1:1비교시 안된다고 나오기 때문에 **순열**로 해야한다고 판단했다.
  >
  >  👉 값이너무 커질까봐 그래서 공식을 생각했던 것인데, user_id 배열의 크기는 1이상 8이하로주어졌다.

- 순열 코드

  ```java
  public static void Permutation(String[] user_id, String[] banned_id, boolean[] visited, int depth, String result) {
    if (depth == banned_id.length) {//금지 아이디 만큼 뽑았을 경우
      if (Check(user_id, banned_id, result)) {
        String[] temp = result.split("");
        Arrays.sort(temp);
        StringBuilder tmpStr = new StringBuilder("");
        for (String str : temp) {
          tmpStr.append(str);
        }
        result_set.add(tmpStr.toString());
      }
      return;
    }
  
    for (int i = 0; i < user_id.length; i++) {
      if (!visited[i]) {
        visited[i] = true;
        Permutation(user_id, banned_id, visited, depth + 1, result + i + "");
        visited[i] = false;
      }
    }
  }
  ```

- 금지 아이디 만큼 뽑았을 경우 금지와 비교해서 되는 case인지 확인해야 한다.

  ```java
  public static boolean Check(String[] user_id, String[] banned_id, String result) {
    for (int i = 0; i < result.length(); i++) {
      String id = user_id[result.charAt(i) - '0'];
      String banned = banned_id[i];
      for (int j = 0; j < id.length(); j++) {
        if (id.length() != banned.length()) return false;
        if (banned.charAt(j) != '*' && id.charAt(j) != banned.charAt(j)) return false;//* 이 아닌 글자는 모두 같아야 함.
      }
    }
    return true;
  }
  ```

- 근데 여기서 가장 또 오래 고민한 것이,

  Frodo crodo abc123과, crodo Frodo abc123은 같은데 어떻게 같은 것으로 표현하지 였다.

  그래서 일단 Check가 True인 경우, 결과값이 204 와 024면 이것을 정렬해서 result_set에 넣어주었다.

  ```java
  if (Check(user_id, banned_id, result)) {
    String[] temp = result.split("");
    Arrays.sort(temp);
    StringBuilder tmpStr = new StringBuilder("");
    for (String str : temp) {
      tmpStr.append(str);
    }
    result_set.add(tmpStr.toString());
  }
  ```

  👉 이부분코드가 별로라고 생각한다. 다른 좋은 방법 있을 거 같은데 좀더 고민해봐야겠다.

### 🔗 링크

- 문제: https://programmers.co.kr/learn/courses/30/lessons/64064
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/4.KAKAO/Q_튜플.java