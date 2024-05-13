## 상황

강의를 듣다 강의 전체의 이름을 가져와서 작업을 해야 하는 상황이다.

한 줄 한 줄 `li` 태그로 묶여 있고, `unit` 이라는 클래스로 지정되어 있는 듯하다.

## 해결

1.  웹 브라우저에서 개발자 도구를 연다
2.  콘솔에 아래 명령어를 입력한다.

```javascript
const listItems = document.getElementsByClassName('unit');
// 임의의 변수를 만들고 해당 클래스명을 가진 요소를 전부 찾아온다.

const texts = Array.from(listItems, item => item.textContent);
// 여러 개이니 배열로 반환되었을 건데, arr.from 메서드로 textContent만 순회하면서 return한다.
// 이를 다시 새로운 배열에 할당한다.

console.log(texts);
// 최종 완료된 배열을 확인한다
```

```javascript
// arr.from() 추가 설명
Array.from(검사할배열, 리턴할 내용(화살표 함수));
```

## 결과

```javascript
[
    "이전",
    "다음 강의",
    "강의 OT (수강대상, 강의 특징정리)",
    "강의 듣기 전 자바스크립트 기본 문법 총정리",
    "this 키워드를 알아보자 1. 함수와 Object에서 사용하면?",
    "this 키워드를 알아보자 2. event listener와 constructor",
    "Arrow function은 function을 대체하는 신문법이 아님",
    "this & arrow function 연습문제 3개",
    "this & arrow function 연습문제 해설",
    "변수 신문법 총정리 1. var let const와 선언,할당,범위",
    "변수 신문법 총정리 2. Hoisting, 전역변수, 참조",
    "변수 연습문제 6개",
    "변수 연습문제 해설",
    "자바스크립트가 문자 다루는 신기한 방법 (Template literals)",
    "Template literals / tagged literals 연습문제 2개와 풀이",
    "모든 괄호를 없애주는 Spread Operator 활용방법 1",
    "Spread Operator 활용방법 2 & apply, call 함수 알아보기",
    "자바스크립트 함수 업그레이드하기 (default parameter/arguments)",
    "함수에서 쓰는 점3개 Rest 파라미터를 알아봅시다",
    "Spread, rest 파라미터 연습문제 8개",
    "Spread, rest 파라미터 연습문제 답안",
    "이상한 Reference data type과 더 이상한 예제 3개",
    "객체지향1. Object 생성기계인 constructor를 만들어 써보자",
    "객체지향2. 이거 보고 prototype 이해 못하면 강의 접습니다",
    "객체지향3. prototype의 특징 몇가지",
    "constructor, prototype 연습문제 4개",
    "(간만에 쉬운거) ES5방식으로 쉽게 구현하는 상속기능",
    "객체지향4. ES6방식으로 안쉽게 구현하는 상속기능 (class)",
    "객체지향5. class를 복사하는 extends / super",
    "getter, setter 대체 왜 쓰는지 알아보기",
    "class, extends, getter, setter 연습문제 5개",
    "class, extends, getter, setter 연습문제 답안",
    "틀린그림 찾기능력이 향상되는 Destructuring 문법",
    "import / export 를 이용한 파일간 모듈식 개발",
    "Stack, Queue를 이용한 웹브라우저 동작원리",
    "동기/비동기처리와 콜백함수라는 용어 깔끔하게 정리",
    "인간의 언어로 설명하는 ES6 Promise",
    "ES6 Promise 간단 연습문제 & 해설",
    "Promise 어려워서 싫으면 async/await을사용합시다",
    "for in / for of 반복문과 enumerable, iterable 속성",
    "Symbol 자료형은 쓸모없어보이는데 왜 있는거죠",
    "매우 짧게 알아보는 Map, Set 자료형",
    "Web Components : 커스텀 HTML 태그 만들기",
    "shadow DOM과 template으로 HTML 모듈화하기",
    "class로 만들어보는 간단한 2D 게임 1 (배웠으면 써먹어야하니까)",
    "class로 만들어보는 간단한 2D 게임 2 (collision detection)",
    "?. / ?? 연산자 (optional chaining)"
]
```

`unit`이라는 클래스에 담긴 것까지 불필요하게 담긴 듯 하지만, 몇 개 안 되니 나머지 부분을 필요에 따라 가공하면 될 것 같다.

## 가공

그런데 나는 저 제목이 필요했던 이유가 하나 하나 파일 제목으로 사용하기 위함인데,

"001. 강의 OT (수강대상, 강의 특징정리).md",

"002. 강의 듣기 전 자바스크립트 기본 문법 총정리.md"

이런 형태로 앞에는 인덱스가 증가함에 따라 `001.` 형태로 숫자를 증가 시키고, 뒤에는 `.md`라는 확장자명을 넣어주고 싶다.

```javascript
// 번호를 붙이고 .md 확장자를 추가하는 코드
const formattedTexts = texts.map((text, index) => {
    const number = (index + 1).toString().padStart(3, '0'); // 숫자 앞에 0을 붙여서 세 자리로 만듦
    return `${number}. ${text}.md`; // 문자열을 조합
});

// 콘솔에 결과 출력
console.log(formattedTexts);
```

필요 없는 내용을 지우고, 다시 위 코드를 적용시켰더니 아래와 같이 잘 나온다.

```javascript
[
    "001. 강의 듣기 전 자바스크립트 기본 문법 총정리.md",
    "002. this 키워드를 알아보자 1. 함수와 Object에서 사용하면?.md",
    "003. this 키워드를 알아보자 2. event listener와 constructor.md",
    "004. Arrow function은 function을 대체하는 신문법이 아님.md",
    "005. this & arrow function 연습문제.md",
    "006. 변수 신문법 총정리 1. var let const와 선언,할당,범위.md",
    "007. 변수 신문법 총정리 2. Hoisting, 전역변수, 참조.md",
    "008. 변수 연습문제.md",
    "009. 자바스크립트가 문자 다루는 신기한 방법 (Template literals).md",
    "010. Template literals / tagged literals 연습문제 2개와 풀이.md",
    "011. 모든 괄호를 없애주는 Spread Operator 활용방법 1.md",
    "012. Spread Operator 활용방법 2 & apply, call 함수 알아보기.md",
    "013. 자바스크립트 함수 업그레이드하기 (default parameter/arguments).md",
    "014. 함수에서 쓰는 점3개 Rest 파라미터를 알아봅시다.md",
    "015. Spread, rest 파라미터 연습문제.md",
    "016. 이상한 Reference data type과 더 이상한 예제.md",
    "017. 객체지향1. Object 생성기계인 constructor를 만들어 써보자.md",
    "018. 객체지향2. 이거 보고 prototype 이해 못하면 강의 접습니다.md",
    "019. 객체지향3. prototype의 특징 몇가지.md",
    "020. constructor, prototype 연습문제 4개.md",
    "021. (간만에 쉬운거) ES5방식으로 쉽게 구현하는 상속기능.md",
    "022. 객체지향4. ES6방식으로 안쉽게 구현하는 상속기능 (class).md",
    "023. 객체지향5. class를 복사하는 extends / super.md",
    "024. getter, setter 대체 왜 쓰는지 알아보기.md",
    "025. class, extends, getter, setter 연습문제.md",
    "026. 틀린그림 찾기능력이 향상되는 Destructuring 문법.md",
    "027. import / export 를 이용한 파일간 모듈식 개발.md",
    "028. Stack, Queue를 이용한 웹브라우저 동작원리.md",
    "029. 동기/비동기처리와 콜백함수라는 용어 깔끔하게 정리.md",
    "030. 인간의 언어로 설명하는 ES6 Promise.md",
    "031. ES6 Promise 간단 연습문제 & 해설.md",
    "032. Promise 어려워서 싫으면 async/await을사용합시다.md",
    "033. for in / for of 반복문과 enumerable, iterable 속성.md",
    "034. Symbol 자료형은 쓸모없어보이는데 왜 있는거죠.md",
    "035. 매우 짧게 알아보는 Map, Set 자료형.md",
    "036. Web Components : 커스텀 HTML 태그 만들기.md",
    "037. shadow DOM과 template으로 HTML 모듈화하기.md",
    "038. class로 만들어보는 간단한 2D 게임 1 (배웠으면 써먹어야하니까).md",
    "039. class로 만들어보는 간단한 2D 게임 2 (collision detection).md",
    "040. ?. / ?? 연산자 (optional chaining).md"
]
```