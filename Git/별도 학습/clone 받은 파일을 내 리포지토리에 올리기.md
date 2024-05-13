## 원격 저장소 주소 수정하는 방법

1. 먼저 클론 받아오기

```
git clone <다른사람 원격저장소 주소.git> .
```

2. 깃허브에서 내 리포지토리 만들기

3. 내 파일 위치로 이동한 뒤 원격 리포지토리 주소를 내 리포지토리 주소로 변경

```
cd <이동할 디렉토리 주소>
git remote set-url <내 리포지토리 주소.git>
```

4. 변경사항 커밋하고 push

```
git add .
git commit -m "커밋 메시지"
git push -u origin master
```

## .git 삭제하고 다시 시작하는 방법

1. 디렉토리에서 숨김폴더인 `.git` 삭제

2. 터미널에서 `git init` 실행

3. 로컬과 내 깃허브 리포지토리 연결

```
git remote add origin <원격 저장소 주소.git>
```

### 강제로 pull 하는 법

`.git`을 지워버렸기 때문에 클론받은 히스토리와 내 리포지토리의 히스토리가 일치하지 않아 pull이 안 될 것이다.

아래 명령어로 강제로 pull 할 수 있다.

```
git pull origin main --allow-unrelated-histories
```

4. 변경사항 커밋하고 push

```javascript
git add .
git commit -m "커밋 메시지"
git push -u origin master
```

## master 브랜치 이름을 main으로 바꾸는 법

```
git branch -m master main
```

