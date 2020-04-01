---

title:  "[ALGORITHM]PROGRAMMERS-Q_종이접기"

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

직사각형 종이를 n번 접으려고 합니다. 이때, 항상 오른쪽 절반을 왼쪽으로 접어 나갑니다. 다음은 n = 2인 경우의 예시입니다.

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_390/v1500952547/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B01_swcvrz.png)

먼저 오른쪽 절반을 왼쪽으로 접습니다.

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_195/v1500952547/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B02_e49oe3.png)

다시 오른쪽 절반을 왼쪽으로 접습니다.

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_95/v1500952178/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B03_nqdurc.png)

종이를 모두 접은 후에는 종이를 전부 펼칩니다. 종이를 펼칠 때는 종이를 접은 방법의 역순으로 펼쳐서 처음 놓여있던 때와 같은 상태가 되도록 합니다. 위와 같이 두 번 접은 후 종이를 펼치면 아래 그림과 같이 종이에 접은 흔적이 생기게 됩니다. 

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_390/v1500952178/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B04_qxfoxr.png)

위 그림에서 ∨ 모양이 생긴 부분은 점선(0)으로, ∧ 모양이 생긴 부분은 실선(1)으로 표시했습니다. 

종이를 접은 횟수 n이 매개변수로 주어질 때, 종이를 절반씩 n번 접은 후 모두 펼쳤을 때 생기는 접힌 부분의 모양을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- 종이를 접는 횟수 n은 1 이상 20 이하의 자연수입니다.
- 종이를 접었다 편 후 생긴 굴곡이 ∨ 모양이면 0, ∧ 모양이면 1로 나타냅니다.
- 가장 왼쪽의 굴곡 모양부터 순서대로 배열에 담아 return 해주세요.

------

##### 입출력 예

| n    | result          |
| ---- | --------------- |
| 1    | [0]             |
| 2    | [0,0,1]         |
| 3    | [0,0,1,0,0,1,1] |

##### 입출력 예 설명

입출력 예 #1
종이의 오른쪽 절반을 왼쪽으로 한번 접었다 펴면 아래 그림과 같이 굴곡이 생깁니다.

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_390/v1500952178/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B05_fpgzni.png)

따라서 [0]을 return 하면 됩니다.

입출력 예 #2
문제의 예시와 같습니다.

입출력 예 #3
종이를 절반씩 세 번 접은 후 다시 펼치면 아래 그림과 같이 굴곡이 생깁니다. 

![image](https://res.cloudinary.com/dpxurmkij/image/upload/c_scale,w_390/v1500952178/%EC%A2%85%EC%9D%B4%EC%A0%91%EA%B8%B06_sbn7li.png)

따라서 [0,0,1,0,0,1,1]을 return 하면 됩니다.

## 코드

```java
class Solution {
    static int[] answer;
    public static int[] solution(int n) {
        answer = new int[(int) (Math.pow(2, n) - 1)];
        if(n != 1){
            make((int) (Math.pow(2, n) - 1), 1);
        }
        return answer;
    }

    public static void make(int size, int cur) {
        if (size / 2 == cur) {
            for (int i = 1; i <= cur; i++) {
                answer[cur+i] = answer[cur-i] == 1? 0: 1;
            }
            return;
        }

        if (cur == 1) {
            answer[cur - 1] = 0;
            answer[cur + 1] = 1;
        }else {
            for (int i = 1; i <= cur; i++) {
                answer[cur+i] = answer[cur-i] == 1? 0: 1;
            }
        }
        make(size, cur * 2 + 1);
    }
}
```

- 우선 **길이의 규칙**을 찾아야 한다.

  ```
  n은 접은 횟수로,
  n = 1 👉 1
  n = 2 👉 3
  n = 3 👉 7
  n = 4 👉 15
  즉, 2의 n승 -1 
  ```

  ```java
  answer = new int[(int) (Math.pow(2, n) - 1)];
  ```

- 다음 규칙을 찾으면, 한 지점을 중심으로 대칭인걸 볼 수 있다.

  ```
  0010011 을 보면,
  7/2 = 3 을 중심으로 0->1, 1->0 으로 대칭이 된다.
  ```

  > 이 규칙을 찾고 DP로 풀어야 하나? 생각했는데, 구현할 방법이 생각나지 않아
  >
  > DFS로 했다. 쪼개서 합치는 느낌으로 했다.

- 코드를 보면,

  ```java
  public static void make(int size, int cur) {
    if (size / 2 == cur) {//중간 지점일 경우 끝
      for (int i = 1; i <= cur; i++) {
        answer[cur+i] = answer[cur-i] == 1? 0: 1;
      }
      return;
    }
    
    if (cur == 1) {//1인 경우 가장 많이 쪼갠 것이기 때문에 앞뒤로 01을 해준다. 즉, 1인 경우는 001이 된다.
      answer[cur - 1] = 0;
      answer[cur + 1] = 1;
    }else {//cur을 중심으로 대칭을 해준다.
      for (int i = 1; i <= cur; i++) {
        answer[cur+i] = answer[cur-i] == 1? 0: 1;
      }
    }
    make(size, cur * 2 + 1);
  }
  ```

  즉, 점점 값을 키워가면서 대칭을 붙여주는 것이다.







##### 기록 

> 문제: https://programmers.co.kr/learn/courses/30/lessons/62049
>
> 저장소 :https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Q_종이접기.java
>