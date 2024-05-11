## Git을 사용하는 이유

코드를 백업하고 이전 버전으로 되돌리기 위함. (버전기록/관리)

## Git 설치 방법

Git windows 검색 후 설치.


**선택 옵션 체크 두 가지**
- Use Visual Studio Code as Git's default editior
- Override the default vbranch name for new repositories => main으로 입력

## Git 최초 1회 개인정보 등록

PowerShell 터미널을 연 후 아래 명령어 하나씩 차례대로 입력.

```
git config --global user.email (예: kim@google.com)
git config --global user.name (예: Kim ByeongJun)
```

## 작업 폴더 세팅하기

1. 작업 폴더에서 에디터나 파워쉘을 엶.
2. git init 1회 입력
   1. .git이라는 숨김 폴더가 생성되면서 이 안에 버전이 기록된다.

## 파일의 현재 상태를 기록하기 (git add)

1. 특정 파일 add 하기
```
git add 파일명1.확장자 파일명2. 확장자 ...
```

2. 전체 파일 add 하기
```
git add .
```

## 기록 남기기 (git commit -m "메시지")

add로 버전을 생성한 파일들의 현재 버전에 대해서 메모하는 기능.

```
git commit -m "최초 파일 생성"
```

파일이 수정돼서 두 번째 버전이 생겼다면 git add, git commit을 반복하면 된다.

### 개념

작업 폴더 -> (git add) -> staging area -> (git commit) -> repository(저장소)


작업 폴더에서 staging area로 파일을 git add로 넘기는 행위를 staging이라고 한다.

## git 상태 확인하기 (git status)

파일의 현재 상태를 볼 수 있음.  


### 주요 상태 메시지

1. nothing to commit, working tree clean
   1. 변경사항이 없기 때문에 commit할 필요가 없다.
2. Changes not staged for commit: 파일명
   1. 변경사항이 있는데 stage 되지 않았다. 즉 git add 되지 않은 상태다.
3. Changes to be committed: 파일명
   1. 변경사항이 staging되었으나 commit은 되지 않았다.
   2. 아래 예제 modified에는 이런 상태 메시지들이 온다.
      1. created: 새로 만들어짐
      2. modified: 수정됨
      3. deleted: 삭제됨
```
Changes to be committed:
        modified: hello.js
```

## 커밋한 내역 확인하기 (git log)

로그가 너무 길어서 터미널에서 빠져나오지 못하면 q를 입력하고 엔터를 누르면 된다.


j / k는 스크롤바 조작.

## 이전 버전과 현재 버전의 차이를 비교하기 (git diff, git difftool)

### git diff

코드를 작성 후 저장하고 나서 내가 이전에 기록한 버전과 어떤 차이가 있는지 비교해볼 수 있다.


터미널에서 스크롤이 길어서 빠져 나오기 힘들면 위처럼 j,k로 스크롤 조작, q로 탈출하면 됨.

### git difftool

그런데 터미널창에서 코드 변화를 보기 어렵기 때문에 vim 에디터를 띄워서 시각화된 비교 자료를 볼 수도 있다. (git difftool 입력 후 y/n 중 선택. 보려면 y) 이곳에서 방향키는 k j k l, 종료는 :q 또는 :qa


#### git difftool 커밋아이디

커밋 아이디를 입력하지 않고 git difftool만 입력하면 바로 이전버전과 비교를 해주고 커밋 아이디를 입력하면 그 커밋 버전과 비교를 해준다.


커밋 아이디는 git log에서 나오는 임의의 숫자+문자가 섞인 코드.

#### git difftool 커밋아이디1 커밋아이디2

커밋아이디1 버전과 커밋아이디2 버전을 비교.

### VSCode에서 익스텐션으로 시각화하기

터미널에서 git log를 하고 commit을 비교해볼 수도 있지만 이를 시각화해서 도와주는 익스텐션이 많이 존재한다.


그 중에서도 Git Graph를 설치하면 VSCode 좌측 아이콘 중 3번째 git과 관련된 메뉴를 누르고 위쪽 아이콘에서 체크표시 옆에 Git Graph 아이콘을 누르면 터미널보다 훨씬 직관적으로 git 내역이 표시됨.

## 브랜치에서 작업하기 (git branch)

브랜치 개념 없이 작업해왔다면 내 작업폴더는 master 브랜치이다. (흑인 노예 주인 인종차별 어감 때문에 main으로 바꾸어 부르는 추세.)


main 브랜치에서 바로 작업을 한다면 에러가 발생할 수 있고 코드가 길어 복잡하거나 main 브랜치는 24시간 서비스 중인 코드일 경우엔 브랜치를 따로 만들어서 그곳에서 main 브랜치의 파일을 복제하여 작업해볼 수 있다.

### 현재 브랜치 위치 확인하기 (git branch)

```
git branch
```

HEAD 라고 나오는 것은 나의 머리임. 즉 나의 위치.

### 브랜치 생성하기 (git branch 브랜치명)

```
git branch 브랜치명
```

### 브랜치 이동하기 (git switch)

```
git switch 브랜치명
```

위 명령어로 이동해보고 잘 이동 됐는지 git branch로 확인해본다.


git branch로 이동하면 main 브랜치의 파일이 그대로 보이는데 여기에서 파일을 수정하거나 더 만들거나 삭제하더라도 main 브랜치에는 영향을 주지 않는다. 따라서 git switch main을 하더라도 내가 수정한 내역은 보이지 않는다. 이 상태에서 내 브랜치에서 작업한 결과를 최종으로 main에 기록하고 싶다면 git merge를 이용해야 한다.

### 메인 브랜치에 최종적으로 기록하기 (git merge)

```
// 현재 내 작업 브랜치에서 main 브랜치로 이동 후
git switch dev (git checkout dev도 있으나 switch 권장)
git merge 내브랜치명
```

정상적으로 완료가 될 수 있지만 다른 사람과 동시에 같은 라인을 수정한 경우 conflict(충돌)가 발생한다.


충돌이 발생하면 에디터 화면에서 위에 뜨는 것이 내가 입력한 것이고, 아래 뜨는 것이 기존에 있던 내용이다. 둘 중 더 괜찮은 코드를 골라서 지우고 남기면서 충돌을 제거해나간다.


또는 VSCode 같은 에디터에서는 '현재 내용으로 수락' 등 다양한 버튼으로 옵션을 제공하기 때문에 직접 지우면서 실수하기 보다는 이런 버튼을 이용하는 것도 좋다.


충돌을 모두 해결하고 나면 git add, git commit을 반복한다. (main 브랜치에 있는 상태임.)


이러한 방식을 현업에서는 **3-way merge**라고 부른다.

#### 브랜치 삭제하기 (git branch -d 브랜치명)

merge를 완료해서 더이상 branch가 필요 없다고 하더라도 브랜치는 삭제되지 않고 남아있다. 삭제가 필요하다면 아래 명령어로 삭제한다.


일반적으론 메인 브랜치에 merge를 완료하여 더이상 필요없는 branch(특정 기능 개발을 위한 브랜치)는 삭제한다.

```
// merge 완료된 브랜치 삭제
git branch -d 브랜치명

// merge 안 한 브랜치 삭제
git branch -D 브랜치명
```

#### 메인 브랜치에서 커밋한 것처럼 merge하기 (git rebase, git merge --squash)

이 말의 의미는, 브랜치를  여러개 만들고 계속 기존 방식대로 merge를 반복하다 보면 main 브랜치에서 commit log를 하더라도 기능 개발을 위해 만들었던 수십 개의 브랜치에서 commit한 log까지 전부 출력된다.


그래서 가독성이 떨어질 수 있는데, 기능 개발을 위해 새로 만들었던 branch에서 개발용으로 막 작성한 잔챙이 commit log 내역은 볼 필요가 없다면 잔챙이 branch에서 main 브랜치로 커밋 로그가 연결되지 않도록 연결점을 끊어주어야 하는데,


이 때 rebase라는 명령어나 git merge --squash 명령어를 사용하면 마치 main 브랜치에서 바로 작업한 것처럼 분할했던 branch의 흔적은 보이지 않는다.

##### git rabase

```
// 새로운 브랜치로 이동
// git rabase 중심브랜치명
// 중심 브랜치로 이동
// git merge 새로운 브랜치명
```

|   명령어   |    시작점     |          명령어          |                       후속타                       |
| :--------: | :-----------: | :----------------------: | :------------------------------------------------: |
| git merge  |  중심 브랜치  | git merge 새로운브랜치명 |                         -                          |
| git rabase | 새로운 브랜치 | git rebase 중심브랜치명  | 중심브랜치로 다시 이동 후 git merge 새로운브랜치명 |

##### git merge --squash

```
git merge --squash 새브랜치
```

[참고자료](https://blog.outsider.ne.kr/1704)

## 버전 되돌리기 (git revert, reset, restore)

### 특정 파일 이전 커밋으로 되돌리기 (git restore)

```
// 특정 파일 이전 커밋으로 되돌리기
git restore 파일명.확장자

// 커밋을 직접 지정해서 되돌리기
git restore --source 커밋아이디 파일명.확장자

// 특정 파일 git add 취소시키기 (staging 취소)
git restore --staged 파일명.확장자
// 이 명령어는 git status를해도 출력되기 때문에 외울 필요 X
```

### 특정 commit 취소하기 (git revert)

```
git revert 커밋아이디1 커밋아이디2 커밋아이디3
```

커밋 자체를 삭제할 순 없다. 그런데 과거 커밋에서 수정했던 버전을 삭제하고 싶다면 위 명령어를 입력한다.


위 명령어를 입력하면 Vim 에디터가 뜨는데 i를 입력해서 Revert "커밋메시지"에 커밋 메시지 작성 후 :wq로 저장 후 닫으면 완료.


예를 들어  
1번 파일 생성 : 1번 커밋  
2번 파일 생성 : 2번 커밋  
3번 파일 생성 : 3번 커밋  


위와 같은 경우

```
git revert 2번커밋아이디
```
를 입력하면 2번 파일을 생성했던 버전이 삭제되어 최종적으로는 1번 파일과 3번 파일만 남게 됨.  
그러나 2번 커밋 자체가 사라지지는 않고 4번 커밋으로 'Revert "커밋메시지"'가 하나 더 커밋 메시지가 생김.

### 가장 최근 commit 취소하기 (git revert HEAD)

바로 방금 전에 커밋한 1개의 커밋을 취소함.

```
git revert HEAD
```

**revert 명령어는 merge를 한 커밋도 취소 가능**

### 과거로 모든 걸 되돌리기 (git reset --hard)

revert는 커밋 메시지 자체를 삭제할 순 없었지만 reset은 내가 1번 파일 생성 커밋 시점으로 복구를 하면 그 뒤로 만들어진 커밋 자체가 전부 삭제되고 복구되기 때문에 **협업 시에는 사용하지 말 것**


혼자 작업할 때도 굳이 기록을 날릴 중요한 이유가 없다면 쓰지 않는 것이 좋음. 짧은 거리를 되돌아 갈 때는 커밋 로그 자체를 깔끔하게 바꿔줄 수도 있겠지만 먼 거리를 되돌아 갈 때는 기록 자체를 날리기 때문에 좋은 것은 아님.

```
// 커밋아이디 이후의 커밋은 싹 날아감
git reset --hard 커밋아이디

// 리셋은 하나 커밋아이디 이후의 커밋이 스테이징 되어 있음.
git reset --soft 커밋아이디

// 리셋은 하나 커밋아이디 이후의 커밋이 스테이징까지 안 돼 있음.
// git reset --mixed 커밋아이디
```

즉 mixed는 필요하다면 git add하여 스테이징 하면 되고, soft는 add는 되어 있는 상테에서 커밋만 취소하는 것이고, hard는 전부 날리는 것이다.

## 원격저장소 이용하기

### 사용하는 이유

로컬에만 의존하지 않는 클라우드 백업과 협업.  
주로 많이 사용하는 원격저장소는 GitHub.

### GitHub 원격 저장소와 내 로컬 저장소 연동하기

1. GitHub 회원가입
2. 원격 저장소 생성 (Repository)
3. 로컬 저장소 작업 폴더에서 git init
4. master branch 이름 변경 git branch -M main
   1. GitHub 권장사항. 인종차별 문제.
5. git add, commit까지 완료된 상태라면,
6. git push -u 원격저장소주소 올릴로컬브랜치명
   1. 원격저장소 주소에는 .git이 붙은 주소여야 함.
   2. 깃허브 리포지토리를 만들고 제공해주는 명령어를 입력해서 위 과정을 모두 마쳤으면 번거롭게 원격저장소주소를 매번 입력 안 해도 되고 git push만 입력해도 됨.
      1. 또는 아래 명령어로 직접 변수에 할당하면 됨.
      2. git remote add origin https://github....
      3. origin은 변수명인데 임의로 작명해도 되고, 관습적으로 많이 사용하는 origin을 사용하면 됨.
      4. git push orign main(브랜치명)을 사용하면 됨.
      5. 그런데 이 과정도 복잡하다면 git push -u origin main라고 입력하면 -u의 의미는 이 뒤는 기억하라는 의미로, git push만 사용해도 뒤가 자동으로 됨.

## 여러명과 협업하기

### 팀원이 코드 다운 받기

1. GitHub 리포지토리에서 Code 버튼을 눌러 .zip 파일로 통째로 다운로드 가능
2. 작업폴더를 만들고 터미널에서 git clone 원격저장소주소 입력하면 모두 다운로드 됨

### 팀원이 push하기

팀원도 GitHub 계정이 있어야 하고 리포지토리에 팀장이 초대해주고 승락해야 함.


팀장은 리포지토리 settings에서 collaborators에서 팀원 깃허브 아이디나 이메일을 입력해서 초대 전송 가능. 팀원도 그 내용을 수락해주어야 함.


#### 다른 팀원이 먼저 push 한 경우 => 충돌 (git pull)

브랜치에 다른 팀원이 먼저 push한 경우에는 git pull을 사용해서 그 팀원이  최신으로 업데이트한 자료로 내 작업 폴더도 업데이트 해야 함.


이런 에러가 발생함.


error: failed to push some refs to '원격저장소주소'

```
git pull
```

원래 a 파일이 있었고 내가 c 파일을 만들어서 push를 하려고 봤더니 다른 팀원이 b 파일을 만들고 먼저 push를 해서 에러가 나는 경우,


git pull을 하면 내 폴더에는 a, b, c 파일이 생긴다. 이후에 git push하면 됨.


```
// 특정 브랜치만 pull 하기
git pull origin 브랜치명
```


git pull의 원리는 git fetch와 git merge를 한번에 하는 명령어.  

git fetch는 원격 저장소 신규 commit을 가져오는 것이고 git merge는 내 브랜치에 merge하는 것.

### GitHub에서 merge하기 (pull request)

로컬에서 git merge를 통해 직접 merge 할 수도 있겠지만, 여러명이 협업하는 경우 팀원들의 코드 리뷰가 필요하다. 이럴 때는 로컬에서보단 GitHub에서 merge를 하는데 리포지토리에서 pull request 버튼을 누르면 됨.


- base : 합쳐질 곳  
- compare : 합칠 것


conflict가 발생하면 solve 버튼을 눌러 해결하면 됨.


merge 옵션으로는 아래 세 가지가 있다. 이 차이는 위에서 언급했다. 아래 두 개는 브랜치의 커밋이 들어가지 않는다.

- Creat a merge commit
- Squash and merge
- Rebase and merge

### 브랜치 전략

브랜치를 사용하는데 있어 중구난방이면 개발과정이 복잡해지기 때문에 미리 약속된 흐름이나 전략이 있다.