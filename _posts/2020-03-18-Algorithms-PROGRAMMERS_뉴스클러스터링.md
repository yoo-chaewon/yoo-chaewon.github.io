---

title:  "[ALGORITHM]PROGRAMMERS-뉴스클러스터링"

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

여러 언론사에서 쏟아지는 뉴스, 특히 속보성 뉴스를 보면 비슷비슷한 제목의 기사가 많아 정작 필요한 기사를 찾기가 어렵다. Daum 뉴스의 개발 업무를 맡게 된 신입사원 튜브는 사용자들이 편리하게 다양한 뉴스를 찾아볼 수 있도록 문제점을 개선하는 업무를 맡게 되었다.

개발의 방향을 잡기 위해 튜브는 우선 최근 화제가 되고 있는 카카오 신입 개발자 공채 관련 기사를 검색해보았다.

- 카카오 첫 공채..'블라인드' 방식 채용
- 카카오, 합병 후 첫 공채.. 블라인드 전형으로 개발자 채용
- 카카오, 블라인드 전형으로 신입 개발자 공채
- 카카오 공채, 신입 개발자 코딩 능력만 본다
- 카카오, 신입 공채.. 코딩 실력만 본다
- 카카오 코딩 능력만으로 2018 신입 개발자 뽑는다

기사의 제목을 기준으로 블라인드 전형에 주목하는 기사와 코딩 테스트에 주목하는 기사로 나뉘는 걸 발견했다. 튜브는 이들을 각각 묶어서 보여주면 카카오 공채 관련 기사를 찾아보는 사용자에게 유용할 듯싶었다.

유사한 기사를 묶는 기준을 정하기 위해서 논문과 자료를 조사하던 튜브는 자카드 유사도라는 방법을 찾아냈다.

자카드 유사도는 집합 간의 유사도를 검사하는 여러 방법 중의 하나로 알려져 있다. 두 집합 `A`, `B` 사이의 자카드 유사도 `J(A, B)`는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의된다.

예를 들어 집합 `A` = {1, 2, 3}, 집합 `B` = {2, 3, 4}라고 할 때, 교집합 `A ∩ B` = {2, 3}, 합집합 `A ∪ B` = {1, 2, 3, 4}이 되므로, 집합 `A`, `B` 사이의 자카드 유사도 `J(A, B)` = 2/4 = 0.5가 된다. 집합 A와 집합 B가 모두 공집합일 경우에는 나눗셈이 정의되지 않으니 따로 `J(A, B)` = 1로 정의한다.

자카드 유사도는 원소의 중복을 허용하는 다중집합에 대해서 확장할 수 있다. 다중집합 `A`는 원소 1을 3개 가지고 있고, 다중집합 `B`는 원소 1을 5개 가지고 있다고 하자. 이 다중집합의 교집합 `A ∩ B`는 원소 1을 min(3, 5)인 3개, 합집합 `A ∪ B`는 원소 1을 max(3, 5)인 5개 가지게 된다. 다중집합 `A` = {1, 1, 2, 2, 3}, 다중집합 `B` = {1, 2, 2, 4, 5}라고 하면, 교집합 `A ∩ B` = {1, 2, 2}, 합집합 `A ∪ B` = {1, 1, 2, 2, 3, 4, 5}가 되므로, 자카드 유사도 `J(A, B)` = 3/7, 약 0.42가 된다.

이를 이용하여 문자열 사이의 유사도를 계산하는데 이용할 수 있다. 문자열 FRANCE와 FRENCH가 주어졌을 때, 이를 두 글자씩 끊어서 다중집합을 만들 수 있다. 각각 {FR, RA, AN, NC, CE}, {FR, RE, EN, NC, CH}가 되며, 교집합은 {FR, NC}, 합집합은 {FR, RA, AN, NC, CE, RE, EN, CH}가 되므로, 두 문자열 사이의 자카드 유사도 `J("FRANCE", "FRENCH")` = 2/8 = 0.25가 된다.

### 입력 형식

- 입력으로는 `str1`과 `str2`의 두 문자열이 들어온다. 각 문자열의 길이는 2 이상, 1,000 이하이다.
- 입력으로 들어온 문자열은 두 글자씩 끊어서 다중집합의 원소로 만든다. 이때 영문자로 된 글자 쌍만 유효하고, 기타 공백이나 숫자, 특수 문자가 들어있는 경우는 그 글자 쌍을 버린다. 예를 들어 ab+가 입력으로 들어오면, ab만 다중집합의 원소로 삼고, b+는 버린다.
- 다중집합 원소 사이를 비교할 때, 대문자와 소문자의 차이는 무시한다. AB와 Ab, ab는 같은 원소로 취급한다.

### 출력 형식

입력으로 들어온 두 문자열의 자카드 유사도를 출력한다. 유사도 값은 0에서 1 사이의 실수이므로, 이를 다루기 쉽도록 65536을 곱한 후에 소수점 아래를 버리고 정수부만 출력한다.

### 예제 입출력

| str1      | str2        | answer |
| --------- | ----------- | ------ |
| FRANCE    | french      | 16384  |
| handshake | shake hands | 65536  |
| aa1+aa2   | AAAA12      | 43690  |
| E=M*C^2   | e=m*c^2     | 65536  |



## 코드

```java
import java.util.HashMap;

class Solution {
    static double VALUE = 65536;
    public static int solution(String str1, String str2) {
        HashMap<String, Integer> str1Hash = MakeHash(str1);
        HashMap<String, Integer> str2Hash = MakeHash(str2);

        if (str1Hash.size() == 0 && str2Hash.size() == 0) return (int)VALUE;
        int sum = 0;
        int intersection = 0;

        for (String key : str1Hash.keySet()){
            if (str2Hash.containsKey(key)){
                sum += Math.max(str1Hash.get(key) , str2Hash.get(key));
                continue;
            }
            sum+=str1Hash.get(key);
        }

        for (String key : str2Hash.keySet()){
            if (str1Hash.containsKey(key)){
                intersection += Math.min(str1Hash.get(key) , str2Hash.get(key));
                continue;
            }
            sum += str2Hash.get(key);
        }

        double temp  = (double)intersection/ (double)sum  * VALUE;
        return (int) temp;
    }

    public static HashMap<String, Integer> MakeHash(String str){
        str = str.toLowerCase();
        HashMap<String, Integer> hash = new HashMap<>();
        for (int i = 0; i < str.length() - 1; i++) {
            if (str.charAt(i) >= 'a' && str.charAt(i) <= 'z'
                    && str.charAt(i + 1) >= 'a' && str.charAt(i + 1) <= 'z') {
                String key = str.substring(i, i+2);
                hash.put(key, hash.getOrDefault(key, 0) + 1);
            }
        }
        return hash;
    }
}
```

- 그리고 str1, str2의 집합을 나타내기 위해 HashMap을 사용하였다.

  👉 일반 집합이라면 중복을 허용하지 않고 키만 저장하는 HashSet을 썻을것이다.

  👉 하지만,  `중복을 허용`하는 다중집합이기 때문에 HashMap의 key에는 문자열 자른것, Value는 갯수를 저장해 두었다.

-  대문자와 소문자의 차이는 무시한다 는 조건이 있기 때문에

  ```java
  str = str.toLowerCase();
  ```

  입력받은 문자를 모두 소문자로 변경해주었다.

- 다중 집합을 만드는 함수를 만들어주었다.

  ```java
  public static HashMap<String, Integer> MakeHash(String str){
    str = str.toLowerCase();
    HashMap<String, Integer> hash = new HashMap<>();
    for (int i = 0; i < str.length() - 1; i++) {
      if (str.charAt(i) >= 'a' && str.charAt(i) <= 'z'
          && str.charAt(i + 1) >= 'a' && str.charAt(i + 1) <= 'z') {
        String key = str.substring(i, i+2);
        hash.put(key, hash.getOrDefault(key, 0) + 1);
      }
    }
    return hash;
  }
  ```

  -  이때 영문자로 된 글자 쌍만 유효하고, 기타 공백이나 숫자, 특수 문자가 들어있는 경우는 그 글자 쌍을 버린다. 

    👉 str.charAt(i) >= 'a' && str.charAt(i) <= 'z'

    이렇게 문자열의 범위를 주었다.

  - 키가 기존에 존재 할 시 value에 +1를 해주도록 하였다.

- 각각의 다중집합을 return받고, 합집합과 교집합을 세어준다.

- 합집합의 경우, 

  > Str1 = {aa, aa, aaa}
  >
  > str2 = {aa, aa}
  >
  > 인 경우 {aaa, aaa, aaa} 가 된다.

  따라서, str1 와 str2가 동일한 키를 갖고 있을 경우, 더 많은 키를 가진 것을 sum+= 해준다.

  ```java
  for (String key : str1Hash.keySet()){
    if (str2Hash.containsKey(key)){
      sum += Math.max(str1Hash.get(key) , str2Hash.get(key));//더 많은 키 기준,
      continue;
    }
    sum+=str1Hash.get(key);//동일한 키가 아닐 경우 그냥 key의 value를 더해준다.
  }
  
  for (String key : str2Hash.keySet()){
    if (str1Hash.containsKey(key)){
      intersection += Math.min(str1Hash.get(key) , str2Hash.get(key));
      //교집합일 경우 동일한 키 중 더 작은 값을 intersection에 더해준다.
      continue;
    }
    sum += str2Hash.get(key);
  }
  ```

- 마지막 계산

  ```java
  double temp  = (double)intersection/ (double)sum  * VALUE;
  return (int) temp;
  ```

  여기서 나누기의 값이 int면 0.25면 0 이 나오기 때문에 계산이 잘 안된다. 따라서 실수형을 사용할 수 있는 변수를사용하도록 한다.

  

##### 기록 

> 이 문제를 풀며 문제를 항상 제발 제대로 읽자고 다짐했다. 너무 길어서 일단 풀고서 조건 추가하자 하다가 왜 답이 안나오지 싶었던 문제다. 제발 문제좀 읽자 ~
>
> 문제: https://programmers.co.kr/learn/courses/30/lessons/17677
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/4.KAKAO/Q_뉴스클러스터링.java