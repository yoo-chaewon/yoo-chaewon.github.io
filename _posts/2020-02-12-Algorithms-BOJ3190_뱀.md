---

title:  "[ALGORITHM]BOJ3190-뱀"

excerpt: "#BOJ #SW #삼성코테"



categories:

- ALGORITHM

tags:

- ALGORITHM

- 삼성코테

- BOJ

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

 'Dummy' 라는 도스게임이 있다. 이 게임에는 뱀이 나와서 기어다니는데, 사과를 먹으면 뱀 길이가 늘어난다. 뱀이 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.

게임은 NxN 정사각 보드위에서 진행되고, 몇몇 칸에는 사과가 놓여져 있다. 보드의 상하좌우 끝에 벽이 있다. 게임이 시작할때 뱀은 맨위 맨좌측에 위치하고 뱀의 길이는 1 이다. 뱀은 처음에 오른쪽을 향한다.

뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.

- 먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.
- 만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.
- 만약 이동한 칸에 사과가 없다면, 몸길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.

사과의 위치와 뱀의 이동경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하라.

### 입력

첫째 줄에 보드의 크기 N이 주어진다. (2 ≤ N ≤ 100) 다음 줄에 사과의 개수 K가 주어진다. (0 ≤ K ≤ 100)

다음 K개의 줄에는 사과의 위치가 주어지는데, 첫 번째 정수는 행, 두 번째 정수는 열 위치를 의미한다. 사과의 위치는 모두 다르며, 맨 위 맨 좌측 (1행 1열) 에는 사과가 없다.

다음 줄에는 뱀의 방향 변환 횟수 L 이 주어진다. (1 ≤ L ≤ 100)

다음 L개의 줄에는 뱀의 방향 변환 정보가 주어지는데,  정수 X와 문자 C로 이루어져 있으며. 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L') 또는 오른쪽(C가 'D')로 90도 방향을 회전시킨다는 뜻이다. X는 10,000 이하의 양의 정수이며, 방향 전환 정보는 X가 증가하는 순으로 주어진다.

### 출력

첫째 줄에 게임이 몇 초에 끝나는지 출력한다.



### 예제 입력 1

```
6
3
3 4
2 5
5 3
3
3 D
15 L
17 D
```

### 예제 출력 1

```
9
```

### 예제 입력 2

```
10
4
1 2
1 3
1 4
1 5
4
8 D
10 D
11 D
13 L
```

### 예제 출력 2

```
21
```

### 예제 입력 3

```
10
5
1 5
1 3
1 2
1 6
1 7
4
8 D
10 D
11 D
13 L
```

### 예제 출력 3

```
13
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.HashMap;
import java.util.LinkedList;

public class Main {
    static int N;
    static int[] dx = {0, 1, 0, -1}; // 북 , 동 , 남 , 서
    static int[] dy = {-1, 0, 1, 0};
    static int[][] map;
    static int curDir = 1; // 동쪽 시작
    static int time = 0;
    static LinkedList<Point> snake;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(bufferedReader.readLine()); // 보드 크기
        int K = Integer.parseInt(bufferedReader.readLine()); // 사과 갯수
        map = new int[N][N];
        map[0][0] = 1;// 뱀 위치

        for (int i = 0; i < K; i++) {
            String[] apple = bufferedReader.readLine().split(" ");
            map[Integer.parseInt(apple[0]) - 1][Integer.parseInt(apple[1]) - 1] = 4; // 사과 위치
        }

        snake = new LinkedList<>();
        snake.add(new Point(0, 0));
      	HashMap<Integer, String> snakeMove = new HashMap<>();
        int L = Integer.parseInt(bufferedReader.readLine()); // 뱀의 방향 변환 횟수
        for (int i = 0; i < L; i++) {
            String[] input = bufferedReader.readLine().split(" ");
            snakeMove.put(Integer.parseInt(input[0]), input[1]);
        }

        while (true) {
            time++;
            Point head = snake.peekLast();
            int nextX = head.x + dx[curDir];
            int nextY = head.y + dy[curDir];
            if (nextX < 0 || nextY < 0 || nextX >= N || nextY >= N || map[nextY][nextX] == 1) break;

            if (map[nextY][nextX] != 4) {//사과
                Point tail = snake.poll();
                map[tail.y][tail.x] = 0;
            }
            map[nextY][nextX] = 1;
            snake.addLast(new Point(nextX, nextY));
            if (snakeMove.containsKey(time)) {
                if (snakeMove.get(time).equals("D")) curDir = (curDir + 1) % 4;
                else curDir = (curDir + 3) % 4;
            }
        }
        System.out.print(time);
    }
}

class Point {
    int x;
    int y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

```

- 이 문제는 head와 tail을 따로 생각하면 진짜 끝도 없다. 머리든 꼬리든 **뱀**이라고 생각하면 된다 !

- 수많은 약 5가지 방법 시도 후 나는 뱀을 디큐로 만들었다.

  ```java
  static LinkedList<Point> snake = new LinkedList<>();
  //뱀들의 위치들을 갖고 있다.
  // 꼬리ㅁㅁㅁㅁㅁㅁ머리 👈 이런 형태로 그렸다.
  ```
  
- 명령어들은 해쉬에 넣었다.

  ```java
  HashMap<Integer, String> snakeMove = new HashMap<>();
  int L = Integer.parseInt(bufferedReader.readLine()); // 뱀의 방향 변환 횟수
  for (int i = 0; i < L; i++) {
    String[] input = bufferedReader.readLine().split(" ");
    snakeMove.put(Integer.parseInt(input[0]), input[1]);
  }
  ```

  이것도 내가 헤매다가 해결한 문제 중 하나다. 처음에는 한 문장씩 받으면서 move()함수라는 것을 실행시켰다. 거기에서 for문을 돌으면서 time부터 input[0]까지 해주었는데 안에서 time++을 해주다보니 꼬이기 시작했다.

  그래서 한번에 명령어들을 받아두고 while문 안에서 if (snakeMove.containsKey(time)) 이렇게 할 수 있도록 하였다.

#### 이제 뱀을 움직여 보자

- 코드는 이러하다.

  ```java
  // 뱀 형태 : 꼬리ㅁㅁㅁㅁㅁㅁ머리 
  while (true) {
    time++;
    Point head = snake.peekLast(); //머리가 꼬리일 가능성이 있으므로, poll 대신 Peek을 한다.
    int nextX = head.x + dx[curDir]; //다음 갈 곳 위치 정해줌.
    int nextY = head.y + dy[curDir];
    if (nextX < 0 || nextY < 0 || nextX >= N || nextY >= N || map[nextY][nextX] == 1)
      break;
    
    //사과를 만나면 꼬리는 그대로 있어도 된다.
    //사과를 만나지 못하면 꼬리는 한칸 앞으로 가야한다. 즉, 마지막 위치를 0으로 바꿔주면 된다.
    if (map[nextY][nextX] != 4) {//사과X
      Point tail = snake.poll(); //꼬리 빼줌.
      map[tail.y][tail.x] = 0; //꼬리의 위치를 0으로 변경
    }
    
    map[nextY][nextX] = 1;//앞으로 전진한 머리를 지도에 1로 표시
    snake.addLast(new Point(nextX, nextY));
    
    //시간에 따라 명령어를 찾는다.
    if (snakeMove.containsKey(time)) {
      if (snakeMove.get(time).equals("D")) curDir = (curDir + 1) % 4; //오른쪽 보는 것
      else curDir = (curDir + 3) % 4;//왼쪽보는 것
    }
  }
  ```

  

## 느낀점

한 5시간 걸린 문제다. 그래도 포기하지 않았던 이유는 점점 답에 가까워졌기 때문이다. 그러다 한 10가지 방법은 도전해본거 같다. 그래도 풀어서 성취감은 있었지만 오늘 하루 뭐했나 생각들었다 ㅠ^ㅜ 풀고나니 그리 어렵지 않았던 문제라는 것을 알게 되어서 더욱 기운이 빠진다. 알고리즘 공부하면서 내가 생각하는 것은 구현해낼 수 있는 어느정도의 자신감을 갖고 있었는데 생각이 많아 지는 하루다. 더 열심히 해야겠다 😭

- 문제: https://www.acmicpc.net/problem/3190
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/SAMSUNG_SW/Q_3190_뱀.java