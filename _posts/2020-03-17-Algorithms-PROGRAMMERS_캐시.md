---

title:  "[ALGORITHM]PROGRAMMERS-캐시"

excerpt: "#PROGRAMMERS #KAKAO"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- KAKAO

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식

- 캐시 크기(`cacheSize`)와 도시이름 배열(`cities`)을 입력받는다.
- `cacheSize`는 정수이며, 범위는 0 ≦ `cacheSize` ≦ 30 이다.
- `cities`는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식

- 입력된 도시이름 배열을 순서대로 처리할 때, 총 실행시간을 출력한다.

### 조건

- 캐시 교체 알고리즘은 `LRU`(Least Recently Used)를 사용한다.
- `cache hit`일 경우 실행시간은 `1`이다.
- `cache miss`일 경우 실행시간은 `5`이다.

### 입출력 예제

| 캐시크기(cacheSize) | 도시이름(cities)                                             | 실행시간 |
| ------------------- | ------------------------------------------------------------ | -------- |
| 3                   | [Jeju, Pangyo, Seoul, NewYork, LA, Jeju, Pangyo, Seoul, NewYork, LA] | 50       |
| 3                   | [Jeju, Pangyo, Seoul, Jeju, Pangyo, Seoul, Jeju, Pangyo, Seoul] | 21       |
| 2                   | [Jeju, Pangyo, Seoul, NewYork, LA, SanFrancisco, Seoul, Rome, Paris, Jeju, NewYork, Rome] | 60       |
| 5                   | [Jeju, Pangyo, Seoul, NewYork, LA, SanFrancisco, Seoul, Rome, Paris, Jeju, NewYork, Rome] | 52       |
| 2                   | [Jeju, Pangyo, NewYork, newyork]                             | 16       |
| 0                   | [Jeju, Pangyo, Seoul, NewYork, LA]                           | 25       |

## 코드

```java
import java.util.*;
class Solution {
  public static int solution(int cacheSize, String[] cities) {
        int answer = 0;
        ArrayList<String> cache = new ArrayList<>();
        for (String str : cities){
            String city = str.toLowerCase();
            if (cache.contains(city)){
                answer+=1;
                cache.remove(city);
                cache.add(city);
            }else {
                answer+=5;
                if (cacheSize == 0) continue;
                if (cache.size() >= cacheSize) cache.remove(0);
                cache.add(city);
            }
        }
        return answer;
    }
}
```

- cache의 자료형을 선택하는데 동일한 Key를 갖지 못하는 Hash를 사용하고 싶었다. 하지만, Hash는 집합개념으로 순서가 따로 없어서 ArrayList를 사용하였다.

- 또한 문제 조건에서 `대소문자 구분을 하지 않는다` 로 제시 되어 있어서, 처음에 입력 받을때 소문자 케이스로 받는다.

  ```java
String city = str.toLowerCase();
  ```
  
- 캐시에 키가 존재하는 경우와 존재하지 않는 경우로 나눠서 해주었다.

- 키가 존재할 경우,

  ```java
  if (cache.contains(city)){
    answer+=1;
    cache.remove(city);
    cache.add(city);
  }
  ```

  > - `cache hit`일 경우 실행시간은 `1`이다. 👉 answer+1를 해주었다.
  >
  > - hit되면 가장 최신에 나온 것으로, 그 자리에서 제거해주고 맨뒤에 붙여준다.
  >
  >   ```java
  >   cache.remove(city);
  >   cache.add(city);
  >   ```

- 키가 존재하지 않을 경우,

  ```java
  else {
    answer+=5;
    if (cacheSize == 0) continue;
    if (cache.size() >= cacheSize) cache.remove(0);
    cache.add(city);
  }
  ```

  >- `cache miss`일 경우 실행시간은 `5`이다. 👉 answer+5를 해주었다.
  >- 이때는 캐시가 꽉찼을 때랑 아닐때로 나뉜다.
  >  - 꽉찼을 경우 맨 앞(가장 오래 된 것)을 제거해주고 뒤에 붙여준다.
  >  - 꽉차지 않았을 경우 붙여준다.
  >-  ‼️ 이때 계속 몇몇의 케이스가 틀리는 경우가 있었는데 cacheSize가 0일때를 고려해주지 않아서였다.
  >  - size가 0인경우 add할 수 없으니 그냥 continue 해준다.

  

##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/17680
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/4.KAKAO/Q_캐시.java