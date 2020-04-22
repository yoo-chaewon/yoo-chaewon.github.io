---

title:  "[Algorithm]플로이드와샬(Floyd Warshall)알고리즘"

excerpt: "#Java #플로이드-와샬 알고리즘 "



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



> - 다익스트라(Dijkstra): 하나의 정점에서 출발했을 때 다른 정점으로의 최단 경로를 구하는 알고리즘
>   - 일차원 배열, 특정한 노드에서
> - 플로이드-와샬(Floyd Warshall): '`모든 정점`'에서 '모든 정점'으로 `최단 경로`를 구하는 알고리즘
>   - 이차원 배열
>
> 👉 다익스트라 알고리즘은 가장 적은 비용을 하나씩 선택해야했다면, 플로이드 와샬 알고리즘은 기본적으로 '거쳐가는 정점'을 기준으로 알고리즘을 수행한다는 점에서 그 특징이 있음.



### 플로이드 와샬 알고리즘

- 그래프에서 모든 꼭지점 사이의 최단 경로의 거리를 구하는 알고리즘이다.
- 이 알고리즘을 이용하면 최단 경로를 알 수 있을 뿐만 아니라 연결되지 않은 경로(갈 수 없는 경로)도 알 수 있다.
- 연결되지 않았다는 것은 i,j의 순서를 알 수 없다는 뜻이다.