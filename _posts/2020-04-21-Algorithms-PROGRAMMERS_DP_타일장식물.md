---

title:  "[ALGORITHM]PROGRAMMERS-DP_타일장식물"

excerpt: "#PROGRAMMERS #DP"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- DP


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

대구 달성공원에 놀러 온 지수는 최근에 새로 만든 타일 장식물을 보게 되었다. 타일 장식물은 정사각형 타일을 붙여 만든 형태였는데, 한 변이 1인 정사각형 타일부터 시작하여 마치 앵무조개의 나선 모양처럼 점점 큰 타일을 붙인 형태였다. 타일 장식물의 일부를 그리면 다음과 같다.

![ㅅㅡㅋㅡㄹㅣㄴㅅㅑㅅ 2018-08-21 ㅇㅗㅎㅜ 5.11.26.png](https://grepp-programmers.s3.amazonaws.com/files/production/3e31bedd54/fcc48066-e72f-45c8-af03-e4360b58b589.png)

그림에서 타일에 적힌 수는 각 타일의 한 변의 길이를 나타낸다. 타일 장식물을 구성하는 정사각형 타일 한 변의 길이를 안쪽 타일부터 시작하여 차례로 적으면 다음과 같다.
[1, 1, 2, 3, 5, 8, .]
지수는 문득 이러한 타일들로 구성되는 큰 직사각형의 둘레가 궁금해졌다. 예를 들어, 처음 다섯 개의 타일이 구성하는 직사각형(위에서 빨간색으로 표시한 직사각형)의 둘레는 26이다.

타일의 개수 N이 주어질 때, N개의 타일로 구성된 직사각형의 둘레를 return 하도록 solution 함수를 작성하시오.

##### 제한 사항

- N은 1 이상 80 이하인 자연수이다.

##### 입출력 예

| N    | return |
| ---- | ------ |
| 5    | 26     |
| 6    | 42     |



## 코드

```java
class Solution {
    public static long solution(int N) {
        if (N == 1) return 4;
        long[] map = new long[N];
        map[0] = 1;
        map[1] = 1;
        for (int i = 2; i < N; i++) {
            map[i] = map[i - 1] + map[i - 2];
        }
        return map[N - 1] * 4 + map[N - 2] * 2;
    }
}
```

- DP문제 기피자 인데 이거는 피보나치 수라서 어렵지 않게 풀었다.
- 그리고 간단하게 둘레는 마지막 수 *4 + 그앞수 * 2 라는 것을 알아냈다.
- **⁉️ 문제에서 이미 return값이 Long으로 주어졌었다.**
  - 보니 80이 들어오면 마지막 값이 `23416728348467685` 이다.
  - map을 처음에 int로 만들어줘서 점수가 100이 안나왔었다 ! 주의하자 !

##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/43104
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/DP_타일장식물.java
>