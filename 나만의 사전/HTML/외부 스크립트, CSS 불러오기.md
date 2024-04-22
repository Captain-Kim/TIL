## 외부 CSS 불러오기

```html
<head>
<link rel="stylesheet" href="style.css" />
</head>
```

style.css는 본인이 설정한 파일명과 경로.

## 외부 JavaScript 불러오기

```html
<script src="login.js"></script>
</body>
```

보통 닫는 </body>바로 위에 작성.  
HTML요소가 모두 로드되고 나서 불러와지는 것이 사용자가 쾌적하게 느껴지기 때문.  
단 필요에 따라 더 HTML보다 더 빨리 로드되어야 하는 것들은 여는 <body> 태그 밑에도 작성 가능함.