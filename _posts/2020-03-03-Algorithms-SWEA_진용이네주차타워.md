---

title:  "[ALGORITHM]SWEA_D3_9280.진용이네주차타워"

excerpt: "#SWEA #D3"



categories:

- ALGORITHM

tags:

- ALGORITHM

- SWEA


author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



**※ SW expert 아카데미의 문제를 무단 복제하는 것을 금지합니다.**

> 위 말에 쫄아서 문제는 쓰지 않는다.
>
> 👉 https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AW9j74FacD0DFAUY&categoryId=AW9j74FacD0DFAUY&categoryType=CODE

## 코드

```java
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int testCase = scanner.nextInt();

        for (int t = 0; t < testCase; t++) {
            int n = scanner.nextInt(); //parking
            int m = scanner.nextInt(); //car
            int[] parkFee = new int[n + 1];
            int[] carFee = new int[m + 1];
            for (int i = 1; i <= n; i++) parkFee[i] = scanner.nextInt();
            for (int i = 1; i <= m; i++) carFee[i] = scanner.nextInt();

            int answer = 0;
            int[] park = new int[n+1];
            int times = m * 2;
            Queue<Integer> queue = new LinkedList<>();
            while (times-->0){
                int cur = scanner.nextInt();
                if (cur > 0){
                    boolean flag = true;
                    for (int i = 1; i <= n; i++){
                        if (park[i] == 0){
                            flag = false;
                            park[i] = cur;
                            answer += parkFee[i] * carFee[cur];
                            break;
                        }
                    }
                    if(flag) queue.add(cur);
                }else {
                    int empty = 0;
                    for (int i = 1; i <= n; i++){
                        if (park[i] == cur * -1){
                            park[i] = 0;
                            empty = i;
                        }
                    }
                    if (!queue.isEmpty()){
                        int curCar = queue.poll();
                        park[empty] = curCar;
                        answer += parkFee[empty] * carFee[curCar];
                    }
                }
            }
            System.out.println("#"+(t+1) + " " + answer);
        }
    }
}
```

- 먼저 나는 주자장 위치별 요금이랑, 자동차별 요금을 Hash에 저장할까 하다가 배열에 했다.

  👉 어차피 1~n, 1~m까지 다 값이 존재하는 것이고, 배열이 hash역을 충분히 해줄수 있기 때문이다.

  ```java
   int n = scanner.nextInt(); //parking
  int m = scanner.nextInt(); //car
  int[] parkFee = new int[n + 1];
  int[] carFee = new int[m + 1];
  for (int i = 1; i <= n; i++) parkFee[i] = scanner.nextInt();
  for (int i = 1; i <= m; i++) carFee[i] = scanner.nextInt();
  ```

- 다음 모든 차들이 한번씩 들어오고, 한번씩 나가기 때문에 총 입력의 횟수는 **자동차수 * 2** 일 것이다.

  ```java
  int times = m * 2;
  while(times-->0){
    //여기서 코드 실행
  }
  ```

- park[] 배열을 만들어 현재 주차장 상황을 나타내는 배열을 만들었다.

- times 동안 자동차를 입력 받는다. 

  > 입력값이 0보다 클 경우는 들어오는 차에 해당한다.
  >
  > 따라서, 이 경우 주차장에 자리가 있으면 넣어주고,
  >
  > 없으면 대기 큐에 넣어준다.

  ```java
   if (cur > 0){
     boolean flag = true;
     for (int i = 1; i <= n; i++)
       if (park[i] == 0){ //주차창에 빈 공간이 존재 할 시
         park[i] = cur;
         answer += parkFee[i] * carFee[cur];
         flag = false;//주차 한 경우,
         break;
       }
   }
  //flag가 T : 주차를 못한경우, flat가 F: 주차를 한 경우다.
  // 주차를 못했을 경우 대기 큐에 넣어준다.
  if(flag) queue.add(cur);
  ```

  

  > 입력값이 0보다 작은 경우는 나가는 차에 해당한다.
  >
  > 이 경우, 차를 내보내고, 만약 큐에 대기차가 있다면 그 나간 자리에 넣어준다.

  ```java
  else {
    int empty = 0;
    for (int i = 1; i <= n; i++){
      if (park[i] == cur * -1){//나가는 차의 위치를 찾아서,
        park[i] = 0;					// 0으로 변경시켜준다.
        empty = i;//자동차 나간 위치를 저장한다.
      }
    }
    // 대기하고 있는 자동차가 존재할 경우,
    if (!queue.isEmpty()){
      int curCar = queue.poll();
      park[empty] = curCar;
      answer += parkFee[empty] * carFee[curCar];
    }
  }
  ```





처음부터 모든 것을 큐에 넣고 시작하니 큐를 두개 써야하나 고민했다. 근데 넣을 수 있는 것은 넣고, 대기할 경우만 큐에 넣어주니 간단하게 해결되었다. 

내 풀이에서 가장 아쉬운 경우는 배열을 매번 검사했다는 점이다. 빈 공간을 찾기 위해 처음부터 검사하고, 나가는 차를 찾기위해 처음부터 검사하고 👉 이부분을 나중에 수정해봐야겠다.

##### 비고

> 오랜만에 재밌는 문제 풀었다.
>
> 문제 : https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AW9j74FacD0DFAUY&categoryId=AW9j74FacD0DFAUY&categoryType=CODE
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/3.SWEA/D3_9280_진용이네주차타워.java
