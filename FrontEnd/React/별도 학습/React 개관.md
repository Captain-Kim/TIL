## 포르젝트 세팅 (vite)

```
// 터미널에서 명령어 입력
npm create vite@latest

// 작업할 프로젝트명 입력
react_basic

// 작성할 언어 선택
javascript

// 디렉토리로 이동
cd react_basic

// 모듈 파일 설치
npm install

// 브라우저 띄우기
npm run dev
```

## 초기 세팅하기

App.jsx, App.css, Index.css 필요없는 부분 지우기

## 초기 작업 - 컴포넌트 세팅

```jsx
import { useState } from ".react"
function App () {
    return <counter></counter>; // 아래에서 만든 counter라는 함수를 이곳에서 컴포넌트로 불러오는 것.

    function counter() {

// 이 영역에 자바스크립트 작성

        return (
            // 이 영역에 jax 작성
            <div>
            </div>
        )
    }
}
```

## state 만들기

### 상태로 정의하기 (state)

```jsx
function App () {
    const [ counter, setCounter ] = useState(0); // counter의 값으로 초기값 지정, 어떠한 자료형이 와도 됨.
    // 그러나 주로 배열을 상태로 많이 정의함.

    return (
        <>

        <div>
            {counter} {/* 위에서 상태로 정의한 state를 이곳에 데이터 바인딩 함.(불러옴)) */}
        </div>

        </> {/* 프레그먼트, 아무것도 없는 div를 jsx return문 안에 최상단에 감싸주어야 함. */}

    )
}
```

### 상태 변경하는 handle 함수 만들기

1. 화살표 함수 만들기

```jsx
function App () {
    const [ counter, setCounter ] = useState(0);

    const handleCounter = () => {}
```

React에서 자바스크립트 작성 부분에 함수를 표현식으로만 사용 하는 이유?

함수 표현식은 함수 호출하지 않아도 바로 실행이 되는지? 아니면 어떻게 실행해야 하는지?

2. 상태 변경 함수를 함수 내부에서 실행하기

```jsx
function handleCounter() = () => {
    setCounter(
        counter = counter++;
    )
}
```

3. 위에서 만든 `handleCounter` 함수를 카운트하기 등의 버튼에 `onClick`으로 바인딩 해주면 됨.

```jsx
import { useState } from 'react';

function Counter() {
  // 상태(state) 정의
  const [counter, setCounter] = useState(0);

  // 상태 변경 함수 정의
  const handleCounter = () => {
    setCounter(counter + 1);
  };

  return (
    <div>
      <p>{counter}</p>
      <button onClick={handleCounter}>증가</button>
    </div>
  );
}

function App() {
  return (
    <>
      <Counter />
    </>
  );
}

export default App;
```

### 만든 함수를 컴포넌트로 만들어서 내보내기

```jsx
// counter 함수 내보내기 (컴포넌트 파일 최하단에 작성)

// src/components/Counter.jsx
import { useState } from 'react';

function Counter() {
  const [counter, setCounter] = useState(0);

  const handleCounter = () => {
    setCounter(counter + 1);
  };

  return (
    <div>
      <p>{counter}</p>
      <button onClick={handleCounter}>증가</button>
    </div>
  );
}

export default Counter; // 파일명. 즉 대문자로 시작하는 파일명임.
```

2. import 하기

```jsx
// src/App.jsx
import Counter from './components/Counter';
import './App.css';

function App() {
  return (
    <>
      <Counter /> {/* Counter 컴포넌트를 jsx에서 불러옴 */}
    </>
  );
}

export default App;
```


### 어떤 것을 컴포넌트로 만들어야 하나?

1. 반복되는 UI

코드에서 동일하거나 비슷한 UI가 반복되는 경우. 예를 들어 여러 곳에서 동일한 버튼, 카드, 입력 폼 등을 사용할 때.

2. 특정 기능을 담당하는 컴포넌트

특정 기능을 담당하는 코드블럭을 하나의 컴포넌트로 분리할 수 있음. 예를 들어 리스트를 렌더링 하는 코드나 폼 제출을 처리하는 코드.

3. 코드가 복잡할 때 분리하기

하나의 코드가 너무 길 때 여러가지 컴포넌트로 분리할 수 있음.

4. 추후 기능을 확장하거나 수정할 가능성이 높은 코드

계속 유지보수가 필요한 코드는 따로 컴포넌트로 분리하면 유지보수하기 편해짐.