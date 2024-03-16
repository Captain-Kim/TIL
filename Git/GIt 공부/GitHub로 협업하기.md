[학습 출처 | 유튜브 코딩알려주는누나](https://www.youtube.com/watch?v=tkkbYCajCjM)

Git과 GitHub에 대한 개념이 있더라도 이를 통해 팀원들과 협업하는 과정은 어렵다.  
별도로 학습해야 할 필요가 있다.  

이 내용을 학습하려면 Git과 GitHub에 대한 이해가 선행되어야 한다.

## 학습 순서

1. Repository 생성
2. 팀원 초대
3. 프로젝트 환경설정
4. develop 브랜치 생성
5. master 브랜치 보호
6. 프로젝트 보드 생성
7. Git issue 생성
8. feature 브랜치 생성
9. 팀원 역할 시작 (협업 시작)
   1.  프로젝트 클론
   2.  feature 브랜치 생성
   3.  개발 진행
   4.  소스코드 업로드
   5.  풀 리케스트 (PR) 생성
   6.  코드리뷰
10. 깃 충돌 해결하기
    1. develop 브랜치에 최신 코드 가져오기
    2. 다시 feature 브랜치로 돌아가기
    3. feature 브랜치에서 develop 브랜치와 merge하기
    4. 코드 충돌 해결하기
    5. 완성된 코드 다시 올리기
    6. 다시 코드리뷰 받고 merge
10. develop에서 master로 배포하기

## Repository 생성

메인에서 New 버튼을 클릭하여 Repository 생성.

## 팀원 초대

- 새로 만든 Repository에서 **Invite collaborators** 클릭
- Add people 클릭
- GitHub 아이디로 팀원 초대
- 초대까지 완료하면 Pending Invite 상태 (초대 미수락)
- 초대를 수락하면 Repository에 함께할 수 있게 됨.
    - 초대를 수락할 때는 이메일로 수락 가능

## 프로젝트 환경설정

### Git remote 도메인 복사

생성된 Repository에서 ...or creat a new repository on the command line에서  
```
git remote add origin <git도메인>
```
부분을 복사 (뒤에 환경 세팅하면서 사용할 것임)

### Master로 지정할 작업 디렉토리에서 터미널을 통하여 Git 세팅

아래 명령어를 순서대로 한 줄 씩 입력 후 엔터.
```
git init
git add .
git commit -m "first commit"
git remote add origin <Repository 도메인> (아까 복사한 내용)
git push origin master
```

## develop 브랜치 생성

Master는 최종 결과물이 저장된 장소. 즉 웹사이트로 예를 들면 실제 유저들이 계속 사용하고 있는 최종 결과물이기 때문에 이곳에서 작업은 어렵다.  

따라서 branch라는 공간을 만들어서 작업을 하게 된다.  
일반적으로 branch는 working directory, staging area, repository 세 개로 구분하지만 이 내용에서는 2개를 만드는 실습을 진행한다.

아래 명령어로 branch를 생성한다.
```
git checkout -b develop
git push
git push --set-upstream origin develop
```

GitHub repository에서 master라고 되어 있는 드롭박스를 클릭하면 develop branch가 생성된 것을 확인할 수 있다.  
그리고 그 아래 경고문이 보인다.
> Your master branch isn't protected  

마스터 branch를 보호해주는 작업을 해야 한다.

## master 브랜치 보호

경고문 옆에 있는 **Protect this branch** 버튼을 클릭.

여러가지 보호 규칙이 뜨는데, 가장 중요한 두 가지를 체크한다.

> Require a pull request befroe merging

develop branch에서 작업한 결과물을 master로 merge(병합)할 때 바로 적용되는 것이 아니라 요청을 생성하고 모든 팀원이 동의해야 merge 되도록 설정하는 보호 옵션.

> Lock branch

master branch에 아무나 push 할 수 없도록 보호하는 옵션.

완료되면 **Creat** 버튼 클릭.

보호가 잘 됐는지 확인해보려면 master 브랜치로 이동하여 push가 되는지 확인해보면 됨.

디렉토리 이동 명령어
```
git checkout master 또는 develop
```

repository에 commit을 추가해서 변경사항을 만들고 push 하는 명령어
```
git add . // 해당 디렉토리의 모든 파일을 Staging Area에 올림.
git commit -m "문구" // 커밋메시지 작성.
git push origin master // master에 push.
```

## 프로젝트 보드 생성

repository에서 **projects** 클릭  
**Link a projectk** 클릭  
**creat new project** 클릭  

이곳에서는 팀원들끼리 To do(해야 할 일), In Progress(하고 있는 중), Done(완료)을 작성할 수 있다.  

In Progress에서 특정 이벤트를 누른 뒤 우측 패널에서 **Convert to issue**를 누르면 해당 이벤트를 git issue로 넘길 수 있음. 

### 특정 feature 작업공간 branch 생성

제목을 눌러서 우측 패널을 다시 보면 **Creat a branch**라는 링크가 있는데 이것을 누르면 이 feature에서 별도로 branch를 생성할 수 있음.  
develop branch를 생성하긴 했지만 이곳에 가기 전까지 연습장처럼 이것저것 개발을 해볼 공간이 필요하기 때문. 위 링크를 눌러서 진입하고 Branch source를 master가 아니라 develop로 바꾸면, 여기서 새로 만든 feature branch가 완료되면 곧바로 master로 push하는 것이 아니라 develop branch로 push 할 수 있음.  
여기까지 완료되면 git 명령어가 팝업되는데 터미널에 입력하면 해당 branch로 접속됨.

## 팀원 역할 시작 (협업 시작)

### 프로젝트 clone

초대받은 repository 방문.  
<> Code를 눌러 clone 주소 복사.  
터미널에서 **git clone <주소> git <나의이름>** 입력.

완료되면 master branch에 있는 파일들이 에디터로 열림.  
지금 열린 소스코드는 master branch 파일이기 때문에 여기서 작업하면 안 되고 본인의 역할을 먼저 확인해야 함.  
GitHub repository에서 projects에서 확인.  
본인의 feature를 확인 후에 convert to issue를 클릭하여 이슈 생성.  
creat a branch를 클릭하여 작업할 branch를 생성.  master source를 develop으로 변경.  
팝업되는 git 명령어를 터미널에 붙여넣기.

### 소스코드 업로드 (본인의 branch로)

본인의 branch에서 작업이 완료되면 소스코드를 내 로컬 저장소에서 원격저장소(방금 만든 feature 브랜치)로 보내야 함.  
```
git add .
git commit -m "커밋메시지"
git push
```
여기까지 완료되면 git repository에서 드롭박스를 눌러보면 내가 만든 branch명이 뜨는 걸 볼 수 있고 소스코드가 업로드 되어있는 것을 확인 가능함.

### develop branch로 보내기 (풀 리퀘스트(PR) 생성)

GitHub repository에서 Pull requests를 눌러서 **New pull request** 버튼을 클릭.  
> base : master <- compare : ??

위처럼 선택할 수 있도록 드롭박스가 열림.  
내가 만든 branch에서 develop로 보낼 것이기에 아래와 같이 수정.
> base : develop <- compare : feature-B(내가 만든 branch명)  

**Creat pull request** 클릭.  
글쓰기 에디터에서 내가 어떤 작업을 했는지 입력해줌.

### 코드리뷰 하기

팀 리더는 Pull request에서 해당 request에 대해서 File changed에서 삭제되고 추가된 내역을 -, + 형태로 확인할 수 있음.  
여기에서 댓글 형태로 코드리뷰 하면 됨.  

리뷰 작성 후, 우측 상단 **Finish your review** 버튼 클릭.  
- Approve : Pull request 승인
- Request changes : Pull request 반려

팀 리더에게 approve가 완료되면 해당 feature branch 대시보드에 내용이 뜨게 되고 **Merge pull request**를 누를 수 있도록 버튼이 생김. 팀 리더가 하는 게 아니라 branch에서 개발한 개발자가 누르는 것.

## 깃 충돌 해결하기

협업을 하다 보면 가장 많이 발생하는 에러가 내가 작성하고 있던 소스코드 라인에 다른 팀원이 작업을 해서 develop에 push를 이미 했는데 내가 그 라인에 작업을 한 경우에 발생하는 에러다. 이외에도 다양한 에러 상황이 있을 수 있다.

>This branch has conflicts that must be resolved

위 문구 아래 두 가지 해결방법 링크가 있는데, web editor에서 직접 수정하는 것과 command line을 사용하는 방법이 있고 command line을 사용하는 것이 좀 더 편리하다.

### develop 브랜치에 최신 코드로 가져오기

**comande line** 을 누르면 충돌을 해결할 수 있는 명령어가 나온다.

```
git checkout develop
git pull origin develop
git checkout <내 브랜치명>
git merge develop
git push -u origin <내 브랜치명>
```

**git checkout develop**  
develop 브랜치로 이동해본다. 다른 팀원이 작업해서 이미 push한 내용이 내 에디터에는 적용이 안 되어있을 것이다. (구버전)

**git pull origin develop**  
내가 가진 develop branch의 버전이 구버전이니 최신 버전으로 불러오기.  
내 에디터에서 최신의 develop 작업물이 출력된다.

**git checkout <내 브랜치명>**  
다시 내 브랜치로 돌아간다.  
만약 내가 develop 브랜치로 들어가기 전 내 브랜치에 있었다면 아래 명령어처럼 입력하면 내가 이전에 있던 브랜치로 이동하기 때문에 바로 이동 가능함. (뒤로가기)
```
git check out -
```

**git merge develop**  
develop에 merge 하고 나면 나와 충돌이 났던 코드와 에디터에 동시에 출력되는데, 여러가지 제안이 뜬다.  

아직 팀프로젝트를 진행해보지 않아, 이해가 어려워 다른 블로그에서 참고하였다. [출처](https://limjunho.github.io/2020/10/12/VSC%EC%97%90%EC%84%9C-git-conflict%ED%95%B4%EA%B2%B0.html)

- Accept Current Change : 헤드 부분을 적용 (내가 고친 것)
- Accept Incoming Change : 변경된 부분을 적용 (병합 대상이 된 브랜치의 내용으로 변경) (남이 고친 것)
- Accept Both Changes : 둘 다 적용 (헤드와 변경된 부분 둘 다)
- Compare Changes : 충돌(Conflict가 난 부분을 좀 더 보기 쉽게 보여줌)

### 완성된 코드 다시 올리기

```
git add
git commit -m "커밋메시지
git push
```

그리고 GitHub에서 내 branch에 접속하면
> This branch ha no conflicts with the base branch  

라고 더이상 충돌이 없다는 문구가 출력된다.  
이후 팀 리더가 확인 후 approve 한 후 branch 개발자가 **Confirm merge**하면 된다.


## develop에서 master로 배포하기

release(배포) 담당자가 실행한다. 현업에서는 보통 2주 단위로 develop에서 master로 배포한다.

new pull request  
base : master <- compare : develop  
creat pull request  

여기까지 하면 아래와 같은 에러가 출력된다.  
> review required

> merging is blocked  

코드 리뷰를 진행하면 된다.  
**Files changed** 클릭 후 문제 없으면 **approve**