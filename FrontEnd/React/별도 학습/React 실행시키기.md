React는 웹 브라우저에서 실행시키기 위해서는 터미널을 이용해야 한다.

## Create-React-App 환경

`Create-React-App`을 이용한 경우, 터미널에 `npm start`를 작성 후 엔터.

## Vite 환경

`Vite`를 이용한 경우, `npm install` 이후 `npm run dev`를 작성 후 엔터.

터미널에 로컬호스트 아이피가 뜨면 그것을 `ctrl + 클릭`하면 웹 브라우저에서 열림.

만약 `vite`의 경우, 여기까지 했는 데도 실행이 안 되면 `package.json` 파일 확인.

```json
// 아래와 같이 작성이 잘 되어 있는지 확인하고 안 되어 있다면 수정해주고 다시 하면 됨.
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
}
```