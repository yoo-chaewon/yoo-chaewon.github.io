---

title:  "[ALGORITHM]BOJ3033_가장긴문자열"

excerpt: "#BOJ #Hash"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- Hash

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

상근이는 꿈에서 길이가 L인 문자열을 외웠다.

꿈에서 깬 상근이는 이 문자열을 종이에 적었다. 종이를 적던 중에 어떤 문자열은 두 번 이상 등장하는 것 같은 느낌을 받았다.

문자열이 주어졌을 때, 두 번 이상 등장한 부분 문자열 중 가장 길이가 긴 것을 찾는 프로그램을 작성하시오. (부분문자열은 겹쳐서 등장할 수도 있다)

### 입력

첫째 줄에 L이 주어진다. (1 ≤ L ≤ 200,000) 다음 줄에는 길이가 L이면서 알파벳 소문자로 이루어진 문자열이 주어진다.

### 출력

첫째 줄에 두 번 이상 등장하는 부분 문자열 중 길이가 가장 긴 것의 길이를 출력한다. 만약 그러한 문자열이 없을 때는 0을 출력한다.



### 예제 입력 1

```
11
sabcabcfabc
```

### 예제 출력 1

```
3
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int L = Integer.parseInt(bufferedReader.readLine());
        String str = bufferedReader.readLine();
        int size = L / 2;


        while (size >1){
            for (int i = 0; i < L - size; i++) {
                String cur = str.substring(i, i + size);

                if (str.substring(i+size, str.length()).contains(cur)){
                    System.out.println(size);
                    return;
                }
            }
            size--;
        }
    }
}
```

- **시간 초과**가 난 코드이다.
- 가장 긴 문자열부터 비교하면서 포함되면 끝나기로 했다.
- 인풋의 범위가 (1 ≤ L ≤ 200,000)  이기 대문에 어쨋든 모든 경우를 다 보면 시간 초과가 당연한 결과기도 하다.
- 머리를 좀 더 쓰자...



```

```



##### 느낀점

> 문제 : https://www.acmicpc.net/problem/3033
>
> 저장소 : 

