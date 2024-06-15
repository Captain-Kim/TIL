## 리덕스 툴킷 설치하기

```
yarn add 
```

## main.jsx에서 스토어 설정하기 (또는 index.jsx)

```jsx
// src/main.jsx

import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store/store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## 파일 생성하기

```
src/
├── store/
│   ├── store.js         // Redux store 설정 파일
│   ├── userSlice.js     // user slice 파일
│   ├── anotherSlice.js  // another slice 파일
│   └── ...              // 추가적인 slice 파일들
└── ...
```

## 필요한 slice 만들기

```js
// src/store/userSlice.js

import { createSlice } from '@reduxjs/toolkit';

const userSlice = createSlice({
  name: 'user',
  initialState: { name: '장원영', visit: 21 },
  reducers: {
    changeName(state) {
      state.name = '안유진';
    },
    increaseVisit(state) {
      state.age += 1;
    }
  }
});

export const { changeName, increaseVisit } = userSlice.actions;
export default userSlice.reducer;
```

```js
// src/store/store.js

import { configureStore } from '@reduxjs/toolkit';
import userReducer from './userSlice';
// 다른 슬라이스가 더 있다면 그 슬라이스의 리듀서도 필요에 따라 import

const store = configureStore({
  reducer: {
    user: userReducer,
    // 다른 슬라이스가 더 있다면 리듀서도 여기 추가
  },
});

export default store;
```

## 필요한 컴포넌트에서 사용하기

```jsx
// src/App.js

import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { changeName, increaseVisit } from './store/userSlice';

const App = () => {
  const dispatch = useDispatch();
  const user = useSelector((state) => state.user);

  return (
    <div>
      <h1>회원 방문정보 기록</h1>
      <p>이름: {user.name}</p>
      <p>나이: {user.age}</p>
      <button onClick={() => dispatch(changeName())}>안유진으로 바꾸기</button>
      <button onClick={() => dispatch(increase())}>방문횟수 증가시키기</button>
    </div>
  );
};

export default App;
```

- useSelector: store에서 상태를 꺼내오는 훅
- useDispatch: store에서 상태 변경 함수를 꺼내오는 훅 (액션을 디스패치한다)