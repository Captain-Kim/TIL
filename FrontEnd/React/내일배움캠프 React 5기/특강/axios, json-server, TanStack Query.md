## axios

### axios의 장점

#### 1. 요청/응답 시 json 데이터 형식을 기본으로 함 (vs fetch API)

axios는 요청을 보낼 때 자동으로 JSON 형식을 사용함. fetch API에서는 이 작업을 하려면 수동으로 해주어야 함.


예를 들어 아래는 같은 작업임.

```jsx
// axios
axios.post('/api/data', { key: 'value' }).then(response => { console.log(response.data);})

// fetch API
fetch('/api/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ key: 'value' })
})
.then(response => response.json())
.then(data => {
  console.log(data);
});
```

위는 POST 요청이고, 아래는 GET 요청의 차이임.

```jsx
// axios
axios.get('/api/data').then(response => { console.log(response.data); });

// fetch API
fetch('/api/data').then(response => response.json()).then(data => {console.log(data);});
```

GET 요청을 해서 데이터를 받아올 때도 axios는 따로 JSON으로 파싱하지 않아도 이미 JSON으로 받아오고 있고, fetch API는 JSON으로 파싱하는 작업이 한 번 더 들어가야 함.

#### 2. Custom Instance와 Interceptor를 이용한 코드 재사용

axios의 알파이자 오메가이다. 특히 baseURL의 재사용성이 뛰어나다. Next.js와 사용할 때는 사용하지 않지만 리액트에서는 axios를 사용할 때 인스턴스는 코드 작성을 편리하게 해준다.

##### 2-1. Custom Instance

**1. baseURL 저장**

```jsx
// API 요청 코드가 수없이 많이 있을 때
axios.get('http://localhost:3000/todos');
axios.get('http://localhost:3000/todos');
axios.get('http://localhost:3000/todos');
axios.get('http://localhost:3000/todos');

// 위 코드에서 baseURL이 변경된다면, 포트가 3000번에서 3001로 변경된다면
// 이 URL이 사용된 모든 코드를 찾아 다니며 바꿔줘야 한다.
// 이런 경우 아래처럼 baseURL을 axiosInstance에 할당하여 재사용 할 수 있다.
// 패턴이니 외우거나 복붙하면 된다. (변수명은 자유작명이나 예시는 컨벤션임.)

const axiosInstance = axios.create({
    baseURL: 'http://localhost:3000',
});

axiosInstance.get('/todos'); // 매개 변수에는 엔드포인트만 작성
```

**2. API 서버 분리**

그리고 프로젝트에서 API 서버가 꼭 한 개인 것은 아니다. 서버가 여러 개인 경우, API 서버를 별도로 분리할 때도 용이하다.

```jsx
// axios instance 미사용 시
// 1. 인증서버
const { data } = await axios.post('URL', {
    'id': '유저 아이디',
    'password' : '유저 비밀번호',
    'nickname' : '유저 닉네임',
});
// 2. json 서버
const { data } = await axios.get('URL');

// axios instance 사용 시
const authApi = axios.create({
    baseURL: 'URL'
});

const jsonApi = axios.create({
    baseURL: 'URL'
})

const { data } = authApi.post('/register', {
    'id': '유저 아이디',
    'password': '유저 비밀번호',
    'nickname': '유저 닉네임'
});
const { data } = jsonApi.get('/todos');
```
##### 2-2. Interceptor

인터셉터는 농구의 기술의 이름처럼, 가로챈다는 뜻이다. 즉 HTTP 요청을 보낼 때 요청 헤더에 자동으로 JWT 토큰을 추가하는 기능을 구현한 것이다.


이렇게 되면 매번 수동으로 토큰을 추가할 필요 없이 모든 요청에 토큰이 포함되도록 할 수 있다.

**1. request header 코드 재사용**

JWT 인증 서버의 경우 요청을 할 때마다 헤더에 accessToken을 넣어줘야 하는 번거로움이 있다.

```jsx
// authAPI로 줄여 놓은 axios 인스턴스에 interceptors 기능을 사용하여
// request(요청)을 가로챈다.
authApi.interceptors.request.use(
    // 요청을 가로챘을 때 실행할 함수를 정의한다.
  (config) => {
    // accessToken이라는 변수에 로컬 스토리지에서 값을 꺼내와 할당한다.
    const accessToken = localStorage.getItem("accessToken");
    
    // 그리고 accessToken이 있으면 (로컬 스토리지에서 값을 잘 꺼내 왔다면)
    // config.headers 객체에 Authorization 속성을 추가하고
    // Bearer 토큰... 을 헤더에 넣는다.
    if (accessToken) {
      config.headers["Authorization"] = `Bearer ${accessToken}`;
    }
    return config;
  },
  // 서버에서 응답 에러 시 Promise 객체를 반환하여 에러를 처리한다.
  (err) => Promise.reject(err)
);
```

json서버 요청의 경우, 로그인 상태일 경우에만 CRUD가 가능해야 하므로,

accessToken을 인증서버에 보내서 인가받은 후에 요청을 보내도록 해야 한다.

```jsx
// jsonApi라는 API 서버로 인터셉터를 통해 요청을 가로챈다.
jsonApi.interceptors.request.use(
  async (config) => {
    // authApi 서버로 get 요청을 보내는데 /user PATH에 보낸다.
    // 응답이 성공하면 내용을 data에 담는다.
    const { data } = await authApi.get("/user");
    // 응답 데이터에서 success라는 속성이 있으면, (GET 요청에 성공하면)
    // 요청을 한 객체 config를 그대로 반환한다.
    if (data.success) return config;
    // 만약 실패하면 Error 객체를 생성하여 Promise를 거부한다.
    return Promise.reject(new Error('사용자 정보 조회에 실패 했습니다.'));
  },
  // 서버에서 응답에 실패하면 에러 처리를 한다.
  (err) => Promise.reject(err)
);
```

토큰 만료시 대처 방법

```jsx
authApi.interceptors.response.use(
  (response) => response,
  (err) => {
    alert(err.response.data.message);
    if (
      err.response.data.message ===
      "토큰이 만료되었습니다. 다시 로그인 해주세요."
    ) {
      // 로그아웃처리
      // 스토어에서 logout 함수를 꺼내 온다. 필요에 따라서 스토어가 아닐 수 있다.
      return store.dispatch(logout());
    }
    return Promise.reject(err);
  }
);
```

**2. response 에러 핸들링 용도로 사용**

#### 3. 크로스 브라우징에 유리

### 자주 사용되는 CRUD 패턴

```jsx
// GET (최신순 정렬)
const { data } = await axios.get(`baseURL/todos?_sort=createdAt&_order=desc"`);

// POST 추가
const { data } = await axios.post(`baseURL/todos`, newTodo);

// PATCH 수정
const { data } = await axios.patch(`baseURL/todos/${id}`, { title: "수정된제목" });

// DELETE 삭제
const { data } = await axios.delete(`baseURL/todos/${id}`);
```

#### 1. GET 요청

- 목적 : todos 엔드포인트에서 데이터를 조회함.
- 조회된 데이터를 생성된 기간(createdAt) 기준으로 내림차순(desc)으로 정렬함.
- `axios.get(URL)` : 이 URL로 GET 요청을 보냄.
- `baseURL/todos` : JSON 서버의 기본 URL과 todos 엔드포인트를 합친 것.
- `?_sort=createdAT&_order=desc` : 쿼리 문자열로, createdAt 필드를 기준으로 내림차순으로 정렬함.
- `await` : 비동기 요청이 완료될 때까지 기다림.
- `{ data }` : 응답 받은 객체에서 data 필드를 구조 분해 할당하여 추출한다.

#### 2. POST 요청

- 목적 : todos에 새로운 할 일인 newTodo를 추가함.
- 기능 : 서버에 새로운 newTodo를 전송하여 저장한다.
- `axios.post(URL, data)` : API URL로 POST 요청을 보낸다. 두번째 매개 변수에는 실어 보낼 데이터를 포함시킨다. 위 예제에서는 newTodo라는 상태에 담았다. 객체 형태로 구성되어 있다.
- `{ data }` : 추가가 완료된 데이터를 data로 추출한다.

#### 3. PATCH 요청

- 목적 : todos에서 일부 필드를 수정함.
- 기능 : `${id}`에 해당하는 todo 항목에서 title이라는 필드의 값을 "수정된제목"으로 바꾼다.
- `axios.patch(URL, data)` : URL로 PATCH 요청을 보낸다. 두 번째 매개 변수에는 수정할 데이터를 실어 보낸다.
- `baseURL/todos/${id}` : JSON 서버의 기본 URL과 todos의 엔드포인트, 그리고 수정할 항목의 ID까지 URL로 구성했다.
- `{ title: "수정된제목" }` : PATCH 요청에 실어 보내는 수정할 정보다.
- `{ data }` : 수정이 완료된 데이터를 data로 추출한다.

#### 4. DELETE 요청

- 목적 : todo 항목을 삭제함.
- 기능 : id에 해당하는 todo 항목을 서버에서 삭제함.
- `axios.delete(URL)` : 지정된 URL에 DELETE 요청을 보냄.
- `baseURL/todos/${id}` : JSON 서버의 기본 URL과 todos 엔드포인트, 그리고 삭제할 항목의 ID를 합친 것.
- `{ data }` : 삭제가 완료된 데이터를 data로 응답받아 추출한다. 데이터가 삭제되어 아무 것도 없으면 빈 객체일 수 있다. 삭제 완료된 데이터를 반환하는 것이 아니라 단순히 성공 메시지를 반환할 수도 있다.

## TanStack 

### TanStack Query란?

TanStack Query란, 웹 애플리케이션과 서버 간 데이터를 쉽게 주고 받을 수 있게 도와주는 라이브러리이다.


웹 애플리케이션은 서버에 데이터를 입력해서 보내고, 서버는 데이터를 사용자에게 보내며 서버와 클라이언트는 상호 작용한다.


이러한 상호작용이 발생할 때 데이터를 효율적으로 관리하는 것은 중요하다.

### TanStack Query의 장점

1. 서버에서 데이터를 쉽게 가져올 수 있음.
2. 한 번 가져온 데이터를 저장해 두었다가 다시 필요할 때 빠르게 불러올 수 있음. 이를 '데이터 캐싱'이라 함.
3. 데이터가 바뀌면 화면에 자동으로 업데이트 됨.
4. 데이터를 가져오거나 불러올 때 에러가 발생한다면 이를 쉽게 처리할 수 있음.

### TanStack Query 주요 개념

- Query : 서버에서 데이터를 가져오는 작업.
- Mutation : 서버에 데이터를 보내는 작업.
- Query Client : Query와 Mutation을 관리하는 역할.

queryClient는 App 컴포넌트에서 선언하지 않도록 한다. main.jsx가 권장된다. 이유는 App 컴포넌트는 함수 컴포넌트이기 때문에 리렌더링의 대상이 된다. 따라서 queryClient라는 store가 불필요하게 계속 생성될 수 있다.

### TanStack Query

#### 라이브러리 설치

```
npm install @tanstack/react-query
또는
yarn add @tanstack/react-query
```

#### Query Client 설정

프로젝트 루트 컴포넌트에서 Query Client를 설정하고, 페이지 라우팅하는 Main.jsx에서 `QueryClientProvider`로 감싸준다.


App 컴포넌트에서 감싸주는 작업을 할 경우, App 컴포넌트 또한 함수형 컴포넌트이다 보니 리렌더링의 대상이 될 수 있고, QueryClient는 일종의 상태 관리용 store인데 컴포넌트가 리렌더링 될 때 불필요하게 store가 계속 생성될 수 있기에 Main.jsx에 작업하는 것이 권장된다.

```jsx
// Main.jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

import App from './App';

// Query Client 생성
const queryClient = new QueryClient();

ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <ReactQueryDevtools initialIsOpen={false} />
    <App />
  </QueryClientProvider>,
  document.getElementById('root')
);
```

#### 데이터 가져오기 (Query)

```jsx
// 데이터를 가져오려는 컴포넌트
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

function UserList() {
  // Query 사용: 서버에서 사용자 리스트 가져오기
  const { data, error, isLoading } = useQuery('users', async () => {
    const response = await axios.get('/api/users');
    return response.data;
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

##### TIP: axios instance 또는 interceptor로 코드 줄이기

위 코드를 보면 axios로 GET 요청을 보내는 비동기 함수가 있는데, 이것을 위에서 배운 axios instance를 응용하여 활용하면 아래와 같다.


보통 axios 설정이 복잡해질 것 같으면 별도의 파일을 만들어서 관리한다.

```js
// api.js
import axios from 'axios';

// axios 인스턴스 생성
const api = axios.create({
    baseURL: 'http://localhost:3000',
    timeout: 5000, // 요청 타임아웃
    headers: {'Content-Type': 'application/json'},
});

export default api;
```

timeout은 밀리초 단위로, 5000이면 5초를 의미하는데, 이 시간 동안 서버에서 응답이 없으면 요청을 취소시킨다. 서버가 상태가 안 좋을 때 사용자가 마냥 기다리는 일이 없도록 도와준다.


headers는 하나의 패턴으로, 'Content-Type' 우리가 보내는 정보가, 'application/json' json 형식임을 서버에 알리는 내용이다.


주요하게 사용되는 패턴은 아래와 같다.


```jsx
// 1. JSON 형식의 데이터를 전송
headers: {'Content-Type': 'application/json'}

// 2. 폼 데이터를 URL 인코딩 형식으로 전송. HTML 폼에서 데이터를 전송할 때 주로 사용.
headers: {'Content-Type': 'application/x-www-form-urlencoded'}

// 3. 주로 이미지 등의 파일을 보낼 때 사용.
headers: {'Content-Type': 'multipart/form-data'}

// 4. 순수 텍스트만 보낼 때 사용
headers: {'Content-Type': 'text/plain'}

// 5. XML 형식의 데이터를 전송할 때 사용
headers: {'Content-Type': 'text/html'}
```

실제 사용 예시는 아래와 같다.

```jsx
// 1. JSON 데이터 전송
const api = axios.create({
  baseURL: 'http://localhost:3000',
  headers: {'Content-Type': 'application/json'},
});

api.post('/data', { key: 'value' });

// 2. 폼 데이터 전송
const api = axios.create({
  baseURL: 'http://localhost:3000',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
});

api.post('/data', new URLSearchParams({ key: 'value' }));

// 3. 파일 업로드
const api = axios.create({
  baseURL: 'http://localhost:3000',
  headers: {'Content-Type': 'multipart/form-data'},
});

const formData = new FormData();
formData.append('file', fileInput.files[0]);

api.post('/upload', formData);


// 4. 순수 텍스트
const api = axios.create({
  baseURL: 'http://localhost:3000',
  headers: {'Content-Type': 'text/plain'},
});

api.post('/text', 'This is plain text');
```

여기서 문제가 하나 있는데, axios 인스턴스에서 헤더까지 설정해버리면 컴포넌트 별로 다른 요청, 다른 데이터 형태를 보내고 싶을 수도 있는데 이것에 대응하지 못한다는 것이다.


이런 경우 1. axios 인스턴스에 baseURL과 지연시간만 정의하고 POST 요청을 보내는 곳에서 Header를 그때마다 정의하거나, 2. 전송할 데이터 별로 인스턴스를 미리 작성해놓는 방법, 3. 인터셉터를 통해 헤더를 동적으로 설정하는 방법이 있다.


이 방식들 중 3번 interceptors를 이용하는 방법이 가장 유연하다.

```jsx
// 1. axios instance를 기본만 설정하고 요청 보내는 곳에서 Header를 정의
import axios from 'axios';

// Axios 인스턴스 생성
const api = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 5000, // 5초
});

// JSON 데이터를 보내는 요청
function sendJsonData(data) {
  return api.post('/json-endpoint', data, {
    headers: {
      'Content-Type': 'application/json'
    }
  });
}

// 파일을 업로드하는 요청
function uploadFile(file) {
  const formData = new FormData();
  formData.append('file', file);

  return api.post('/upload-endpoint', formData, {
    headers: {
      'Content-Type': 'multipart/form-data'
    }
  });
}

// 2. 파일 형태 별로 axios 인스턴스를 따로 정의
import axios from 'axios';

// 기본 Axios 인스턴스 생성
const api = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 5000, // 5초
});

// JSON 데이터를 보내는 인스턴스 생성
const jsonApi = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 5000, // 5초
  headers: {
    'Content-Type': 'application/json'
  }
});

// 파일 업로드를 위한 인스턴스 생성
const fileUploadApi = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 5000, // 5초
  headers: {
    'Content-Type': 'multipart/form-data'
  }
});

function sendJsonData(data) {
  return jsonApi.post('/json-endpoint', data);
}

function uploadFile(file) {
  const formData = new FormData();
  formData.append('file', file);

  return fileUploadApi.post('/upload-endpoint', formData);
}

// 3. interceptors를 통한 동적인 Header 정의
import axios from 'axios';

// Axios 인스턴스 생성
const api = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 5000, // 5초
});

// 요청 인터셉터 설정
api.interceptors.request.use((config) => {
  if (config.url === '/upload-endpoint') {
    config.headers['Content-Type'] = 'multipart/form-data';
  } else {
    config.headers['Content-Type'] = 'application/json';
  }
  return config;
}, (error) => {
  return Promise.reject(error);
});

function sendJsonData(data) {
  return api.post('/json-endpoint', data);
}

function uploadFile(file) {
  const formData = new FormData();
  formData.append('file', file);

  return api.post('/upload-endpoint', formData);
}
```

interceptors를 사용하여 동적으로 헤더를 설정하는 방법을 상세히 다뤄보겠다.

```jsx
// api.js
import axios from 'axios';

// axios 인스턴스는 기본만 생성
const api = axios.create({
    baseURL: 'http://localhost:3000',
    timeOUT: 5000,
});

// 엔드포인트와 Content-Type 매핑 객체 생성
// (엔드포인트에 따라 파일 형태가 계속 달라지는 경우)
const contentTypeMapping = {
  '/upload-endpoint': 'multipart/form-data',
  '/json-endpoint': 'application/json',
  '/xml-endpoint': 'application/xml',
};

// 요청 인터셉터 설정
api.interceptors.request.use((config) => {
  // 요청 URL에 따라 Content-Type 헤더를 동적으로 설정
  const contentType = contentTypeMapping[config.url];
  if (contentType) {
    config.headers['Content-Type'] = contentType;
  }
  return config;
}, (error) => {
  return Promise.reject(error);
});

// 데이터 요청 함수 작성할 때
function sendJsonData(data) {
    return api.post('/json-endpoint', data);
}

function uploadFile(file) {
    const formData = new FormData();
    formData.append('file', file);

    return api.post('/upload-endpoint', formData);
}
```

이것을 TanStack Query를 사용하여 Query(데이터 가져오기)를 하는 곳에서 사용한다.

```jsx
import { useQuery } from '@tanstack/react-query';
import api from './api'; // axios 인스턴스를 가져옴

function UserList() {
  // Query 사용: 서버에서 사용자 리스트 가져오기
  const { data, error, isLoading } = useQuery('users', async () => {
    const response = await api.get('/users');
    return response.data;
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default UserList;
```

#### 데이터 보내기 (Mutation)

`useMutation` 훅을 사용하여 서버에 데이터를 전송할 수 있다. 그러면 성공이나 실패 상태를 관리해준다.


아래 예제도 api.js를 호출하여 axios instance로 축약 가능하다.

```jsx
// 데이터를 보내려는 컴포넌트
import { useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

function AddUser() {
  const queryClient = useQueryClient();

  // Mutation 사용: 새로운 사용자 추가하기
  const mutation = useMutation(newUser => axios.post('/api/users', newUser), {
    onSuccess: () => {
      // 사용자 리스트 새로고침
      queryClient.invalidateQueries('users');
    }
  });

  const handleSubmit = () => {
    const newUser = { name: 'New User' };
    mutation.mutate(newUser);
  };

  return <button onClick={handleSubmit}>Add User</button>;
}
```

### TanStack Query DevTools DevTools

TanStack Query DevTools는 리액트 쿼리의 상태를 시각화하고 디버깅하는데 도움을 준다.


앱의 쿼리 상태, 캐시된 데이터, 쿼리의 생명주기 등을 실시간으로 확인할 수 있다.


참고 : 데브툴스의 상태 = 서버 상태, 쿼리 데이터, 캐시라고 불린다.

#### 셋업 하기

패키지 설치하기

```
npm install @tanstack/react-query-devtools
또는
yarn add @tanstack/react-query-devtools
```

QueryClientProvider 내에 ReactQueryDevtools 추가

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import App from './App';

// Query Client 생성
const queryClient = new QueryClient();

ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <ReactQueryDevtools initialIsOpen={false} />
    <App />
  </QueryClientProvider>,
  document.getElementById('root')
);
```


## JSON-Server

JSON Server란 백엔드 구축을 기다리지 않고도 개발자 환경에서 서버를 빠르게 구축해볼 수 있도록 도와주는 도구이다. 빠르게 RESTful한 API를 생성할 수 있다는 장점이 있다.


하지만 이는 배포용 서버가 아니라 개발자 테스트 목적의 가짜, 즉 임시 서버이다.

### 셋업하기

#### 패키지 설치하기

```
npm install json-server -D
또는
yarn install json-server -D
```

json server 설치 시 -D를 꼭 붙이자. `yarn add json-server -D`와 같이 입력하면 dependencies에 설치되지 않고, devDependencies에 설치된다. devDependencies에 설치된 패키지는 개발할 때만 사용하는 패키지라 vercel 등에 배포할 때는 업로드 되지 않아 충돌 등을 일으키지 않는다.

#### package.json에서 scripts에 명령어 추가

```jsx
"json" : "json-server --watch db.json --port 5055"
```

위의 명령어를 입력하면 터미널에서 json server를 열 때마다 저 위 명령어를 치지 않아도 되고, `yarn json`이라고만 쳐도 json 서버를 열 수 있다.


json server는 계속 켜두어야 개발 서버도 돌릴 수 있다.


개발 서버와 json server를 두 번 켜야 하는 것이다.


그리고 json server는 Glitch라는 서비스를 이용해서 배포하는 것이 더 좋다.

#### fake json 파일 생성 (db.json)

```jsx
{
  "posts": [
    { "id": 1, "title": "Hello World" },
    { "id": 2, "title": "JSON Server" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 },
    { "id": 2, "body": "another comment", "postId": 1 }
  ]
}
```

### axios로 json-server에 API 요청 보내기

1. api.js 파일 생성

```jsx
// api.js
import axios from 'axios';

// axios 인스턴스는 기본만 생성
const api = axios.create({
    baseURL: 'http://localhost:3000', // json-server에서 설정한 대로
    timeOUT: 5000,
});

// 엔드포인트와 Content-Type 매핑 객체 생성
// (엔드포인트에 따라 파일 형태가 계속 달라지는 경우)
const contentTypeMapping = {
  '/upload-endpoint': 'multipart/form-data',
  '/json-endpoint': 'application/json',
  '/xml-endpoint': 'application/xml',
};

// 요청 인터셉터 설정
api.interceptors.request.use((config) => {
  // 요청 URL에 따라 Content-Type 헤더를 동적으로 설정
  const contentType = contentTypeMapping[config.url];
  if (contentType) {
    config.headers['Content-Type'] = contentType;
  }
  return config;
}, (error) => {
  return Promise.reject(error);
});

// 데이터 요청 함수 작성할 때
function sendJsonData(data) {
    return api.post('/json-endpoint', data);
}

function uploadFile(file) {
    const formData = new FormData();
    formData.append('file', file);

    return api.post('/upload-endpoint', formData);
}
```

2. json-server에 CRUD API 요청 보내기

```jsx
import api from './api';

// 모든 포스트 가져오기 (GET 요청)
function getAllPosts() {
  api.get('/posts')
    .then(response => console.log('All posts:', response.data))
    .catch(error => console.error('Error fetching posts:', error));
}

// 새로운 포스트 생성 (POST 요청)
function createPost(newPost) {
  api.post('/posts', newPost)
    .then(response => console.log('Post created:', response.data))
    .catch(error => console.error('Error creating post:', error));
}

// 특정 포스트 수정 (PUT 요청)
function updatePost(postId, updatedPost) {
  api.put(`/posts/${postId}`, updatedPost)
    .then(response => console.log('Post updated:', response.data))
    .catch(error => console.error('Error updating post:', error));
}

// 특정 포스트 삭제 (DELETE 요청)
function deletePost(postId) {
  api.delete(`/posts/${postId}`)
    .then(response => console.log('Post deleted:', response.data))
    .catch(error => console.error('Error deleting post:', error));
}

// 사용 예시
getAllPosts();

createPost({ title: 'New Post' });

updatePost(1, { title: 'Updated Post' });

deletePost(2);
```

3. 이것을 TanStack Query를 사용하여 CRUD를 구현해보자면 아래와 같다.

```jsx
import React from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import api from './api';

// 모든 포스트 가져오기 (GET 요청)
const fetchPosts = async () => {
  const { data } = await api.get('/posts');
  return data;
};

// 새로운 포스트 생성 (POST 요청)
const createPost = async (newPost) => {
  const { data } = await api.post('/posts', newPost);
  return data;
};

// 특정 포스트 수정 (PUT 요청)
const updatePost = async ({ postId, updatedPost }) => {
  const { data } = await api.put(`/posts/${postId}`, updatedPost);
  return data;
};

// 특정 포스트 삭제 (DELETE 요청)
const deletePost = async (postId) => {
  const { data } = await api.delete(`/posts/${postId}`);
  return data;
};

function App() {
  const queryClient = useQueryClient();

  // 모든 포스트 가져오기
  const { data: posts, error, isLoading } = useQuery(['posts'], fetchPosts);

  // 포스트 생성 Mutation
  const createPostMutation = useMutation(createPost, {
    onSuccess: () => {
      queryClient.invalidateQueries(['posts']); // 포스트 목록 새로고침
    },
  });

  // 포스트 수정 Mutation
  const updatePostMutation = useMutation(updatePost, {
    onSuccess: () => {
      queryClient.invalidateQueries(['posts']); // 포스트 목록 새로고침
    },
  });

  // 포스트 삭제 Mutation
  const deletePostMutation = useMutation(deletePost, {
    onSuccess: () => {
      queryClient.invalidateQueries(['posts']); // 포스트 목록 새로고침
    },
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {posts.map(post => (
          <li key={post.id}>
            {post.title}
            <button onClick={() => deletePostMutation.mutate(post.id)}>삭제하기e</button>
            <button onClick={() => updatePostMutation.mutate({ postId: post.id, updatedPost: { title: '수정된 타이틀' } })}>수정하기</button>
          </li>
        ))}
      </ul>
      <button onClick={() => createPostMutation.mutate({ title: '새로운 글' })}>등록하기</button>
    </div>
  );
}

export default App;
```