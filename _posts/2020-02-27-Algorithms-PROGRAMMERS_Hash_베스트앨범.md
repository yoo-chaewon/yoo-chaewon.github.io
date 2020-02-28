---

title:  "[ALGORITHM]PROGRAMMERS/Hash-베스트앨범"

excerpt: "#PROGRAMMERS #Hash"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- Hash

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

##### 입출력 예

| genres                                | plays                      | return       |
| ------------------------------------- | -------------------------- | ------------ |
| [classic, pop, classic, classic, pop] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

##### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.



## 코드

```java
import java.util.*;
class Solution {
    public int[] solution(String[] genres, int[] plays) {
        HashMap<String, Integer> albumSum = new HashMap<>();
        HashMap<String, List<Song>> hashMap = new HashMap<>();

        for (int i = 0; i < genres.length; i++){
            String curKey = genres[i];
            if (hashMap.containsKey(curKey)){
                List<Song> tmp = hashMap.get(curKey);
                tmp.add(new Song(i, plays[i]));
                hashMap.put(curKey, tmp);
                albumSum.put(curKey, albumSum.get(curKey) + plays[i]);
            }else {
                albumSum.put(curKey, plays[i]);
                List<Song> arr = new LinkedList<>();
                arr.add(new Song(i, plays[i]));
                hashMap.put(curKey, arr);
            }
        }

        LinkedList<String> keys = new LinkedList<>();
        keys.addAll(albumSum.keySet());
        Collections.sort(keys, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return albumSum.get(o2)-albumSum.get(o1);
            }
        });

        for (String key : keys){
            Collections.sort(hashMap.get(key), new Comparator<Song>() {
                @Override
                public int compare(Song o1, Song o2) {
                    return o2.count - o1.count;
                }
            });
        }

        ArrayList<Integer> result = new ArrayList<>();
        for (String key : keys){
            int size = Math.min(2, hashMap.get(key).size());
            for (int i = 0; i < size; i++){
                result.add(hashMap.get(key).get(i).num);
            }
        }

        int[] answer = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            answer[i] = result.get(i);
        }
        return answer;
    }
}

class Song{
    int num;
    int count;
    Song(int num, int count){
        this.num = num;
        this.count = count;
    }
}
```

주어진 조건은

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

이므로, 이 순대로 풀어보았다.

1. 속상노래가 가장 많이 재생된 장르를 먼저 수록한다. 👉 **장르별로 재생 수를 모두 합하여 장르 정렬하기**

   > 장르를 HashMap으로 만들어, 장르 재생 횟수를 value로 저장하였다.

   ```java
   HashMap<String, Integer> albumSum = new HashMap<>();
    for (int i = 0; i < genres.length; i++){
      String curKey = genres[i];
      if (hashMap.containsKey(curKey)){
        albumSum.put(curKey, albumSum.get(curKey) + plays[i]); 
        //해당 장르가 존재 할경우 그냥 + 재생횟수
      }else {
        hashMap.put(curKey, arr); //없을 경우, gener를 key로, 재생 횟수를 value로 
      }
    }
   ```

   문제의 입력을 사용할 경우, 

   > 결과,
   >
   > pop: 3100
   > classic: 1450

   가 된다. 그러면 이제, 이것을 재생 횟수 순으로 장르를 정렬해야 한다.

   `Comparator`을 이용하여 정렬해주었다.

   ```java
   LinkedList<String> keys = new LinkedList<>();
   keys.addAll(albumSum.keySet());
   Collections.sort(keys, new Comparator<String>() {
     @Override
     public int compare(String o1, String o2) {
       return albumSum.get(o2)-albumSum.get(o1);
     }
   });
   ```

   - 어떻게 해줘야하나 고민하였는데, 어차피 HashMap에는 순서가 존재하지 않기 때문에 key들을 LinkedList에 넣어주었다.
   - 다음 HashMap에서 재생 수를 가져와 Key들의 arr를 정렬해주었다.

   > 결과,
   >
   > pop
   > classic

2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다. 👉 **재생된 노래들 정렬 !**

   > 현재는
   >
   > pop: ( 1,600 ) ( 4,2500 )
   > classic: ( 0,500) ( 2,150 ) ( 3,800)
   >
   > 이런 형태로, 한 장르에 대해 배열로, (음악 번호, 음악 실행 재생 횟수) 로 저장되어 있다.
   >
   > 이제 이 배열들 정렬 해야한다.

   ```java
   for (String key : keys){
     Collections.sort(hashMap.get(key), new Comparator<Song>() {
       @Override
       public int compare(Song o1, Song o2) {
         return o2.count - o1.count;
       }
     });
   }
   ```

   - hashMap.get(key) 의 배열을 정렬해준다. 이때 정렬 기준은 count의 내림차순 (큰 것부터) 정렬해준다.

3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다. 👉 **위 정렬 대로 2개씩 노래를 뽑는다.**

   > 여기서,  **재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록** 이라는 조건은, 입력 받으면서 노래 순으로 정렬이 이미 먼저 이루어져있기 때문에, (List는 순서를 갖음) 상관 없이 재생횟수로 정렬을 하면 저 조건에 이미 만족한다.

   ```java
   ArrayList<Integer> result = new ArrayList<>();
   for (String key : keys){
     int size = Math.min(2, hashMap.get(key).size());
     for (int i = 0; i < size; i++){
       result.add(hashMap.get(key).get(i).num);
     }
   }
   ```

   노래를 2개씩 뽑기 때문에, 2개 보다 적을 수 있기 때문에 Math.min(2, hashMap.get(key).size()) 을 해주어 더 오류를 막았다.



##### 느낀점

> 코로나 조심😷
>
> 문제 : https://programmers.co.kr/learn/courses/30/lessons/42579
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/with%20Study/Hash_베스트앨범.java
>
