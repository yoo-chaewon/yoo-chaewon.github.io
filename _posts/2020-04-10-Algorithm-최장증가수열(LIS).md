---

title:  "[Algorithm]최장증가수열(Longest Increaing Subsequence)"

excerpt: "#Java #최장증가수열 #LIS "



categories:

- Algorithm

tags:

- Algorithm

- Java

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



최장증가수열은 가장 긴 증가하는 부분 수열을 찾는 것이다.

예를 들어 수열 A = {10,20,10,30,20,50}인 경우 

가장 긴 증가하는 부분 수열은  A = {**10**,**20**,10,**30**,20,**50**}이고, 길이는 4이다.

👉 LIS는 앞에서부터 뒤로 숫자를 선택하며 부분 수열을 구성해 나갈 때 증가하는 순서대로 숫자를 고르면서 고른 부분 수열의 길이가 최대 길이가 되도록 숫자를 선택하는 것이다.



O(N^2)로 구할 수 있지만, DP를 사용하면 O(NlogN)



```java
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int size = scanner.nextInt();
        int[] arr = new int[size];
        int[] answer = new int[size];

        for (int i = 0; i < size; i++) {
            arr[i] = scanner.nextInt();
        }

        int index = 0;
        answer[0] = arr[0];
        for (int i = 1 ; i < size; i++){
            if (answer[index] < arr[i]){
                answer[++index] = arr[i];
            }else{
                int temp = Arrays.binarySearch(answer, 0, index, arr[i]);
                if (temp < 0){
                    answer[temp * -1 -1] = arr[i];
                }else {
                    answer[temp] = arr[i];
                }
            }
        }


        System.out.println(index+1);

    }
}
```





#### 🔗 연관 문제

- [가장 긴 증가하는 부분 수열](https://www.acmicpc.net/problem/11053)