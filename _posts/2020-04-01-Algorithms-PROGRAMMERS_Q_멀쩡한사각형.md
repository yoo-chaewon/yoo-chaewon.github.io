---

title:  "[ALGORITHM]PROGRAMMERS-Q_멀쩡한사각형"

excerpt: "#PROGRAMMERS #Summer/Winter Coding"



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

가로 길이가 Wcm, 세로 길이가 Hcm인 직사각형 종이가 있습니다. 종이에는 가로, 세로 방향과 평행하게 격자 형태로 선이 그어져 있으며, 모든 격자칸은 1cm x 1cm 크기입니다. 이 종이를 격자 선을 따라 1cm × 1cm의 정사각형으로 잘라 사용할 예정이었는데, 누군가가 이 종이를 대각선 꼭지점 2개를 잇는 방향으로 잘라 놓았습니다. 그러므로 현재 직사각형 종이는 크기가 같은 직각삼각형 2개로 나누어진 상태입니다. 새로운 종이를 구할 수 없는 상태이기 때문에, 이 종이에서 원래 종이의 가로, 세로 방향과 평행하게 1cm × 1cm로 잘라 사용할 수 있는 만큼만 사용하기로 하였습니다. 
가로의 길이 W와 세로의 길이 H가 주어질 때, 사용할 수 있는 정사각형의 개수를 구하는 solution 함수를 완성해 주세요.

##### 제한사항

- W, H : 1억 이하의 자연수

#### 입출력 예

| W    | H    | result |
| ---- | ---- | ------ |
| 8    | 12   | 80     |

##### 입출력 예 설명

입출력 예 #1
가로가 8, 세로가 12인 직사각형을 대각선 방향으로 자르면 총 16개 정사각형을 사용할 수 없게 됩니다. 원래 직사각형에서는 96개의 정사각형을 만들 수 있었으므로, 96 - 16 = 80 을 반환합니다.

![572957326.92.png](https://grepp-programmers.s3.amazonaws.com/files/production/ee895b2cd9/567420db-20f4-4064-afc3-af54c4a46016.png)



## 코드

```java
class Solution {
    public static void main(String[] args) {
        System.out.println(solution(8, 12));

    }

    public static long solution(int w, int h) {
        long wl = Long.parseLong(String.valueOf(w));
        long hl = Long.parseLong(String.valueOf(h));

        return wl * hl - (wl + hl - GCD(w, h));
    }

    public static long GCD(int w, int h) {
        long big = Math.max(w, h);
        long small = Math.min(w, h);

        while (small != 0) {
            long tmp = big % small;
            big = small;
            small = tmp;
        }
        return big;
    }
}
```

- 그냥 수학 경시대회 같은 문제다..몰라서 다른 사람 풀이 보고 했다.

- 이 문제는 **전체 사각형 크기**에서 **잘린사각형**들을 빼주면 된다.

  👉 여기서 잘린 사각형을 구하는게 문제다.

  - 위 예시에서 최대한 16이 나오도록 규칙을 찾아보았다. 

- 아무튼, 찾은 규칙(내가 찾은건 아니지만,)

  ```
  직사각형의 가로길이와 세로길이를 더해,
  여기서 최대공약수를 빼준다.
  즉, W*H의 사각형을 대각선으로 잘랐을 때 망가지게 되는 사각형: [W+H-최대공약수]
  ```

- 위를 구현해주기 위해 **최대공약수**를 구현해야 한다.

  ```java
   public static long GCD(int w, int h) {
     long big = Math.max(w, h);
     long small = Math.min(w, h);
     while (small != 0) {
       long tmp = big % small;
       big = small;
       small = tmp;
     }
     return big;
   }
  ```

  



##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/62048
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Q_멀쩡한%20사각형.java
>
> 참고 : https://taesan94.tistory.com/55