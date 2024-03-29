---

title:  "[STUDY]좋은 git커밋 메시지를 작성하기 위한 7가지 약속"

excerpt: "#Git"


categories:

- GIT

tags:

- GIT

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



https://meetup.toast.com/posts/106 이 포스팅을 읽고, Git에 대해 회고 아닌 회고와 배운 점을 정리하고자 한다.

### Git 커밋 메세지를 잘 쓰려고 노력해야 하는 이유

- 더 좋은 커밋 로그 가독성
- 더 나은 협업과 리뷰 프로세스
- 더 쉬운 코드 유지보수



✔️인턴 시절 협업을 해보면서 커밋메세지의 중요성에 대해 알게 되었다. 처음에는 나도 커밋 메세지를 대충 작성했는데 나중에 필요한 코드를 찾을 경우 커밋메세지를 보고 찾는 경우가 많았다. 그 뒤로 커밋 메세지를 기능단위로 나누면서 해야겠다고 생각했다. 하지만 특정한 형식으로 커밋 메세지를 작성하지는 않았다. 이 포스팅을 보고 많은 것을 배워야 겠다 !



## 좋은 Git 커밋 메시지를 작성하기 위한 7가지 약속

### #1. 제목과 본문을 한 줄 띄워 분리하기

커밋메세지는 **50자 이내 요약문자**과 **빈줄 하나**, 그리고 **설명문**으로 구성하면 좋다.

빈줄 하나를 넣는 이유는 **git log --online**을 사용했을 때 출력이 보기 좋게 하기 위함이다.

```
아직 긴 commit메세지를 작성해 본적은 없지만 알아둬야 겠다.!
git log / git log --online / git shortlog 알아두기‼️
```



### #2. 제목은 영문 기준 50자 이내로

더 읽기 좋은 커밋 메세지를 만들기위해 노력 하라 !

```
인턴을 하며 글자수 상관없이 작성했던 것 같다. 물론 한글이었지만. 짧고 간결하게 작성하도록 해야겠다.
```



### #3.제목 첫글자를 대문자로

```
요즘 가장 지키기 위해 노력하는 부분이다. 하지만 귀찮을 때 그냥 올리기도 하는데 지키도록 해야겠다.

알고리즘 풀면 Solve Q_문제번호_문제이름 이런식으로 작성하도록 해야겠다.
```



### #4. 제목 끝에 . 금지



### #5. 제목은 명령조로

가장 **중요한 것** 이다 !

제목을 작성하거나 한 줄 메시지로 커밋을 할 때, 즉 커밋메세지 가장 첫 문장의 영문법은 **명령조**로 해야한다. 명령조니까 첫 단어는 **동사원형**으로 쓰여야 한다.

- 왜 ⁉️

  git은 스스로가 자동 커밋을 작성할 때 명령문을 사용하고 있다. 예를 들어 git merge를 실행했을 때 커밋 메세지 기본 값은 이러하다.

  ```bash
  Merge pull request #123 from someuser/somebranch
  ```

  이처럼 커밋 메시지를 명령문으로 작성하는 것은 git의 빌트-인 컨벤션(Built-in Convention)을 그대로 따른다는 것을 의미한다.

- 명령문 예!

  ```
  - Fix
  - Add
  - Remove
  - Use
  - Refactor
  - Update
  - Improve
  - Make
  - Implement
  - Revise
  - Correct
  - Ensure
  - Prevent
  - Avoid
  - Move
  - Rename
  - Allow
  - Verify
  - Set
  - Pass
  ```

  



### #6. 본문은 영어 기준 72자마다 줄 바꾸기

git은 자동으로 커밋 메세지 줄바꿈을 하지 않는다. 단순한 git log명령어 입력만으로도 보기 좋은 메시지를 만들고자 한다면 적당한 위치에서 엔터키를 눌러 바꿔줘야 한다. 그 적당한 위치로 72자를 추천한다.



### #7. 본문은 어떻게보다 무엇을, 왜에 맞춰 작성하기



### TIP- 커밋 메시지로 Git이슈 자동 종료시키기

git에 커밋 메시지를 특정한 단어를 사용해 자동으로 이슈를 종료시키는 기능이 탑재되어있다. 이 예약어는 커밋 메시지 안의 어느 위치에서나 사용 가능하다. 형식은

```
키워드 #이슈번호
```

Git이 이슈 종료로 인식하는 키워드는 다음과 같다.

```
- Close / Closes / Closed
- fix / fixex / fixed
- resolve / resolves/ resolved
```





#### 느낀점

위에서 말한 것들을 모두 지키기는 쉽지 않을 것 같다. 하지만 유의해서 지켜보도록 하겠다 ‼️