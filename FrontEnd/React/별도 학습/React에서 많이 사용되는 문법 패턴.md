리액트 프로젝트에서 자주 사용되는 주요 문법 패턴을 정리해보겠습니다. 이 패턴들은 대부분의 리액트 프로젝트에서 거의 항상 등장하며, 리액트 개발자라면 익숙해져야 할 요소들입니다.

### 1. 함수형 컴포넌트 (Functional Components)

함수형 컴포넌트는 간단하게 함수로 정의된 컴포넌트입니다. 상태와 훅을 사용할 수 있습니다.

```javascript
import React from 'react';

const MyComponent = () => {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
};

export default MyComponent;
```

### 2. 상태 관리 (State Management) with `useState`

`useState` 훅을 사용하여 컴포넌트의 상태를 관리합니다.

```javascript
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default Counter;
```

### 3. 사이드 이펙트 관리 (Side Effects) with `useEffect`

`useEffect` 훅을 사용하여 컴포넌트의 생명주기 동안 사이드 이펙트를 관리합니다.

```javascript
import React, { useEffect, useState } from 'react';

const DataFetcher = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // 빈 배열은 컴포넌트가 처음 렌더링될 때만 실행됨을 의미

  return (
    <div>
      {data ? <pre>{JSON.stringify(data, null, 2)}</pre> : 'Loading...'}
    </div>
  );
};

export default DataFetcher;
```

### 4. 컴포넌트 프롭스 (Component Props)

프롭스를 사용하여 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달합니다.

```javascript
import React from 'react';

const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

const App = () => {
  return <Greeting name="Alice" />;
};

export default App;
```

### 5. 컨디셔널 렌더링 (Conditional Rendering)

조건에 따라 다른 요소를 렌더링합니다.

```javascript
import React, { useState } from 'react';

const LoginButton = ({ isLoggedIn, handleLogin }) => {
  return (
    <button onClick={handleLogin}>
      {isLoggedIn ? 'Logout' : 'Login'}
    </button>
  );
};

const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const handleLogin = () => {
    setIsLoggedIn(!isLoggedIn);
  };

  return (
    <div>
      <LoginButton isLoggedIn={isLoggedIn} handleLogin={handleLogin} />
    </div>
  );
};

export default App;
```

### 6. 리스트 렌더링 (List Rendering)

리스트를 순회하여 여러 요소를 렌더링합니다.

```javascript
import React from 'react';

const List = ({ items }) => {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
};

const App = () => {
  const fruits = ['Apple', 'Banana', 'Cherry'];

  return <List items={fruits} />;
};

export default App;
```

### 7. 컨텍스트 API (Context API)

`Context`를 사용하여 컴포넌트 트리 전체에 걸쳐 데이터를 전달합니다.

```javascript
import React, { createContext, useContext } from 'react';

const MyContext = createContext();

const Child = () => {
  const value = useContext(MyContext);
  return <div>{value}</div>;
};

const Parent = () => {
  return (
    <MyContext.Provider value="Hello from context!">
      <Child />
    </MyContext.Provider>
  );
};

export default Parent;
```

### 8. 커스텀 훅 (Custom Hooks)

중복되는 로직을 재사용 가능한 훅으로 추출합니다.

```javascript
import { useState, useEffect } from 'react';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
};

export default useFetch;
```

### 9. 리액트 라우터 (React Router)

`React Router`를 사용하여 클라이언트 사이드 라우팅을 구현합니다.

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';

const Home = () => <h1>Home</h1>;
const About = () => <h1>About</h1>;

const App = () => {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
};

export default App;
```

이러한 패턴들은 리액트 프로젝트에서 자주 사용되는 기본적인 패턴들입니다. 이들에 익숙해지면 리액트 애플리케이션을 더 효과적으로 개발할 수 있습니다.