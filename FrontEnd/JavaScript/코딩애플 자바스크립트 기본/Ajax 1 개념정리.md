## 서버란?

데이터를 저장해놓는 컴퓨터.

## 서버에 데이터를 요청할 때

1. 어떤 데이터의 url인지.
2. GET 요청인지 POST 요청인지 method를 정확하게.

| CRUD  | method | 기능                   |
| :---: | :----: | :--------------------- |
| Creat |  POST  | 데이터를 서버로 보냄   |
| Read  |  GET   | 데이터를 서버에서 받음 |

이러한 내용은 API 서버에서 명세서를 작성해둠. 그걸 보고 필요한 요청을 고르면 됨.

## POST 요청 보내기

```HTML
<form action="url" method="post"> </form>
```

## GET / POST 등 method의 단점

브라우저가 새로고침 됨.

## Ajax 통신이란?

새로고침 없이 GET, POST를 요청하는 기능.

## Ajax 통신을 하는 방법

index.html을 크롬 등으로 바로 open하면 작동하지 않을 수 있음.  
Live Server 등으로 열어서 도메인이 아이피주소로 뜨는지 확인해야 함.