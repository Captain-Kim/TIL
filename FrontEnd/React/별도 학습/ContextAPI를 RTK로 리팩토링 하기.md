## GitHub 확인하기

자세한 수정 내역은 본인의 프로젝트 커밋 내역을 확인해보면 된다.
[링크](https://github.com/Captain-Kim/NBC-Homework-0614-/commit/79cffce5e8a1b3ca81d0455ba51469f6da4a0aa0#diff-d274a54187c91ba0f532df2a9e194e27ab50e988f5e4c33f5a7893918320c661)


## 기존 프로젝트 해부하기

```js
import React, { createContext, useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";

export const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [user, setUser] = useState(null);
  const [nickname, setNickname] = useState('');
  const navigate = useNavigate();

  useEffect(() => {
    const token = localStorage.getItem("accessToken");
    const storedUser = localStorage.getItem("user");
    if (storedUser) {
      const userObject = JSON.parse(storedUser);
      setNickname(userObject.nickname);
    }
    if (token) {
      setIsAuthenticated(true);
      try {
        const userObject = storedUser ? JSON.parse(storedUser) : null;
        setUser(userObject);
      } catch (error) {
        console.error("로컬 스토리지에서 user 객체를 불러오지 못했습니다요 왜냐면 값이 =>", error);
      }
    }
  }, []);

  const login = (token, user) => {
    localStorage.setItem("accessToken", token);
    localStorage.setItem("nickname", nickname)
    localStorage.setItem("user", JSON.stringify(user));
    setIsAuthenticated(true);
    setUser(user);
  };

  const logout = () => {
    localStorage.removeItem("accessToken");
    localStorage.removeItem("user");
    setIsAuthenticated(false);
    setUser(null);
    navigate('/login');
  };

  return (
    <AuthContext.Provider value={{ isAuthenticated, login, logout, user, setUser, nickname, setNickname }}>
      {children}
    </AuthContext.Provider>
  );
};
```

- 이 AuthContext는 유저의 인증 상태를 관리하기 위해 만들었다.
- main.jsx에서 Provider를 통해 App 컴포넌트에 상태를 내려주고 있다.
- 상태를 세 개 만들어서 관리하고 있다.
  - isAuthenticated : 유저의 인증 상태를 불리언 값으로 저장한다. 유저가 로그인 상태인지, 로그아웃 상태인지 알기 위함이다. 유저가 로그인에 성공해서 서버로부터 토큰을 받았을 때 그 토큰을 로컬 스토리지에 저장해서, 이 로컬 스토리지에 값이 있는지 여부로 판단한다.
  - user : 마이페이지에서 필요하다.
  - nickname : 헤더에 닉네임을 렌더링 하기 위함.
- 최초 마운트 되었을 때 useEffect 훅을 이용하여 로컬 스토리지에서 토큰을 꺼내면서 유저 정보도 꺼냄 -> 만약 유저 정보가 있다면(로그인이 되어 있는 상태라면) 유저 정보를 로컬 스토리지에서 꺼내온 유저 정보를 객체로 storedUser라는 변수에 값으로 초기화 함 -> 토큰이 있다면 isAuthenticated라는 상태, 즉 인증 상태를 true로 바꿈 -> 유저 인증 정보가 있어서 여기까지 완료되면 user라는 상태도 이 유저 정보로 덮어 씌움.
- login 함수
  - token과 user 정보를 매개 변수로 받는 로그인 함수를 생성함.
  - 로그인 함수를 실행하면 accessToken이라는 key를 로컬 스토리지에 만들고 유저의 토큰 값을 저장함.
  - nickname 필드도 만들어서 저장하고, user 정보도 객체로 풀어서 저장함.
  - 그리고 isAuthenticated 상태도 true로 전환함.
  - user 상태도 이 최신화된 유저 정보로 상태 변경함.
- logout 함수
  - 액세스 토큰과 user 정보를 로컬 스토리지에서 삭제하고, user 상태도 null로 초기화 시킴.
  - isAuthenticated 상태를 false로 바꿈.
  - login 페이지로 이동시킴.

### 전역 상태로 관리한 이유

#### isAuthenticated

사용자 인증 상태는 이 프로젝트의 모든 부분에 영향을 끼친다. 로그인이 되어 있지 않으면 기본적으로 사이트를 이용할 수 없게 해야 하기 때문에 모든 곳에서 isAuthenticated의 상태가 true인지 false인지 알아야 한다.

#### user

헤더와 마이페이지에서 유저의 정보가 필요하다.

#### nickname

가계부 각각의 아이템, Create 컴포넌트, 마이페이지, 헤더에서 렌더링하기 위해 필요하다.

#### login, logout

마이페이지, 헤더에서 필요하다. 또한 토큰이 만료되었을 때 강제로 로그아웃 시키는 로직까지 적용할 경우 App 컴포넌트에서도 필요할 수 있다.

## 리팩토링 하기

### 리덕스 패키지 설치

```
yarn add @reduxjs/toolkit react-redux
```

### 파일 구조 만들기

```
redux
    store.js
    authSlice.js
```

### authSlice 작성

```js
import { createSlice } from '@reduxjs/toolkit';

const authSlice = createSlice({
  name: 'auth',
  initialState: {
    isAuthenticated: false,
    user: null,
    nickname: '',
  },
  reducers: {
    login(state, action) {
      const { token, user } = action.payload;
      localStorage.setItem('accessToken', token);
      localStorage.setItem('user', JSON.stringify(user));
      state.isAuthenticated = true;
      state.user = user;
      state.nickname = user.nickname;
    },
    logout(state) {
      localStorage.removeItem('accessToken');
      localStorage.removeItem('user');
      state.isAuthenticated = false;
      state.user = null;
      state.nickname = '';
    },
    setNickname(state, action) {
      state.nickname = action.payload;
    },
    loadUserFromStorage(state) {
      const token = localStorage.getItem('accessToken');
      const storedUser = localStorage.getItem('user');
      if (storedUser) {
        const userObject = JSON.parse(storedUser);
        state.nickname = userObject.nickname;
      }
      if (token) {
        state.isAuthenticated = true;
        try {
          const userObject = storedUser ? JSON.parse(storedUser) : null;
          state.user = userObject;
        } catch (error) {
          console.error('로컬 스토리지에서 user 객체를 불러오지 못했습니다요 왜냐면 값이 =>', error);
        }
      }
    },
    updateUser(state, action) {
      const { nickname, avatar } = action.payload;
      if (state.user) {
        state.user.nickname = nickname;
        state.user.avatar = avatar;
      }
      state.nickname = nickname;
    },
  },
});

export const { login, logout, setNickname, loadUserFromStorage, updateUser } = authSlice.actions;
export default authSlice.reducer;
```

**코드 분석**

```jsx
import { createSlice } from '@reduxjs/toolkit';
```
createSlice 함수를 import한다. createSlice 함수는 슬라이스(상태)와 리듀서(상태 변경 함수), 액션(함수 내용)을 정의하는 데 사용된다.

```jsx
const authSlice = createSlice({
  name: 'auth',
  initialState: {
    isAuthenticated: false,
    user: null,
    nickname: '',
  },
  reducers: {
```
authSlice라는 슬라이스를 생성한다. slice의 이름은 auth로 작명하며 initialState(초기값)으로는 실제로 관리할 상태들(reducer)들의 초기값을 객체 형태로 여러 개 생성한다.


createSlice의 두 번째 매개 변수로 reducers(상태 변경 함수들)를 작성한다.

```js
    login(state, action) {
      const { token, user } = action.payload;
      localStorage.setItem('accessToken', token);
      localStorage.setItem('user', JSON.stringify(user));
      state.isAuthenticated = true;
      state.user = user;
      state.nickname = user.nickname;
    },
```

- login 리듀서(함수)를 만든다.
- action.payload에서 token과 user를 받아온다.
- login 리듀서 호출 시 로컬 스토리지에 accessToken과 user 정보를 저장한다. 이는 페이지를 새로고침 하더라도 로그인 상태가 풀리지 않게 하기 위함이다.

```js
    logout(state) {
      localStorage.removeItem('accessToken');
      localStorage.removeItem('user');
      state.isAuthenticated = false;
      state.user = null;
      state.nickname = '';
    },
```

- logout 리듀서를 만든다.
- 로컬 스토리지에서 accessToken과 user 정보를 제거한다.
- isAuthenticated 인증 상태를 false로 전환한다.