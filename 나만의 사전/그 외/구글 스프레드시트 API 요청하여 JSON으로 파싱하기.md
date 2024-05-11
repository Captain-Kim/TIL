## 상황

구글 스프레드에 작성된 데이터를 배열 안에 여러 객체가 담기는 형태로 API를 요청하고 이를 JavaScript 코드로 JSON 파싱하여 실시간 데이터를 사용하고 싶다.

Fetch API를 사용하여 비동기적으로 데이터를 요청하고 응답을 받을 것이다.

## 방법

1. 구글 스프레드 시트에서 데이터를 작성한다.
2. Apps Script 열기
   1. 스프레드시트에서 `확장 프로그램` > `Apps Script` 를 선택.
   2. 편집기가 열리면 기존 코드를 지우고 아래 코드를 붙여 넣음.

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var rows = sheet.getDataRange().getValues();
  var data = [];
  for (var r = 1; r < rows.length; r++) {
    var record = {};
    for (var c = 0; c < rows[0].length; c++) {
      record[rows[0][c]] = rows[r][c];
    }
    data.push(record);
  }
  return ContentService.createTextOutput(JSON.stringify(data))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. 스크립트 편집기 우측 상단에서 `배포` > `새 배포` 클릭.
4. 아래 각 항목처럼 설정한다.

|             구분             | 설정 내용                 |
| :--------------------------: | ------------------------- |
|             설명             | 배포의 이름. 임의로 작성. |
| 다음 사용자 인증 정보로 실행 | 나(구글 이메일)           |
|  엑세스 권한이 있는 사용자   | 모든 사용자               |

5. `배포` 버튼 클릭.
6. 배포ID (API key)와 웹 앱 (API 주소)가 나온다. 이를 잘 가지고 있는다. 까먹었어도 `배포` 버튼을 다시 클릭하고 `배포 관리`를 누르면 볼 수 있다.
7. 자바스크립트에서 아래와 같이 코드를 작성하면 API를 받아올 수 있다. console.log 자리에 코드를 이어나가면 된다.

```javascript
// API URL 정의
const apiUrl = '배포url + API key';

// 데이터를 비동기적으로 가져오는 함수
async function fetchData() {
  try {
    const response = await fetch(apiUrl); // API 호출
    if (!response.ok) { // 응답이 성공적이지 않을 경우 예외 처리
      throw new Error('Network response was not ok');
    }
    const data = await response.json(); // 응답 데이터를 JSON으로 파싱
    console.log(data); // 콘솔에 데이터 출력
  } catch (error) {
    console.error('Fetch error: ', error.message);
  }
}

// fetchData 함수를 호출하여 API에서 데이터를 가져옵니다.
fetchData();
```

## 여담

- 실험해보니 구글 스프레드시트에서 데이터 행을 추가하면서 콘솔을 확인했더니 API에 반영되기 까지 딜레이가 거의 없는 수준이다.

- API 요청이란?

  - API(응용 프로그래밍 인터페이스) 요청이란 컴퓨터나 스마트폰 등의 인터넷을 통해 다른 컴퓨터나 서버에 특정 정보를 요청하는 행위를 말함.
  - 예를 들어 naver.com라는 url을 입력해서 브라우저에 웹 페이지를 불러오는 것처럼 API 주소를 입력해서 정보에 접근하는 것을 의미함.

- JSON 형태로의 파싱이란?

  - JSON(JavaScript Object Notation)은 데이터의 형식인데 JavaScript에서 표준 포맷으로 정해서 많이 사용하고 있는 방식임.
  - JSON은 텍스트 기반의 데이터 구조임.
  - 파싱(parsing)이란 JSON 형태의 텍스트를 컴퓨터가 이해하고 사용할 수 있는 실제 데이터(객체, 배열)로 변환하는 과정을 말함.

- API 요청 보내기

  - API 주소와 API KEY를 발급 받아야 함.
    - 아무에게나 정보를 무분별하게 띄워주면 안 되기 때문. PublicAPI에도 횟수 제한이 있는 경우도 많음.
  - API를 호출하기 위해서는 fetch() 함수를 이용함.

  ```javascript
  fetch('https://jsonplaceholder.typicode.com/posts/1') // 이 주소는 test용 API
    .then(response => response.json()) // 서버로부터 받은 응답을 JSON으로 파싱
    .then(data => console.log(data))   // 파싱된 데이터를 콘솔에 출력
    .catch(error => console.error('Error fetching data:', error)); // 오류 처리
  ```

  ```javascript
  // API주소로 부터 정보를 가져오라고 요청함.
  fetch('API주소')
  ```

  ```javascript
  // API fetch 요청을 서버에서 응해서 데이터를 넘겨줄 때 reponse라는 객체로 넘겨줌.
  // 이것을 response라는 이름을 가진 json 파일로 바꿔줌. 이렇게 바꿔야 우리가 자바스크립트에서 사용하기 편한 데이터 형태로 가공됨.
  .then(response => response.json())
  ```

  ```javascript
  // 위 단계에서 json 파일로 바뀐 데이터를 data라는 변수로 할당함. 그리고 data가 제대로 출력되는지 콘솔에 출력해봄.
  // 코드를 이어나가고 싶다면 console.log 자리에 작성해나가면 됨. (두번째 then 메서드인 이 코드블럭 스코프 안에)
  .then(data => console.log(data))
  ```

  ```javascript
  // 만약 API 서버에서 어떤 이유로 문제가 생기면 이 부분의 코드가 실행됨.
  // 콘솔에 에러를 띄우라는 이야기임. (data를 요청할 수 없다는 에러)
  .catch(error => console.error('Error fetching data:', error))
  ```

  