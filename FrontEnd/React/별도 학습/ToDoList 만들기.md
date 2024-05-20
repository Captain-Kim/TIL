## 깃허브 리포지토리 세팅하기

1. 리포지토리 세팅하는 방식으로 세팅함.

2. MIT 라이센스로 설정하면 좋음. 오픈소스로 활용하되 사용하는 경우 언급해달라는 뜻.

3. 작업할 로컬 디렉토리를 만든 후 터미널로 들어와 `clone`함.

## vite 환경 프로젝트 세팅하기

```
// 터미널

npm create vite@latest

또는

yarn create vite

// 이후 프로젝트 이름을 쓸 때 . 하나만 입력하면 현재 만든 폴더에 그대로 만들겠다는 의미.

project name: .

// 라이센스는 Ignore.... 선택

// 라이브러리는 React 선택

// 언어는 JavaScript + SWC (SWC는 성능이 개선된 버전)

yarn // npm install과 같은 것.

yarn dev // 라이브 서버 여는 것. package.json에 사용 가능한 명령어 있음.
```

## 초기 프로젝트 파일 세팅하기

1. App.jsx 내용 전부 삭제 후 `function App() {}` 과 `export default App`만 남기고 전부 삭제.

2. App.css 파일 자체를 삭제. (App.jsx의 컴포넌트용 CSS인데 어차피 투두리스트 원페이지라 index.css 만으로도 가능)

3. index.css 기본 내용물 전부 삭제.

4. 구글에 `reset CSS` 검색 후 기본 CSS 내용 전부 초기화하기 위해 index.css에 복붙. 별도로 파일로 만들어서 import 해도 됨.

## 메인 레이아웃 만들기 (App.jsx 메인 컴포넌트)

### input 영역

```jsx
import React from "react";

function App() {

  const onSubmit = (e) => {
      e.preventDefault();
  }
  
      return(
          <>
  
          <div>
              <form onSubmit={onSubmit}>
                  <label>제목</label>
                  <input type="text" placeholder="제목">
                  </input>
                  <label>내용</label>
                  <input type="text" placeholder="내용">
                  </input>
                  <button type="submit">작성하기</button>
              </form>
          </div>
  
          </>
      )
  }

  export default App
```

`onSubmit`이라는 함수를 만들고 내용으로 `e.preventDefault` 내장 함수를 넣었다. form 제출 버튼 클릭 시 페이지가 새로고침 되는 것을 막기 위함이다.


`form`태그에 사용된 `onSubmit`이라는 속성은 form이 제출될 때 어떤 함수를 실행할 것인지에 대해서 명시하는 것이다. 일단 `preventDefault`로 막아두고 추후 `addTodo`라는 함수를 만들어서 함수를 `onSubmit` 속성에 지정해줄 것이다.


