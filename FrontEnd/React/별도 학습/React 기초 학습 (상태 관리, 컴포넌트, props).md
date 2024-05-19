## 기존코드와 개선된 코드

### 기존 코드 (하나의 App.jsx에 작성)

```jsx
import { useState } from 'react';
import './App.css';

function App() {
  const [cards, setCards] = useState([]);
  const [newTitle, setNewTitle] = useState('');
  const [newBody, setNewBody] = useState('');

  const addCard = (e) => {
    e.preventDefault();
    if (newTitle === '' || newBody === '') {
      setNewTitle('');
      setNewBody('');
      alert('값이 입력되지 않았습니다.');
      return;
    }
  
    setNewTitle('');
    setNewBody('');
  
    const newCard = {
      id: Date.now(),
      title: newTitle,
      body: newBody,
      isDone: false
    };

    setCards([...cards, newCard]);
  };

  const removeCard = (id) => {
    const filteredCards = cards.filter(card => card.id !== id);
    setCards(filteredCards);
  };

  const handleComplete = (id) => {
    const updatedCards = cards.map(card => {
      if (card.id === id) {
        return { ...card, isDone: true };
      }
      return card;
    });
    setCards(updatedCards);
  };

  const handleCancel = (id) => {
    const updatedCards = cards.map(card => {
      if (card.id === id) {
        return { ...card, isDone: false };
      }
      return card;
    });
    setCards(updatedCards);
  };

  return (
    <>
      <form onSubmit={addCard}>
        <div className="input-group">
          <span>제목</span> <input type="text" value={newTitle} className="input-title" onChange={(e) => setNewTitle(e.target.value)} placeholder="제목" />
          <span>내용</span> <input type="text" value={newBody} className="input-body" onChange={(e) => setNewBody(e.target.value)} placeholder="내용" />
          <button type="submit" className="submit-button">작성하기</button>
        </div>
      </form>
      
      <div>
        <h1>Working!</h1>
        <ul>
          {cards.filter(card => !card.isDone).map(card => (
            <li key={card.id} className="card-item">
              <h2>{card.title}</h2>
              <h3>{card.body}</h3>
              <div>
                <button className="delete-button" onClick={() => removeCard(card.id)}>삭제</button>
                <button className="done-button" onClick={() => handleComplete(card.id)}>완료</button>
              </div>
            </li>
          ))}
        </ul>
      </div>
      
      <div>
        <h1>Done!</h1>
        <ul>
          {cards.filter(card => card.isDone).map(card => (
            <li key={card.id} className="card-item">
              <h2>{card.title}</h2>
              <h3>{card.body}</h3>
              <div>
                <button className="delete-button" onClick={() => removeCard(card.id)}>삭제</button>
                <button className="cancel-button" onClick={() => handleCancel(card.id)}>취소</button>
              </div>
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

export default App;
```

### 분리한 최종 코드 (컴포넌트를 분리)

App.jsx

```jsx
// src/App.jsx
import { useState } from 'react';
import './App.css';
import Input from './components/Input';
import CardSection from './components/CardSection';

function App() {
  const [cards, setCards] = useState([]);
  const [newTitle, setNewTitle] = useState('');
  const [newBody, setNewBody] = useState('');

  const addCard = (e) => {
    e.preventDefault();
    if (newTitle === '' || newBody === '') {
      setNewTitle('');
      setNewBody('');
      alert('값이 입력되지 않았습니다.');
      return;
    }
    setNewTitle('');
    setNewBody('');

    const newCard = {
      id: Date.now(),
      title: newTitle,
      body: newBody,
      isDone: false
    };

    setCards([...cards, newCard]);
  };

  const removeCard = (id) => {
    const filteredCards = cards.filter(card => card.id !== id);
    setCards(filteredCards);
  };

  const handleComplete = (id) => {
    const updatedCards = cards.map(card => {
      if (card.id === id) {
        return { ...card, isDone: true };
      }
      return card;
    });
    setCards(updatedCards);
  };

  const handleCancel = (id) => {
    const updatedCards = cards.map(card => {
      if (card.id === id) {
        return { ...card, isDone: false };
      }
      return card;
    });
    setCards(updatedCards);
  };

  return (
    <>
      <h1>나만의 To Do List</h1>
      <form onSubmit={addCard}>
        <div className="input-group">
          <Input
          newLabel={'제목'}
          newValue={newTitle}
          setNewValue={setNewTitle} />
          <Input
          newLabel={'내용'}
          newValue={newBody}
          setNewValue={setNewBody} />
          <button
          type="submit"
          className="submit-button">
            작성하기
          </button>
        </div>
      </form>

      <div>
        <CardSection 
          newCards={cards.filter(card => !card.isDone)} 
          toggleClassName={'done-button'} 
          toggleText={'완료'} 
          h1Text={'Working! 🔥'} 
          handleToggle={handleComplete}
          removeCard={removeCard} 
        />
      </div>

      <div>
        <CardSection 
          newCards={cards.filter(card => card.isDone)} 
          toggleClassName={'cancel-button'} 
          toggleText={'취소'} 
          h1Text={'Done! ✅'} 
          handleToggle={handleCancel}
          removeCard={removeCard} 
        />
      </div>
    </>
  );
}

export default App;
```

컴포넌트1. CardSection.jsx

```jsx
// src/components/CardSection.jsx
function CardSection({ newCards, toggleClassName, toggleText, h1Text, handleToggle, removeCard }) {
    return (
      <>
        <h1>{h1Text}</h1>
        <ul>
          {newCards.map(card => (
            <li key={card.id} className="card-item">
              <h2>{card.title}</h2>
              <h3>{card.body}</h3>
              <div>
                <button className="delete-button" onClick={() => removeCard(card.id)}>삭제</button>
                <button className={toggleClassName} onClick={() => handleToggle(card.id)}>{toggleText}</button>
              </div>
            </li>
          ))}
        </ul>
      </>
    );
  }
  
  export default CardSection;
```

컴포넌트2. input.jsx

```jsx
function Input({newLabel, newValue, setNewValue}) {
    return(
        <>

        <span>{newLabel}</span>
        <input
        type="text"
        value={newValue}
        className="input-title"
        onChange={(e) => setNewValue(e.target.value)}
        placeholder={newLabel}
        />

        </>
    )
}

export default Input
```

## 제작 순서

만들고자 하는 것은 간단한 To Do List 사이트


[배포주소](https://bootcamp-0517.vercel.app)


### Vite 환경으로 프로젝트 세팅하기

```
// 작업할 프로젝트 폴더 만들기

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

### 프로젝트 파일에서 불필요한 코드 지우기

위 명령어로 세팅이 완료되었다면,


App.jsx, App.css, Index.css., Index.html 필요없는 부분 지우기.


App.jsx에서 필요한 부분은 다음과 같음.

```jsx
import { useState } from ".react"

function App () {
// 이 영역에 자바스크립트 작성

        return (
            <>
            {/* 이 영역에 마크업 작성 */}
            </>
        )
    }
}
```

## Pseudo Code

1. `할 일 제목`과 `할 일 내용`을 사용자에게 입력받을 영역을 그리고, 버튼을 만든다.
2. `작성하기` 버튼을 누르면 카드가 `Working` 영역에 만들어진다.
3. `input` 영역 아래로 `Working` 영역과 `done` 영역을 만든다.
4. 할 일 카드의 기본 형태를 jsx에 마크업 한다.
5. input 영역과 할 일 카드의 기본 서식을 CSS로 꾸민다.
6. 할 일 카드에 들어갈 기본 내용을 배열로 만든다. `id`, `title`, `body`, `isDone`을 key로 하는 객체를 배열의 인덱스로 예시로 명시한다. 여기서 `isDone`은 할 일을 등록하면, 할 일이 안 끝났다는 의미에서 `isDone`으로 만든 후 기본값으로 `false`를 주고 시작한다.
7. 만약 할 일이 완료되었다면 `Working`에 그려진 할 일 카드에서 완료 버튼을 눌렀을 때 `Done` 영역으로 옮겨지게 한다.
8. 사용자에게 `input`에서 입력받은 값을 처음에 만든 배열에서 각 인덱스 객체의 value로 넣어준다.
9. 이렇게 입력받은 값을 기존 배열을 유지한 채로, 즉 불변성을 유지한 채로 계속 배열을 쌓아가도록 코드를 작성한다.
10. 코드가 완성되면 jsx 부분에서(화면을 그리는 부분)에서 컴포넌트로 분리할 수 있는 단위가 무엇이 있는지 고민한다.
11. 그렇게 분리한 컴포넌트가 다른 곳에서도 반복된다면 `props`를 사용해서 재사용 할 수 있을지 고민해본다.

## 코드 작성

### 뼈대 갖추기

```jsx
//App.jsx

// state를 사용하기 위해서 import를 하고 시작한다.
import { useState } from "react";
// 원래는 index.html에서 전역 CSS를 적용시키고,
// App 컴포넌트의 구체적인 CSS는 App.css에서 작성한 뒤
// 아래처럼 import해야 함. 그러나 본인은 페이지가 하나 뿐이라
// 편의상 index.css에서만 작업함.
import './App.css';

// App 컴포넌트는 App 함수 내부에서 작성하기 때문에 뼈대를 갖춤.
function App() {

    // 이 부분에 JavaScript 코드를 작성함.
    
    return (
        <> {/*빈 div의 역할을 하는 부모 빈 태그를
        작성함. Fragment라고 부름.*/}

        {/* return문 내부에 소괄호를 열고 마크업을 작성함 */}

        </>
    )

}

```

### 화면 그리기 (마크업 작성)

화면을 먼저 구성해야 기능을 어떻게 넣을 것인지 감이 오고, `className`이나 `id`로 위치를 지정할 수 있기 때문에 마크업부터 작성한다.

#### jsx 영역 그리기

```jsx
// App.jsx
// return문 내부

return (
    <>
<form>
  <div className="input-group">
    <span>제목</span> 
    <input type="text" className="input-title" placeholder="제목" />
    <span>내용</span> 
    <input type="text" className="input-body" placeholder="내용" />
    <button type="submit" className="submit-button">작성하기</button>
  </div>
</form>

<div>
  <h1>Working!</h1>
  <ul>
    <li className="card-item">
      <h2>제목</h2>
      <h3>내용</h3>
      <div>
        <button className="delete-button">삭제</button>
        <button className="done-button">완료</button>
      </div>
    </li>
  </ul>
</div>

<div>
  <h1>Done!</h1>
  <ul>
    <li className="card-item">
      <h2>제목</h2>
      <h3>내용</h3>
      <div>
        <button className="delete-button">삭제</button>
        <button className="cancel-button">취소</button>
      </div>
    </li>
  </ul>
</div>
    </>
)
```

jsx에서는 `class`가 아니라 `className`으로 속성을 준다.


이 점을 빼면 HTML과 같은 형태로 작성을 한다.

#### 반복되는 부분 확인하기

jsx, 즉 화면을 그리는 부분에서 반복되는 요소는 `input` 제목, 내용 입력하는 영역 두 개, Working, Done 카드가 그려지는 영역 두 개가 반복된다.

```jsx
<form>
  <div className="input-group">
    <span>제목</span> 
    <input type="text" className="input-title" placeholder="제목" /> {/* 반복 1 */}
    <span>내용</span> 
    <input type="text" className="input-body" placeholder="내용" /> {/* 반복 2 */}
    <button type="submit" className="submit-button">작성하기</button>
  </div>
</form>
```

```jsx
{/* 반복 1 */}
<div>
  <h1>Working!</h1>
  <ul>
    <li className="card-item">
      <h2>제목</h2>
      <h3>내용</h3>
      <div>
        <button className="delete-button">삭제</button>
        <button className="done-button">완료</button>
      </div>
    </li>
  </ul>
</div>

{/* 반복 2 */}
<div>
  <h1>Done!</h1>
  <ul>
    <li className="card-item">
      <h2>제목</h2>
      <h3>내용</h3>
      <div>
        <button className="delete-button">삭제</button>
        <button className="cancel-button">취소</button>
      </div>
    </li>
  </ul>
</div>
```

#### 컴포넌트 분리

```jsx
// components라는 폴더를 만든 후 분리할 컴포넌트.jsx를 생성

// ./src/components/Input.jsx

// 분리할 jsx를 가져옴.
// CSS 호환을 고려하여 컴포넌트가 바인딩 될 부분의 상위 div는
// 그대로 두고 안의 내용만 가져옴.

function Input({newLabel, newValue, setNewValue}) {
// 분리한 컴포넌트의 이름으로 함수를 선언한다.
// 함수의 매개변수로는 크게 {} 중괄호로 묶고, 매개변수로는
// return문 (jax 영역) 내부에 바인딩 할 (데이터를 넘겨줄)
// 변수 또는 함수명을 입력한다.

    return(
        <>
        
        {/* 화면에 그려줄 뼈대는 아래와 같다.
        그리고 내부로 넘겨주는, 바인딩 된 값은
        App.jsx에서 정의하고 넘겨오는 값이다. */}

        <span>{newLabel}</span>
        
        {/* type="text"는 고정된 값이고 */}
        <input
        type="text"

        {/* App.jsx에서 넘겨 받을 값은 {}중괄호로 바인딩 한다. */}
        value={newValue}
        className="input-title"

        {/* onClick이나 onChange로 함수를
        넘겨줄 수도 있다. */}
        onChange={(e) => setNewValue(e.target.value)}
        placeholder={newLabel}
        />

        </>
    )
}

{/* 컴포넌트 모듈을 넘겨주어야 한다. */}
export default Input
```

위의 구성을 보면 간단하다. 컴포넌트로 만들 이름을 `function`으로 정의하고 함수를 선언한다.


그리고 `return`문을 작성하고, 그 안에서 반복되는 UI의 뼈대를 그려주고, 안에 들어갈 내용은 `App.jsx`에서 설정한 뒤 컴포넌트에서는 바인딩하여 받아오면 된다.


React에서는 일반적으로 부모컴포넌트(App.jsx)에서 데이터(props)와 상태(state)를 관리하고, 이를 자식컴포넌트에서 받아서 사용한다.

이렇게 단방향 데이터 흐름을 유지하여 앱의 상태를 쉽게 관리할 수 있게 된다.


자식 컴포넌트에서 함수를 선언하며 설정한 매개변수에는 {} 중괄호로 묶여있다. 이것은 props를 구조 분해 할당하여 데이터의 값에 쉽게 접근할 수 있도록 돕는 것이다.


```jsx
// App.jsx

// 자식 컴포넌트를 import 해야 한다.
import Input from './components/Input';

function App() {

    return (
        <>

      <h1>나만의 To Do List</h1>
      <form onSubmit={addCard}>
        <div className="input-group">
          <Input
          newLabel={'제목'}
          newValue={newTitle}
          setNewValue={setNewTitle} />
          <Input
          newLabel={'내용'}
          newValue={newBody}
          setNewValue={setNewBody} />
          <button
          type="submit"
          className="submit-button">
            작성하기
          </button>
        </div>
      </form>

        </>
    )
}
```

위 코드를 보면 속성을 한줄 씩 분리해서 썼지만 결국

```jsx
<h1></h1>
<form>

<input/>
<input/>

</form>
```

위와 같이 몇 개 태그가 없는 상태이다. `input` 태그에 붙여 준 속성은 자식 컴포넌트로 넘겨 줄 데이터, 즉 `props`라고 한다.


아직 상태를 정의하지 않았기 때문에 현재 위 태그에서 속성의 값으로 사용한 `{newBody}` 등은 어떤 값인지 유추할 수 없지만,


바뀌지 않는 고정값은 '' 따옴표로 문자열 처리 하였고, 자식 컴포넌트에서 props로 받을 값이 `{newLabel}`이라면 부모 컴포넌트에서 `newLabel={넘겨줄값}`으로 설정해서 데이터를 흘려 보냈다.


이런 원리로 나머지도 재사용 가능하도록 컴포넌트로 분리하면 되고, 몇 가지 더 살펴볼 점을 살펴보겠다.


```jsx
// App.jsx
      <div>
        <CardSection 
          newCards={cards.filter(card => !card.isDone)} 
          toggleClassName={'done-button'} 
          toggleText={'완료'} 
          h1Text={'Working! 🔥'} 
          handleToggle={handleComplete}
          removeCard={removeCard} 
        />
      </div>
```

위의 코드는 Working 영역과 Done 영역으로 나뉘는 부분을 표시하고 있는데 역시 `<CardSection/>` 으로 자식 컴포넌트를 불러오는 것이고, 속성으로 설정한 값들은 자식 컴포넌트로 넘겨주는 데이터들이다.


그런데 특이한 점은 `newCards`라는 속성을 보면 {} 중괄호 내부에 함수가 작성되어 있다.


`cards`라는 배열을 filter로 돌려서 인덱스 하나 하나인 `card`의 `isDone`이라는 key의 value가 false일 경우를 새로운 배열로 반환하라는 함수이다. 그렇게 반환된 배열을 `newCards`라는 변수에 담은 것과 같다.

```jsx
newCards = {cards.filter(card => !card.isDone)}
```

위 내용에서 중괄호는 '자바스크립트 표현식을 사용하겠다'라는 의미이고, 내부에 자바스크립트 표현식을 실제로 작성한 형태인데,

```jsx
cards.filter(card => card.isDone === false)
```

이와 같은 코드이고, filter의 콜백함수로 화살표 다음 코드블럭은 조건식이 들어간다.

```jsx
array.filter(element => condition)
```

그리고 이 데이터를 받는 자식 컴포넌트로 가보면

```jsx
// CardSection.jsx

<h1>{h1Text}</h1>
        <ul>
          {newCards.map(card => (
            <li key={card.id} className="card-item">
              <h2>{card.title}</h2>
              <h3>{card.body}</h3>
              <div>
                <button className="delete-button" onClick={() => removeCard(card.id)}>삭제</button>
                <button className={toggleClassName} onClick={() => handleToggle(card.id)}>{toggleText}</button>
              </div>
            </li>
          ))}
        </ul>
```

부모 컴포넌트에서 `filter`를 돌려서 새롭게 반환된 배열인 `newCards`에 `map`을 돌려 화면을 그리고 있다.


`map` 함수는 `filter` 함수와 다르게 화살표 다음 조건식이 오는 것이 아니라 화면을 렌더링 할 코드를 return한다.


#### 상태로 정의하기

사실 이 부분이 가장 먼저 언급되었어야 했는데, 본인은 jsx를 먼저 완성한 후 상태를 정의했기 때문에 이 문서는 순서를 바꾼 듯 작성하였고, 위 코드들은 정의된 상태를 모르면 이해할 수 없다.


상태(state)로 관리한다는 개념은,

- 사용자가 입력한 값
- 서버로부터 받아온 값
- 버튼 등 클릭 이벤트 등으로 변화되는 값

위 처럼 '변하는' 값들을 일반 변수 선언이 아니라 `state`로 정의하여 관리한다.


그런데 핵심은 재 렌더링 하는 요소에만 상태를 정의하는 것이다. React는 상태가 변화할 때 HTML처럼 페이지 전체를 리로드하는 것이 아니라 상태가 변화된 곳만 리로드 한다는 특징이 있다. 그렇기 때문에 사용자가 더 빠른 속도로 페이지를 이용할 수 있다는 장점도 있다.


```jsx
const [cards, setCards] = useState([]);
const [newTitle, setNewTitle] = useState('');
const [newBody, setNewBody] = useState('');
```

본인이 상태로 정의한 것은 크게 세 가지다.

첫번째로 화면에 그려지는 카드에 들어가는 모든 요소를 배열로 담은 것이다. 여기에는 사용자가 `input`에 입력하는 값을 받아 `newTitle`과 `newBody`에 state로 정의할 것이고, 이것은 **(1) 값이 계속 바뀌고 (2) 재 렌더링(화면에 바뀐 값으로 계속 출력되어야 함) 되는 값들만 상태로 관리**하는 것이다.


예를 들어 카드의 기본 프로퍼티 중 id 또한 유니크한 값을 넣기 위해 `Date.now()`라는 메서드를 사용하지만, 이는 카드에 재 렌더링 되는 부분이 아니기 때문에 이는 상태로 관리하지 않는다. 마찬가지로 `isDone`이라는 부분도 기능을 위해 필요한 프로퍼티일 뿐 재 렌더링 되는 부분은 아니기에 상태로 관리하지 않는다.


상태로 관리하기 위해선 `useState` 훅을 사용하며, 앞은 변수명, 뒤는 상태를 변경할 함수명을 명시한다.

```jsx
const 
```


그리고 위 코드에서 개선할 점이 있다.

```jsx
const [변수명, 상태를 변경하는 함수명] = useState(변수의 초기값);
```

### 자바스크립트 기능 구현하기

`function` 내부에서, `return` 문 위에서 자바스크립트를 작성할 수 있다.

#### 카드 추가 함수 정의

```jsx
const addCard = (e) => {
    // form 이 제출될 때 값이 입력되지 않아도 코드가
    // 진행되지 않도록 preventDefault를 사용한다.
    e.preventDefault();
    // 만약 newTitle과 newBody가 빈 스트링이라면
    // 코드블럭을 실해한다.
    if (newTitle === '' || newBody === '') {
      // 빈 값이 있으면 코드 실행을 하면 안 되니
      // input이 빈 값인지 검사하기 위해 유효성 검사를
      // 논리합 연산자를 통해서 한다.
      setNewTitle('');
      setNewBody('');
      // 각 input에 사용자가 입력한 값을 깔끔하게 지워준다.
      alert('값이 입력되지 않았습니다.');
      // 값이 입력되지 않았다는 alert을 띄워준다
      return;
      // 그리고 addCard라는 함수를 더이상 진행시키지 않고
      // return 시킨다.
    }
    setNewTitle('');
    setNewBody('');
    // if문 밖에서는 값이 둘 다 입력되었을 때
    // 카드를 추가하는 코드를 작성해준다.
    // 마찬가지로 input의 내용을 지워준다.

    const newCard = {
      id: Date.now(),
      title: newTitle,
      body: newBody,
      isDone: false
    };
    // 그리고 newCard라는 변수를 만들고 객체 형태로
    // 카드 한장마다 위 정보를 담는다.
    // 위 카드는 cards라는 카드 전체의 배열에
    // 하나의 인덱스로 들어갈 내용이다.
    // id는 유니크한 아이디 값을 주기 위해 Date.now()라는
    // 메서드를 사용하였고,
    // title과 body는 input에서 받아온 데이터를
    // 넣어준다. input에서 받는 값은 string 타입이다.
    // isDone은 처음 카드를 등록했을 때는 할 일을
    // 하기 위해 등록한 것이니 완료 상태는 false로
    // 설정한다.

    setCards([...cards, newCard]);
    // 이 부분은 불변성을 유지하기 위한 부분이다.
    // cards라는 원본배열은 그대로 유지하면서
    // newCard라는 새로운 객체만 추가하는 방법인데
    // 전체적으로 배열 형태로 만들기 위해 대괄호를
    // 씌워주고, ...cards처럼 spread operator로
    // 원본 배열의 대괄호를 벗겨 준 후 누적시켜 저장시킨다.
  };
```

#### 카드 제거 함수 정의

```jsx
const removeCard = (id) => {
    // 카드 제거 함수를 만들 건데 id를 매개변수로 받고
    // 이 id는 removeCard라는 함수가 실행 되는 곳
    // 즉 jsx 영역에서 버튼 어딘가에서 onClick 등으로
    // 받을 것이다.
    const filteredCards = cards.filter(card => card.id !== id);
    // filter 함수를 사용해서 filteredCards라는 새로운
    // 배열을 만들어내는데, 이 새로운 배열은 cards라는
    // 원본 배열을 filter하여 card라는 매개변수 즉
    // 인덱스 하나하나를 돌며 객체의 key 중 id라는 값을
    // 찾아 현재 id로 받은 값과 같은지 조건식으로
    // 검사한다. 그리고 현재 아이디와 다른 값들만
    // 새롭게 배열로 만들어서
    setCards(filteredCards);
    // 그 배열을 원본 배열을 변경하는 함수로 전달한다.
    // 그러면 마치 onClick으로 붙어있는 그 카드의
    // id의 카드만 삭제된 것처럼 보인다.
  };
```

#### 완료 버튼과 취소 버튼 정의

```jsx
const handleComplete = (id) => 
// 완료 버튼을 누르면 isDone의 상태를 true로 만드는
// 함수를 만든다. id는 역시 onClick으로 달린 곳에서
// 그 카드의 id를 받을 것이다.

    // 새로운 배열로 뱉어내는데, cards라는 원본 배열을
    // map 메서드로 돌면서 특정 요소만 찾아 수정한다.
    const updatedCards = cards.map(card => {
        /// 만약 현재 전달받은 매개변수가 cars라는 배열에서
        // card 즉 인덱스 하나 하나 돌면서 id값만 검사해서
        // 일치하는 것들만 return 시킨다.
        // 근데 카드 한 장이니 일치하는 것들도 없다.
        // 지금 완료 버튼이 눌린 그 카드 딱 한 장만
        // map 메서드로 원본 배열을 순회돌며 찾아내는
        // 것이다. 그렇게 찾아서 idDone의 상태만
        // 바꿔주는 것이다.
        if (card.id === id) {
            // 역시 카드 하나하나를 새로운 객체로 리턴 시키는데
            // 누적시키기 위해, 즉 원본 객체의 불변성을
            // 유지시키기 위해 spread operator를
            // 사용하여 펼쳐주고,
            // isDone의 상태만 true로 바꿔준다.
            return { ...card, isDone: true };
        }
        return card;
        // 이렇게 바뀐 card를 다시 한 번 return 해주고
    });
    setCards(updatedCards);
    // 속성이 바뀐 card를 return받은 원본 배열에서
    // 그 하나만 수정된 새로운 배열을 cards 원본배열을
    // 수정하는 함수의 매개변수로 전달한다.
}
```

위에서 버튼에 달리는 기능들마다 지금 버튼이 눌린 카드의 id를
매개변수로 받아오는데 이게 가능한 이유는 jsx에서 카드를 만들 때 `li` 태그를 이용해 하나씩 만들었고, 이 `li`태그에서 가장 먼저 `key`라는 속성값을 주고 원본 배열에서 `card.id`를 받아오도록 설정했기 때문이다.


```jsx
{/* 자식컴포넌트 CardSection */}
        <h1>{h1Text}</h1>
        <ul>
          {newCards.map(card => (
            <li key={card.id} className="card-item">
              <h2>{card.title}</h2>
              <h3>{card.body}</h3>
              <div>
                <button className="delete-button" onClick={() => removeCard(card.id)}>삭제</button>
                <button className={toggleClassName} onClick={() => handleToggle(card.id)}>{toggleText}</button>
              </div>
            </li>
          ))}
        </ul>
```