`margin` 속성은 요소의 외부 여백을 설정하는 속성이다.

`margin` 다음에 오는 값의 개수에 따라 설정되는 방향이 달라진다.

1.  **한 개의 값 (`margin: value;`)**: 모든 방향(위, 오른쪽, 아래, 왼쪽)에 동일한 여백 적용.
2.  **두 개의 값 (`margin: value1 value2;`)**:
    -   첫 번째 값(`value1`): 위와 아래 여백
    -   두 번째 값(`value2`): 왼쪽과 오른쪽 여백
3.  **세 개의 값 (`margin: value1 value2 value3;`)**:
    -   첫 번째 값(`value1`): 위 여백
    -   두 번째 값(`value2`): 왼쪽과 오른쪽 여백
    -   세 번째 값(`value3`): 아래 여백
4.  **네 개의 값 (`margin: value1 value2 value3 value4;`)**:
    -   첫 번째 값(`value1`): 위 여백
    -   두 번째 값(`value2`): 오른쪽 여백
    -   세 번째 값(`value3`): 아래 여백
    -   네 번째 값(`value4`): 왼쪽 여백

### `margin: 30px 0 30px 0;`

해석:

-   위 여백: 30px
-   오른쪽 여백: 0
-   아래 여백: 30px
-   왼쪽 여백: 0

### 가운데 정렬 (수평)

요소를 수평으로 가운데 정렬하려면, `margin-left`와 `margin-right`를 `auto`로 설정해야 한다. 이렇게 하면 해당 요소는 가로 방향으로 남은 여백을 균등하게 나누어 가지게 된다.

예를 들어:

```
.element {
  margin: 0 auto;
}
```

이 구문은 다음과 같이 해석된다:

-   위 여백: 0
-   오른쪽 여백: auto
-   아래 여백: 0
-   왼쪽 여백: auto

### `0`과 `auto`의 의미

-   `0`: 여백이 없음을 의미
-   `auto`: 브라우저가 여백을 자동으로 계산하여 균등하게 배분. 주로 가운데 정렬에 사용.

### 전체 예제

다음은 `div` 요소를 수평으로 가운데 정렬하는 예제:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Centered Element</title>
  <style>
    .centered-element {
      width: 50%; /* 너비를 지정해야 가운데 정렬이 효과적임 */
      margin: 0 auto; /* 수평으로 가운데 정렬 */
      background-color: lightblue;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="centered-element">
    This element is centered horizontally.
  </div>
</body>
</html>
```

위의 예제에서는 `.centered-element` 클래스의 `div`가 수평으로 가운데 정렬됨. `margin: 0 auto;` 속성을 사용하여 이를 구현함.

이제, 주어진 요소의 `margin` 속성을 적절히 설정하여 원하는 레이아웃을 구성할 수 있다.