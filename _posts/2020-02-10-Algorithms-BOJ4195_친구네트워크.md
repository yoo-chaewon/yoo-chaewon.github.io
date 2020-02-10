---

title:  "[ALGORITHM]BOJ4195_친구네트워크"

excerpt: "#BOJ #Hash #디스조인트셋 #disjonit-set"



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

민혁이는 소셜 네트워크 사이트에서 친구를 만드는 것을 좋아하는 친구이다. 우표를 모으는 취미가 있듯이, 민혁이는 소셜 네트워크 사이트에서 친구를 모으는 것이 취미이다.

어떤 사이트의 친구 관계가 생긴 순서대로 주어졌을 때, 두 사람의 친구 네트워크에 몇 명이 있는지 구하는 프로그램을 작성하시오.

친구 네트워크란 친구 관계만으로 이동할 수 있는 사이를 말한다.

### 입력

첫째 줄에 테스트 케이스의 개수가 주어진다. 각 테스트 케이스의 첫째 줄에는 친구 관계의 수 F가 주어지며, 이 값은 100,000을 넘지 않는다. 다음 F개의 줄에는 친구 관계가 생긴 순서대로 주어진다. 친구 관계는 두 사용자의 아이디로 이루어져 있으며, 알파벳 대문자 또는 소문자로만 이루어진 길이 20 이하의 문자열이다.

### 출력

친구 관계가 생길 때마다, 두 사람의 친구 네트워크에 몇 명이 있는지 구하는 프로그램을 작성하시오.



### 예제 입력 1 

```
2
3
Fred Barney
Barney Betty
Betty Wilma
3
Fred Barney
Betty Wilma
Barney Betty
```

### 예제 출력 1 

```
2
3
4
2
2
4
```

##### 

## 접근방식

친구 네트워크란 친구 관계만으로 이동할 수 있는 사이를 말한다.

- 노드: 친구
- 간선: 친구관계

👉 서로 친구관계가 아닐 경우 각각 다른 트리로 구성되어 있으며, 친구 관계로 연결될 경우 하나의 트리로 만들어짐.

#### 입력

```
Fred Barney // 하나의 트리
Betty Wilma // 하나의 트리
Barney Betty //두 트리간 연결
```

이 문제는 기본적인 **디스조인트-셋**을 이용할 수 있다.



### 디스조인트-셋(disjoint-set)

- 유니온-파인드(union-find) / 머지-파인드-셋(merge-find-set)

- 디스조인트 셋은 서로소 집합

  👉 공통된 원소가 없는 집합으로써, 교집합이 공집합인 집합이라고 보면 됨.

> 많은 **상호 배타적** 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조
>
> 👉 상호배타적이란, AB사건이 존재한다면, AB는 동시에 일어나지 않고, 둘 중 하나만 일어나야만 함.

- 구현
  - 유니온(union): 두 개의 집합을 하나의 집합으로 합친다.
  - 파인드(find): 어떤 원소가 주어졌을 때 이 원소가 속한 집합을 반환한다. 파인드는 일반적으로 어떤 원소가 속한 집합을 대표하는 원소를 반환하는데, 이를 위하여 어떤 원소와 각 대표 원소들 간의 파인트 결과를 비교하여 같은 집합임을 판단한다.



위 자료구조를 이용하여 문제를 접근하면,

```java
int parent[]
// 자신 부모의 root node 번호를 저장하고 있다. 처음 시작해줄때는 자기 자신이 root노드이므로 자기 자신의 인덱스로 초기화해 준다.
int level[]
// level을 저장하는 변수, 자식 노드보다 parent노드가 +1 높음.
int relation[]
// 각 노드마다 관계를 몇개 갖고 있는지 저장하는 변수/ 부모노드는 자식노드꺼 까지 모든 관계를 갖고 있다.
HashMap<String, Integer> hash = new HashMap<>()
// 친구를 hash로 index를 주어 저장.
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.HashMap;

public class Main {
    static int MAX_SIZE = 200001;
    static int index = 1;
    static int[] parent;
    static int[] level;
    static int[] relation;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int testCase = Integer.parseInt(bufferedReader.readLine());

        while (testCase-- > 0) {
            StringBuilder stringBuilder = new StringBuilder();
            int f = Integer.parseInt(bufferedReader.readLine());
            HashMap<String, Integer> hash = new HashMap<>();
            parent = new int[MAX_SIZE];
            level = new int[MAX_SIZE];
            relation = new int[MAX_SIZE];

            for (int i = 1; i < MAX_SIZE; i++) {
                parent[i] = i;
                relation[i] = 1;
            }

            while (f-- > 0) {
                String[] input = bufferedReader.readLine().split(" ");
                String a = input[0];
                String b = input[1];

                if (!hash.containsKey(a)){
                    hash.put(a, index++);
                }

                if (!hash.containsKey(b)){
                    hash.put(b, index++);
                }
                int aIndex = hash.get(a);
                int bIndex = hash.get(b);

                Merge(aIndex, bIndex);
            }
        }
    }

    public static void Merge(int a, int b){
        a = find(a);
        b = find(b);

        if (a == b){
            System.out.println(relation[a]);
            return;
        }

        //b가 더 높다고 가정
        if (level[a] > level[b]){
            int tmp = a;
            a = b;
            b = tmp;
        }

        parent[a] = b;
        relation[b] += relation[a];
        System.out.println(relation[b]);

        if (level[a] == level[b]){
            level[b]++;
        }
    }

    //parent node 찾는 메소드
    public static int find(int u){
        if (u == parent[u]){
            return u;
        }
        return parent[u] = find(parent[u]);
    }
}
```



##### 저장소

> 문제 : https://www.acmicpc.net/problem/4195
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_4195_친구네트워크.java



##### 디스조인트-셋 문제 List

- 친구네트워크
- 방청소
- 집합의 표현
- 공항

