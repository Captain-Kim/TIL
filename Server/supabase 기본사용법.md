먼저 [supabase 공식문서](https://supabase.com/docs)를 많이 참고해야 한다.

## supabase란?


Supabase 등장 이전 프론트엔드 개발자들에겐 Firebase가 더 익숙한 백엔드 서비스일 것이다.


Supabase와 Firebase는 모두 백엔드 서비스 플랫폼으로, 애플리케이션 개발을 돕기 위해 데이터베이스, 인증, 저장소 등의 기능을 제공힌디. 두 플랫폼은 개발자에게 서버 관리 부담을 줄이고, 빠르고 효율적인 개발을 가능하게 한다. 하지만 이 두 플랫폼은 각각의 장단점을 가지고 있다.


Supabase는 오픈 소스 백엔드 서비스로, Postgres를 기반으로 한 데이터베이스를 제공합니다. 다음과 같은 주요 기능을 포함한다
- **PostgreSQL 데이터베이스**: 관계형 데이터베이스로, 고급 쿼리 기능과 데이터 무결성을 제공한다.
- **실시간 기능**: 데이터 변경 사항을 실시간으로 감지하고 반영할 수 있다.
- **인증 및 권한 관리**: 다양한 인증 방법을 지원하며, 세밀한 권한 관리를 할 수 있다.
- **스토리지**: 파일 스토리지 기능을 제공하여 이미지, 비디오 등의 파일을 저장할 수 있다.
- **자동 API 생성**: 데이터베이스 테이블에 기반한 RESTful API를 자동으로 생성한다.


Firebase를 많이 써왔을 테니 이와 비교해보겠다. Firebase는 구글이 제공하는 백엔드 서비스로, 다양한 클라우드 기능을 통합하여 제공한다. 주요 기능은 다음과 같다.
- **NoSQL 데이터베이스**: Cloud Firestore와 Realtime Database를 제공하여 비정형 데이터를 빠르게 처리한다.
- **인증**: 다양한 인증 제공자와의 통합을 쉽게 처리할 수 있다.
- **호스팅**: 정적 및 동적 콘텐츠를 위한 호스팅 서비스를 제공한다.
- **클라우드 함수**: 서버리스 함수로 백엔드 로직을 작성할 수 있다.
- **스토리지**: 대용량 파일을 저장할 수 있는 클라우드 스토리지를 제공한다.
- **애널리틱스**: Firebase 애널리틱스로 사용자 행동을 분석할 수 있다.

### Supabase vs Firebase 장단점 비교

#### Supabase의 장점
1. **오픈 소스**: 소스 코드가 공개되어 있어 커스터마이징이 가능하고, 커뮤니티의 기여를 받을 수 있다.
2. **PostgreSQL**: 고급 SQL 기능과 확장성, 안정성을 제공한다.
3. **실시간 데이터베이스**: 실시간 데이터 업데이트가 가능하여, 실시간 기능이 필요한 애플리케이션에 적합하다.

#### Supabase의 단점
1. **상대적으로 적은 기능**: Firebase에 비해 아직 제공하는 기능이 제한적일 수 있다.
2. **커뮤니티와 지원**: Firebase에 비해 상대적으로 작은 커뮤니티와 지원 체계를 가지고 있다.

#### Firebase의 장점
1. **풍부한 기능**: 다양한 백엔드 기능을 통합적으로 제공하여, 올인원 솔루션으로 활용할 수 있다.
2. **강력한 인프라**: 구글 클라우드 인프라를 기반으로 높은 안정성과 성능을 보장한다.
3. **광범위한 문서와 커뮤니티**: 풍부한 공식 문서와 커뮤니티 지원을 받을 수 있다.

#### Firebase의 단점
1. **비용**: 사용량이 증가할수록 비용이 빠르게 증가할 수 있다.
2. **락인(Lock-in) 효과**: 구글의 생태계에 종속되기 쉬워, 다른 플랫폼으로의 이전이 어려울 수 있다.
3. **제한된 SQL 기능**: NoSQL 데이터베이스를 사용하므로, 복잡한 쿼리나 데이터 무결성 관리가 어려울 수 있다.

### 결론
Supabase와 Firebase는 각각의 장단점을 가지고 있어, 프로젝트의 요구사항에 따라 선택이 달라질 수 있다. 실시간 기능과 SQL 데이터베이스가 중요한 프로젝트라면 Supabase가 유리할 수 있으며, 다양한 백엔드 기능과 구글 생태계와의 통합이 필요한 경우 Firebase가 적합할 수 있다.

## Supabase 기본 사용법

### supabase 클라이언트 라이브러리 설치

```
npm install @supabase/supabase-js

또는

yarn add @supabase/supabase-jf
```

### 클라이언트 셋업

supabase에 데이터를 CRUD 하기 위해서는 나의 supabase URL과 anon key가 필요하다.


이는 supabase에서 획득할 수 있다. 그리고 더 나아가 환경변수 처리까지 해주면 보안상 더 좋다. 그러나 노출되어도 크게 문제는 없는 듯하다.


#### 별도의 파일로 supabase 키를 분리하기

해도 되고 안 해도 되지만 여러 컴포넌트에서 사용하는 경우 따로 빼는 것이 재사용성이 좋다. 따로 아래처럼 작성해놓고 사용하고자 하는 파일에서 `import { supabase } from '../supabase.js`만 해주면 된다.

```jsx
// supabaseClient.js
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'https://your-supabase-url.supabase.co';
const supabaseKey = 'your-anon-key';
export const supabase = createClient(supabaseUrl, supabaseKey);
```


```jsx
// App.jsx
  import { useEffect, useState } from "react";
  import { createClient } from "@supabase/supabase-js";

  const supabase = createClient("https://<project>.supabase.co", "<your-anon-key>");

  function App() {
    const [countries, setCountries] = useState([]);

    useEffect(() => {
      getCountries();
    }, []);

    async function getCountries() {
      const { data } = await supabase.from("countries").select();
      setCountries(data);
    }

    return (
      <ul>
        {countries.map((country) => (
          <li key={country.name}>{country.name}</li>
        ))}
      </ul>
    );
  }

  export default App;
```

위처럼 진행해도 되고, 만약 supabase에 대한 fetch를 여러 컴포넌트에서 반복할 것이라면 supabase 키를 미리 따로 컴포넌트에 분리해놓고 재사용해도 좋다.

#### 셋업 코드 상세 설명

```jsx
// App.jsx
import { useEffect, useState } from "react";
import { createClient } from "@supabase/supabase-js";
```

`useEffect` 훅과 `useState` 훅을 사용한다. supabase에서 fetch 해 온 데이터를 state에 담아서 사용하기 위함이다.


이 코드의 예제는 supabase에서 국가 목록을 테이블에서 가져오는 예제이다. supabase의 데이터테이블은 엑셀과 같이 생긴 테이블이고 column과 row가 존재한다. 차이가 있다면 엑셀 시트에서는 다른 시트, 즉 다른 테이블과의 관계를 설정하여 데이터를 유기적으로 주고 받을 수 있는 관계형 데이터베이스(SQL)의 형태를 띄고 있다는 것이다.


엑셀과 비슷한 형태라는 것은 코드로 나타내면 이러한 배열의 형태를 띄고 있는 것이다.

```jsx
[
    {
        국가명: "대한민국",
        영어명: "Republic of Korean",
    }, 
    {
        국가명: "미국",
        영어명: "United State America",
    }, 
    {
        ...
    }, 
    {
        ...
    }
]
```

따라서 `coutires`라는 state는 빈 배열로 초기화 한 것이다.


```jsx
useEffect(() => {
    getCountries();
}, []);

async function getCountries() {
    const { data } = await supabase.from("countries").select();
    setCountries(data);
}
```

그리고 위 코드를 보면 `useEffect`훅을 사용하여 함수 `getCountries`를 실행하고 있다.


함수 `getCountries`는 임의로 작명한 함수이고, 함수의 내용을 보면 `async...await` 비동기 함수로 설정하였고, supabase에서 `countries`라는 테이블을 불러오는 함수이다.


data라는 변수로 불러와서 구조 분해 할당한 뒤 `setCountries`라는 상태 변경 함수를 통해 `countries` state에 담는다.


데이터를 불러오는 함수를 비동기적으로 처리했다라는 말의 의미를 생각해보아야 한다. `getCountries` 함수를 선언하는데 `async` 키워드를 사용하여 함수를 비동기로 선언했다.


`supabase.from("데이터테이블명").select()`는 서버에 API 요청을 보내는 fetch 함수이기 때문에 시간이 걸리는 작업이다. 만약 비동기함수로 선언하지 않았다면 코드가 동기적으로 실행됨에 따라 내 코드에서 이 fetch 함수가 종료될 때까지 다음 코드 라인이 실행되지 않기 때문에 시간이 얼마나 걸리는 지에 따라서 UI가 렌더링 되지 않을 수도 있다.


보통 리액트 프로젝트는 함수가 먼저 실행된 후 return JSX문에서 UI가 렌더링 되기 때문에 이를 기다리고 있을 수만은 없다. 따라서 fetch 함수가 종료되기를 기다리지 않고, 일단 fetch 요청을 보내고 다음 라인으로 넘어간다는 개념이다.


따라서 `await` 키워드를 사용하면 이 요청이 완료될 때까지 기다리지만, 그 시간이 걸리는 동안 나의 웹 사이트는 다른 작업을 계속 할 수 있다.


위 함수는 supabase 라이브러리를 설치하면 supabase에서 제공하는 내장함수이며, 자바스크리브 문법이 아니다. 만약 이를 리액트에서 사용하는 자바스크립트 문법으로 풀어서 쓰면 아래와 같은 형태가 된다.


```javascript
import React, { useEffect, useState } from 'react';

const fetchPosts = async () => {
  const response = await fetch('https://your-project-url.supabase.co/rest/v1/posts', {
    method: 'GET',
    headers: {
      'apikey': 'your-anon-key',
      'Authorization': 'Bearer your-anon-key',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.message);
  }

  const data = await response.json();
  return data;
};

const Posts = () => {
  const [posts, setPosts] = useState([]);
  const [error, setError] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    const getPosts = async () => {
      try {
        const data = await fetchPosts();
        setPosts(data);
      } catch (error) {
        setError(error.message);
      } finally {
        setIsLoading(false);
      }
    };

    getPosts();
  }, []);

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error: {error}</div>;
  }

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <h2>{post.title}</h2>
            <p>{post.content}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Posts;
```

### CRUD 수행하는 방법

#### C(데이터 삽입): `insert` 메서드

```jsx
const { data, error } = await supabase
  .from('posts')
  .insert([
    { title: 'Hello World', content: 'This is my first post!' }
  ]);

if (error) console.error('Error inserting data:', error);
else console.log('Data inserted:', data);
```

`posts`라는 데이터 테이블에서 `title`이라는 컬럼에 'Hello World'라는 텍스트를, `content`라는 컬럼에 'This is my first post!'라는 텍스트를 삽입하는 예제이다.


그런데 보통은 사용자에게 입력필드에서 데이터를 받아서 '작성' 버튼 등에 이벤트 핸들러를 달아 작성할 것이기 때문에 이런 경우 아래와 같이 응용하면 된다.

```jsx
// src/components/PostForm.js
import React, { useState } from 'react';
import { supabase } from '../supabaseClient';

const PostForm = () => {
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');

  const handleSubmit = async (event) => {
    event.preventDefault();
    const { data, error } = await supabase
      .from('posts')
      .insert([
        { title, content }
      ]);

    if (error) {
      console.error('Error inserting data:', error);
    } else {
      console.log('Data inserted:', data);
      alert('Post created successfully!');
      setTitle('');
      setContent('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="title">Title:</label>
        <input 
          type="text" 
          id="title" 
          value={title} 
          onChange={(e) => setTitle(e.target.value)} 
          required 
        />
      </div>
      <div>
        <label htmlFor="content">Content:</label>
        <textarea 
          id="content" 
          value={content} 
          onChange={(e) => setContent(e.target.value)} 
          required 
        />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default PostForm;
```

타이틀, 컨텐트 입력필드를 state로 관리하고, 이벤트 핸들로 상태 변경함수를 연결한다. 그리고 핸들링 함수에 fetch 함수를 정의하는데, insert에서 `title: title, content: content` 형태로 상태롤 연결지으면 되는데, 같은 키워드는 생략할 수 있기 때문에 `{title, content}`로 사용할 수 있다.

#### R(데이터 조회): `select` 메서드

```jsx
const { data, error } = await supabase
  .from('posts')
  .select('*');

if (error) console.error('Error fetching data:', error);
else console.log('Data fetched:', data);
```

`'*'`은 모든 테이블을 불러오라는 의미이다. 그런데 supabase는 기본적으로 무료 용량을 제공하지만 제공량을 넘을 수 있으므로 꼭 필요한 데이터만 조회하는 것이 최적화 측면에서도 중요하다.

#### U(데이터 업데이트): `update` 메서드

```jsx
const { data, error } = await supabase
  .from('posts')
  .update({ content: 'This is an updated post!' })
  .eq('id', 1); // ID가 1인 레코드 업데이트

if (error) console.error('Error updating data:', error);
else console.log('Data updated:', data);
```

#### D(데이터 삭제): `delete` 메서드

```jsx
const { data, error } = await supabase
  .from('posts')
  .delete()
  .eq('id', 1); // ID가 1인 레코드 삭제

if (error) console.error('Error deleting data:', error);
else console.log('Data deleted:', data);
```

### 로그인 / 로그아웃 수행하는 방법

#### 로그인

```jsx
const { user, error } = await supabase.auth.signIn({
  email: 'user@example.com',
  password: 'password'
});

if (error) console.error('Error signing in:', error);
else console.log('User signed in:', user);
```

### 로그아웃

```jsx
const { error } = await supabase.auth.signOut();

if (error) console.error('Error signing out:', error);
else console.log('User signed out');
```

### 스토리지에 파일 업로드/다운로드 하기

#### 파일 업로드

```jsx
const file = document.querySelector('input[type="file"]').files[0];
const { data, error } = await supabase.storage
  .from('uploads')
  .upload('public/my-file.txt', file);

if (error) console.error('Error uploading file:', error);
else console.log('File uploaded:', data);
```

`.from('uploads')` 메서드는 uploads라는 스토리지의 특정 버킷을 지목하는 것임.


`.upload('public/my-file.txt')` 는 위 버킷에서 public이라는 폴더에 있는 my-file.txt 파일을 다운로드하라는 의미임. 


이 코드를 사용자에게 파일을 입력필드로 받아서 등록하는 버튼에 이벤트 핸들러에 연결하려면 아래와 같이 활용할 수 있음.

```jsx
import React, { useState } from 'react';
import supabase from './supabaseClient'; // supabaseClient 파일 경로를 맞게 설정하세요.

const FileUpload = () => {
  const [file, setFile] = useState(null);
  const [uploadError, setUploadError] = useState(null);
  const [uploadSuccess, setUploadSuccess] = useState(null);

  // 파일이 선택되었을 때 호출되는 함수
  const handleFileChange = (event) => {
    setFile(event.target.files[0]);
  };

  // 파일 업로드 함수
  const handleFileUpload = async () => {
    if (!file) {
      setUploadError('파일을 선택해주세요.');
      return;
    }

    // 고유한 파일 이름 생성
    const uniqueFileName = `${Date.now()}-${file.name}`;

    const { data, error } = await supabase.storage
      .from('uploads')
      .upload(`public/${uniqueFileName}`, file);

    if (error) {
      setUploadError(`Error uploading file: ${error.message}`);
      setUploadSuccess(null);
    } else {
      setUploadError(null);
      setUploadSuccess('File uploaded successfully.');
      console.log('File uploaded:', data);
    }
  };

  return (
    <div>
      <input type="file" onChange={handleFileChange} />
      <button onClick={handleFileUpload}>업로드 하기</button>
      {uploadError && <p style={{ color: 'red' }}>{uploadError}</p>}
      {uploadSuccess && <p style={{ color: 'green' }}>{uploadSuccess}</p>}
    </div>
  );
};

export default FileUpload;
```

#### 파일 다운로드

```jsx
const { data, error } = await supabase.storage
  .from('uploads')
  .download('public/my-file.txt');
```

```jsx
if (error) console.error('Error downloading file:', error);
else {
  const url = URL.createObjectURL(data);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'my-file.txt';
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
}
```