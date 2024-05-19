## 상황

웹 폰트를 지원하지 않는 폰트를 별도로 설치하여 사용하고 싶다. 예를 들어 `AppleSDGothicNeo` 즉 애플고딕은 웹 폰트를 지원하지 않으나, 상업적 비상업적 무료로 라이센스를 지원하기 때문에 폰트를 별도로 사용 가능하다.

## 방법

1. 원하는 폰트를 설치한다.

2. 프로젝트 폴더에 `fonts`라는 폴더를 만든 후 그곳에 폰트를 넣는다. (자유작명)

3. 설치한 폰트를 CSS에서 import한다.

```CSS
@font-face {
  font-family: 'AppleSDGothicNeoEB';
  src: url('../fonts/AppleSDGothicNeoEB.ttf') format('truetype'),;
  font-weight: normal;
  font-style: normal;
}
```

아래처럼 여러 개 할 수도 있음.


`format`은 폰트 확장자마다 입력 방법이 다르니, 아래 예시를 참고하거나 별도로 검색할 것.


CSS에서는 상대경로를 입력할 때 `.` 마침표 하나가 아니라 `..` 마침표 두 개로 들어간다는 점에 유의할 것.

```css
/* styles.css */
@font-face {
    font-family: 'MyCustomFont';
    src: url('../fonts/MyCustomFont.woff2') format('woff2'),
         url('../fonts/MyCustomFont.woff') format('woff'),
         url('../fonts/MyCustomFont.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
}

body {
    font-family: 'MyCustomFont', Arial, sans-serif;
}
```

4. 그리고 위처럼 `body` 태그 전체에 적용시키거나, 필요한 부분에서 `font-family`로 가져오면 됨.

그러나 주의할 점은 브라우저마다의 호환성을 생각해서 2~3순위의 폰트로 `Arial`, `sans-serif` 등을 같이 지정해주는 것이 좋음.