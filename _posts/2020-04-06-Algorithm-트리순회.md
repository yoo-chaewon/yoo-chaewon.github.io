---

title:  "[Algorithm]트리순회"

excerpt: "#Java #트리순회 "



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





# 트리순회(전위 순회, 중위 순회, 후위 순회)

![img](https://www.acmicpc.net/JudgeOnline/upload/201007/trtr.png)

> 전위 순회(PreOrder): ROOT - L - R
>
> 중위 순회(InOrder): L - ROOT - R
>
> 후위 순회(PostOrder): L - R - ROOT

👉 즉, ROOT를 언제 순회하냐에 따라서 달라짐 !



#### 위의 예제에서 순회의 결과는

- 전위순회: ABDCEFG
- 중위순회: DBAECFG
- 후위순회: DBEGFCA



### 🔗 문제

[트리순회](https://www.acmicpc.net/problem/1991)

