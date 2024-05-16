## 상황

Mac OS 환경에서는 에디터로 작업 후 push를 할 때 `DS_Store`라는 파일이 자동으로 생성되어 함께 push된다.

이 파일에는 보기 옵션이나 아이콘 위치 등 민감할 수 있는 정보들이 담기면서 계속 수정이 되는데, 개발자의 의도와는 다르게 파일에 계속 변경사항이 생기기 때문에 merge 과정에서 계속 성가신 충돌을 발생시킨다.


이 문제를 해결하기 위해선 `.gitignore`파일을 만들어서 push할 때 업로드 되지 않도록 하여야 한다.

그런데 매번 프로젝트를 생성할 때마다 이 작업을 하긴 번거로우니, `.gitignore`를 전역으로 생성하도록 설정하면 된다.

## 방법

```
// .gitignore 파일 생성
git config --global core.excludesfile ~/.gitignore_global

// .gitignore 파일에 DS_Store 파일 추가
echo ".DS_Store" >> ~/.gitignore_global

// 만약 이미 원격저장소에 DS_Store 파일이 올라 가 있다면 아래 명령어로 DS_Store 파일 삭제
git rm --cached -r .DS_Store
git commit -m "Remove: .DS_Store 파일 삭제"
```