## 이벤트 핸들러란?

웹 페이지 내에서 마우스를 클릭하거나, 스크롤을 올리고 내리거나, 키보드를 누르는 등의 이벤트가 발생했을 때 함수를 실행되는 함수를 말한다.


```jsx
import React from 'react';

function App() {
  // 이벤트 핸들러 함수 정의
  function handleClick() {
    alert('버튼이 클릭되었습니다!');
  }

  return (
    <div>
      {/* 버튼에 onClick 이벤트 핸들러 설정 */}
      <button onClick={handleClick}>클릭하세요</button>
    </div>
  );
}

export default App;
```

위와 같이 버튼 태그에 {} 중괄호로 함수를 바인딩하여 호출할 수 있다.

## React에서 자주 사용되는 이벤트 핸들러

### onClick (버튼)

특정 요소가 클릭되었을 때 동작하는 함수.

```jsx
function handleClick() {
  console.log('버튼을 클릭했습니다.');
}

<button onClick={handleClick}>눌러보세요.</button>
```

### onChange (input)

input과 같은 입력 필드의 값이 변경되었을 때 동작하는 함수.


주로 state의 값을 바꾸어주는 setState와 같은 상태 변경 함수에 `(e) => setState(e.target.value)`와 같이 input 값을 밀어 넣어 줄 때 사용한다.

```jsx
function handleChange(event) {
  console.log('다음과 같이 입력 하셨습니다:', event.target.value);
}

<input type="text" onChange={handleChange} />
```

### onSubmit (form)

폼이 버튼 등을 클릭해서 제출될 때 실행되는 함수.

```jsx
function handleSubmit(event) {
  event.preventDefault(); // 폼 제출 시 페이지가 리로드 되는 것을 막음
  console.log('입력한 내용이 제출되었습니다.');
}

<form onSubmit={handleSubmit}>
  <button type="submit">제출하기</button>
</form>
```

### onMouseEnter / onMouseLeave (div 등)

특정 요소에 마우스가 들어오거나 나갈 때 실행하는 함수.

```jsx
function handleMouseEnter() {
  console.log('마우스가 들어 와 있습니다.');
}

function handleMouseLeave() {
  console.log('마우스가 나갔습니다.');
}

<div onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
  마우스를 올려보세요.
</div>
```

### onKeyUp / down (input)

키보드를 누르거나 뗄 때 실행하는 함수.

```jsx
function handleKeyDown(event) {
  console.log('다음 키가 눌렸습니다:', event.key);
}

function handleKeyUp(event) {
  console.log('다음 키가 떼어졌습니다:', event.key);
}

<input type="text" onKeyDown={handleKeyDown} onKeyUp={handleKeyUp} />
```

## 이벤트 핸들러를 전달하는 두 가지 방식

### 외부에서 핸들 함수 정의 후 이벤트 핸들러에서 호출만 하기

```jsx
function handleClick() {
  console.log('버튼을 눌렀습니다.');
}

return(
    <>
<button onClick={handleClick}>눌러보세요.</button>
    </>
)
```

컴포넌트가 재 렌더링 될 때마다 새로 로드되지 않는다. 따라서 메모리 관리에 효율적이다.

### 이벤트 핸들러 내부에서 직접 인라인으로 핸들 함수 정의하기

다만 컴포넌트가 재 렌더링 되더라도 인라인으로 핸들 함수를 정의해야 할 때도 있다.


핸들 함수 외 그 이벤트 핸들러에서만 추가적으로 실행해야 하는 로직이 있을 때이다.

```jsx
<button onClick={(e) => {
  alert('버튼이 클릭되었습니다.');
  handleClick();
}}>Click me</button>
```

만약 다른 곳에서는 alert을 띄울 필요가 없는데 이 버튼에서만 그러한 액션이 필요하다면, `handleClick`이라는 핸들 함수를 여러 군데서 갖다 쓰고 있다면 더더욱 함수 내용으로 `alert`을 넣을 수가 없다. 따라서 여기에서만 추가적으로 사용해야 하기 때문에 이런 경우에는 위와 같은 형태로 작성한다.


