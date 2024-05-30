## Redux의 기본 개념

### 액션

상태에 어떤 변화가 필요하면 action이라는 것이 발생한다. action은 하나의 객체로 표현된다. 이 액션 개체는 아래와 같은 형태로 이루어져 있다.

```jsx
{
    type: 'TOGGLE_VALUE'
}
```

액션을 조금 더 쉽게 설명하자면 스토어라는 상태 중앙 저장소에 상태에 어떠한 변화가 일어났다고 보내는 메시지의 개념이다.


액션은 자바스크립트 객체로 이루어져 있고, 리덕스에게 "이런 일이 발생했으니, 데이터를 바꿔주세요!"라고 요청하는 것이다.

#### 타입(type) 필드

개발자가 자유롭게 작명하는 액션 객체의 이름임. 이를 통해 나중에 상태 업데이트 시 참고할 수 있음.

```jsx
{
    type: 'ADD_TODO',
    data: {
        id: 1,
        text: '리덕스 배우기'
    }
}

{
    type: 'CHANGE_INPUT',
    text: '안녕하세요.'
}
```

type의 작명을 보면 동사로 시작한다. 즉 어떠한 액션이 발생했다고 알려주는 것이다.


두번째 프로퍼티로 쓰인 data나 text는 type에서 알려주는 저 액션이 발생했는데, 그래서 어떠한 변화가 일어난 것인지 부연설명하는 정보로, **payload**라고 말한다. data나 text가 아니라 payload로 프로퍼티를 주어도 된다. 다만 시멘틱하지 않을 뿐.


위의 액션 객체 예시에서 첫번째 ADD_TODO를 보면 ADD_TODO라는 액션이 발생했고, text라는 객체의 내용으로 상태가 바뀌었다고 알게 되는 것으로, 이것을 토대로 스토어는 상태를 변경할 수 있음.

#### 액션 생성 함수 (action creator)

액션 객체를 만들어 주는 함수임. 위에서 설명한 액션을 만들어주는 함수임. 비교해보면 이해가 됨.


함수의 이름은 액션의 이름이고, 매개 변수가 payload가 됨. 그리고 return 해주는 값이 payload의 value임. 즉 아래 예시에서는 ADD_TODO라는 액션을 만들기 위해 addTodo라는 함수를 선언한 것.

```jsx
function addTodo(data) {
    return {
        type: 'ADD_TODO',
        data
    };
}

const myTodo = {
    id: 1,
    data: '쿠팡에서 장보기'
}
addTodo(myTodo); 

// 화살표 함수도 가능
const changeInput = text => (
    {
        type: 'CHANGE_INPUT',
        text
    }
);

const myInput = '안녕하세요, 장원영입니다.'
changeInput(myInput);
```

### 리듀서(reducer)

- 리듀서는 변화를 일으키는 함수임.
- 액션을 만들어서 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아옴. 그리고 두 값을 참고하여 새로운 상태를 만들어서 반환해줌.
- 즉 액션은 '~~ 이렇게 상태를 바꿔주세요'라고 말하는 주문서인 것이고, 그 주문서의 내용을 토대로 리듀서라는 주방장이 요리를 하며 상태를 바꿔주는 것임.
- 리듀서라는 주방장은 재료들이 무엇인지(현재 상태)가 무엇인지 알아야 하고, 액션이라는 주문서를 받아본 뒤 주문서의 내용을 토대로 상태를 변경해주는 것임. 그런데 여기서 중요한 것은 기존 상태를 바꾸는 것이 아니라 기존 상태는 불변성을 유지하며 그대로 두고, 새로운 상태를 반환한다는 것임.

```jsx
// 리듀서 함수 정의
const initialState = {
    counter: 1
}
function reducer(state = initialState, action) {
    switch (action.type) {
        case INCREMENT:
            return {
                ...state,
                counter: state.counter + action.payload
            };
            default:
                return state;
    }
}

// 액션 정의
const addCountAction = {
    type: INCREMENT, // 문자열이 아닌 상수임.
    payload: 1 // payload가 액션의 내용을 나타내는 관습적 표현으로, counter 같은 이름보단 더 시멘틱함.
}

// 리듀서 호출(변수에 할당하여 즉시 호출)
const newState = reducer(initialState, addCountAction);
```

위 예시를 보면 initialState로 counter가 1로 되어 있는데, reducer 함수에서 현재 state를 initialState 즉 1로 전달받았고, 두번째 매개 변수로 action을 받았는데,


switch문은 정해진 키워드이고, 개발자가 작명하는 것은 case문에서의 액션 type명(여기서는 INCREMENT)이고 return문으로 바꿔줄 값을 말하고 있는데, 현재 카운터 값을 바꿔버리는 것이 아니라 현재 값을 유지시킨 상태로 +1만 시켜서 새로운 state로 return 하는 형태임.

### 스토어(store)

- 프로젝트에 리덕스를 적용하기 위해 필요한 것.
- 전역상태를 저장하는 저장소 개념임.
- 한 개의 프로젝트에서는 무조건 하나의 스토어만 가질 수 있음.
- 스토어에는 현재 애플리케이션의 상태와 리듀서가 포함됨.

#### 디스패치(dispatch)

- 스토어의 내장 함수 중 하나.
- 액션을 발생시키는 함수임.
- `dispatch(action)` 과 같은 형태로 액션 객체를 파라미터로 넣어서 호출함.
- 이 함수가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만듦.

#### 구독(subscribe)

- 스토어의 내장 함수 중 하나.
- subscribe 함수 안에 리스너 함수를 파라미터로 넣어 호출 시 이 리스너 함수가 액션이 디스패치 되어 상태가 업데이트 될 때마다 호출됨.

```jsx
const listener = () => [
    console.log('상태가 업데이트 되었습니다.');
]
const unsubscribe = store.subscribe(listener);

unsubscribe(); // 추후 구독을 비활성화할 때 호출하는 함수
```

## Redux의 세 가지 규칙

### 단일 스토어

- 하나의 애플리케이션, 즉 하나의 프로젝트에는 하나의 스토어만 만드는 것이 권장됨.
- 업데이트가 빈번한 것은 따로 분리해서 만들 순 있겠으나 상태관리가 복잡해져 권장되지 않음.

### 읽기 전용 상태

- 상태를 업데이트 할 때 불변성을 유지하며 기존 객체는 건드리지 않고 새로운 객체를 생성해주어야 함.
- 리덕스에서 불변성을 유지해야 하는 이유는 리덕스 내부에서 얕은 비교(shallow equality)를 하기 때문임. 객체의 변화를 감지할 때 깊은 비교를 하면 성능 저하가 오기 때문에 이런 방식을 채택하고 있음.

### 리듀서(상태 변화 함수)는 순수한 함수여야 함

리듀서 작성 시 주의사항은 아래와 같음.

- 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받음.
- 파라미터 외의 값에 의존하면 안 됨.
- 이전 상태는 건드리지 않고 변화를 준 새로운 상태 객체를 만들어서 반환해야 함.
- 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과 값을 반환해야 함.

가령 리듀서 함수 내에서 랜덤한 값을 만들거나 `date.now()`와 같은 메서드를 사용하면 파라미터를 같게 지정하더라도 매번 다른 결과를 내기 때문에 사용하면 안 됨.


만약 랜덤한 값을 만들고 싶다면 리듀서가 아닌 리듀서 외부에서 처리해주어야 함.

## Redux의 장점

- 컴포넌트에서 상태를 관리하던 것을 분리해서 스토어에서 상태 관리를 일괄적으로 하기 때문에 유지보수하기가 편리함.
- 여러 컴포넌트에서 동일한 상태를 공유할 때 props drilling이 발생하지 않아 간편함.
- 업데이트가 필요한 컴포넌트만 리렌더링 되도록 최적화 해줄 수 있음.

## Redux를 사용하는 절차

프로젝트 준비 -> 프레젠테이셔널 컴포넌트 작성 -> 리덕스 관련 코드 작성 -> 컨테이너 컴포넌트 작성 -> connect 대신 Hooks 사용

## 실제로 사용해보기

### 프로젝트 셋업

CRA나 vite를 이용해서 프로젝트를 셋업한다.

### Redux 패키지 설치

```
npm install react-redux
```

### 컴포넌트 분리하기

#### 프리젠테이셔널 컴포넌트

- 프리젠테이셔널 컴포넌트란 상태 관리를 하진 않고 상태를 받아와서 UI를 보여주기만 하는 컴포넌트를 말함.
- src/components 경로에 셋업함.

#### 컨테이너 컴포넌트

- 리덕스와 연동된 컨테이너로, 상태를 받아오면서 리덕스 스토어에 액션을 디스패치하기도 하는 컴포넌트를 말함.

### 예제

1. 카운터 컴포넌트 만들기

```jsx
// components/Counter.jsx

const Counter = ( { number, onIncrease, onDecrease } ) => {
    return (
        <div>
            <h1>{ number }/h1>
            <div>
                <button onClick={onIncrease}> +1 증가 </button>
                <button onClick={onDecrease}> -1 감소 </button>
            </div>
        </div>
    );
}

export default Counter;
```

2. 카운터 컴포넌트 앱 컴포넌트에서 렌더링 하기

```jsx
// App.jsx

import Counter from './components/Counter';

const App = () => {
    return (
        <div>
            <Counter number={0}/>
        </div>
    );
};

export default App;
```

3. 할 일 목록 컴포넌트 만들기

```jsx
// components/Todos.jsx

const TodoItem = ({ todo, onToggle, onRemove }) => {
    return (
        <div>
            <input type="checkbox"/>
            <span>샘플 텍스트입니다.</span>
            <button>삭제하기</button>
        </div>
    );
};

const Todos = ( { input, todos, onChangeInputm onInsert, onToggle, onRemove} ) => {
    const onSubmit = e => {
        e.preventdefault();
    };
    return (
        <div>
            <form onSubmit={onSubmit}>
                <input/>
                <button type="submit">등록하기</button>
            </form>
            <div>
                <TodoItem/>
                <TodoItem/>
                <TodoItem/>
                <TodoItem/>
                <TodoItem/>
            </div>
        </div>
    );
};

export default Todos;
```

함수형 컴포넌트를 파일 하나에 두 개를 선언했다. 두 개 각각 분리해도 된다.

4. 할 일 목록 리스트를 카운터 컴포넌트 아래에서 렌더링 하기

```jsx
// App.jsx

import Counter from './components/Counter';

const App = () => {
    return (
        <div>
            <Counter number={0}/>
            <hr/>
            <Todos/>
        </div>
    );
};

export default App;
```

5. 리덕스 관련 코드 작성하기 - 파일 트리 만들기

먼저 알아두어야 할 것은 리덕스를 사용하려면 액션 타입, 액션 생성 함수, 리듀서 코드를 세트로 작성해야 하고, 이를 각각의 디렉토리를 만들어 작성하는 것이 가장 전통적인 방법이다.


```js
// 예시 : 전통적인 구조
// 참고로 리덕스 파일은 jsx 구문이 없기에 js 확장자로 만든다.
actions
    counter.js
    todos.js
constants
    ActionType.js
reducer
    counter.js
    todos.js
```

그러나 새로운 액션이 생길 때마다 모든 파일을 수정해야 하기 때문에 번거로워 **Ducks 패턴**이 생겨났고, ducks 패턴에 의하면 파일 트리는 아래와 같음.

```
modules
    counter.js
    todos.js
```

6. counter 모듈 작성하기

만들어야 할 것은 액션 타입, 액션 생성 함수, 리듀서.


**액션 타입 정의하기**

```js
// modules/counter.js

// 액션 타입 정의하기 (대문자로 작명함, 문자열은 '모듈명/액션명')
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';
```

위와 같은 액션명은 유니크하지 않기 때문에 다른 모듈에서도 중복될 수 있는데, 문자열에 모듈명까지 넣어줌으로써 완전한 중복을 피하고 있음.


**액션 생성 함수 만들기**

```js
// modules/counter.js

// 액션 생성 함수 만들기
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = () => ({type: INCREASE});
export const decrease = () => ({type: DECREASE});
```

**초기 상태 및 리듀서 함수 만들기**

```js
// modules/counter.js
// counter 모듈의 초기 상태와 리듀서 함수 작성

const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const initialState = {
    number: 0
}

function counter(state = initialState.action) {
    switch (action.type) {
        case INCREASE"
            return {
                number: state.number + 1
            };
        case DECREASE:
            return {
                number: state.number + 1
            };
        default:
            return state:
    }
}

export default counter;
```