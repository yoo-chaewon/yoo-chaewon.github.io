---

title:  "[Algorithm]최소 신장 트리(MST, Minimum Spanning Tree)"

excerpt: "#Java #MST #최소 신장 트리"



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





# [알고리즘] 최소 신장 트리(MST, Minimum Spanning Tree)

> Spanning Tree
>
> MST



## Spanning Tree란

##### 그래프 내의 모든 정점을 포함하는 트리

- Spanning Tree = 신장 트리 = 스패닝 트리
- **Spanning Tree**는 **그래프의 최소 연결 부분** 그래프이다.
  - 최소 연결 👉 `간선 수가 가장 최소`
  - n개의 정점을 가지는 그래프의 최소 간선의 수는(n-1)개이고, (n-1)개의 간선으로 연결되어 있으면 필연적으로 **트리**형태가 되고 이것이 바로 Spanning Tree가 된다.
  - 즉, 그래프에서 일부 간선을 선택하여 만든 트리



#### Spanning Tree의 특징

- DFS/BFS를 이용하여 그래프의 신장 트리를 찾을 수 있다.
  - 탐색 도중에 사용된 간선만 모으면 만들 수 있다.
- 하나의 그래프에 많은 신장트리가 존재할 수 있다.
- Spanning Tree는 트리의 특수한 형태이므로 **모든 정점들이 연결** 되어 있어야 하고, **`사이클을 포함해서는 안된다`**.
- 따라서 Spanning Tree는 그래프에 있는 **n개의 정점을 정확히 (n-1)개의 간선으로 연결**한다.



#### Spanning Tree의 사용 사례

- 통신 네트워크 구축
  - 회사 내에서 모든 전화기를 가장 적은 수의 케이블을 사용하여 연결 하는 경우
  - n개의 위치를 연결하는 통신 네트워크를 최소 링크(간선)을 이용하여 구축하고자 하는 경우, 최소 링크의 수는 (n-1)개가 되고, 따라서 Spanning Tree가 가능해진다.



## 🌲 최소 신장 트리(MST)란

- Spanning Tree 중에서 사용된 간선들의 **`가중치 합이 최소`**인 트리

  > MST = Minimum Spanning Tree = 최소 신장 트리
  >
  > 각 간선의 가중치가 동일하지 않을 때 단순히 가장 적은 간선을 사용한다고 해서 최소 비용이 얻어지는 것이 아니다.
  >
  > MST는 간선에 가중치를 고려하여 최소 비용의 Spanning Tree를 선택하는 것을 말한다.

- 네트워크(가중치 간선에 할당한 그래프)에 있는 모든 정점들을 가장 적은 수의 간선과 비용으로 연결하는 것이다.



#### MST의 특징

1. 간선의 가중치의 합이 최소여야 한다.
2. n개의 정점을 가지는 그래프
3. 사이클이 포함되어서는 안된다.



#### MST의 사용 사례

통신망, 도로망, 유통만에서 **길이**, 구축 **비용**, 전송**시간** 등을 최소로 구축하는 경우



## 🤓 MST의 구현 방법

### Kruskal MST 알고리즘

탐욕적인 방법(greedy method)을 이용하여 네트워크(가중치를 간선에 할당한 그래프)의 모든 정점을 최소 비용으로 연결하는 최적 해답을 구하는 것.

- MST가 최소 비용의 간선으로 구성됨/ 사이클이 포함되지 않음

  👉 각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택한다.

- 간선 선택을 기반으로 하는 알고리즘이다.

- 이전 단계에서 만들어진 신장트리와 상관없이 무조건 최소 간선만을 선택하는 방법이다.



##### 과정

> 1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.
> 2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.
>    - 가장 낮은 가중치를 먼저 선택한다.
>    - 사이클을 형성하는 간선을 제외한다.
>      - Union-Find 알고리즘을 통해 찾는다
> 3. 해당 간선을 현재의 MST(최소 비용 신장 트리)의 집합에 추가한다.



#### 🔗 관련 문제

- [최소 스패닝 트리](https://www.acmicpc.net/problem/1197)
- [네트워크 연결](https://www.acmicpc.net/problem/1922)