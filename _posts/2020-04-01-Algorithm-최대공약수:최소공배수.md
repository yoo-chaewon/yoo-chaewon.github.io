---

title:  "[Algorithm]최대공약수 / 최소공배수"

excerpt: "=#Java "



categories:

- Data structure

tags:

- Algorithm

- Java

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---





# 최대공약수(GCD) & 최소공배수(LCM)

### GCD(최대공약수)

```java
//최대공약수
public static int GCD(int a, int b) {
  int big = a > b ? a : b;
  int small = a <= b ? a : b;
  
  while (small != 0) {
    int tmp = big % small;
    big = small;
    small = tmp;
  }
  return big;
}
```



### LCM(최소공배수)

```java
//최소공배수
public static int LCM(int a, int b) {
  return a*b/GCD(a, b);
}
```

