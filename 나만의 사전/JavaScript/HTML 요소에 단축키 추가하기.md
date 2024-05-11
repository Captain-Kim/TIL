## 상황

간단하게 지출번호를 입력하고 버튼을 클릭하면 이벤트 리스너가 동작하여 문서를 만들어주는 웹 페이지를 만들었다.

하단으로 쭉 HTML이 작성되는데, 끝까지 확인한 후 다시 위로 가서 다른 지출번호를 입력해서 다른 문서를 확인하는 이 과정 자체를 줄이고 싶다.

## 해결 방법

1.  top 버튼을 만들어 하단에서 최상단으로 버튼 클릭 한 번에 이동한다. --> 그런데 스크롤은 위로 한 번에 올라가더라도 어쨌든 input 클릭 한 번 하고 버튼을 또 눌러야 하기 때문에 이벤트가 크게 줄진 않을 것 같다.
2.  단축키를 만들어서 한 번에 포커스를 input으로 옮기고, 지출번호를 타이핑한 뒤 단축키를 또 만들어서 버튼을 눌러 이벤트 리스너를 동작시킨다. --> 이벤트 횟수 자체는 줄지 않았지만 적어도 마우스를 안 잡아도 돼서 속도가 빨라진다.

## 코드

이벤트 리스너 내부에서 if문과 else...if문으로 단축키를 여러 개 설정할 수 있다.

```javascript
// 문서 전체에 이벤트 리스너를 단다.
// keydown, 즉 키보드를 누르는 이벤트를 감지한다. 매개변수로는 event를 넣는다.
document.addEventListener('keydown', function(event) {
  // 조건문으로 ctrl, shift이 눌렸는지 확인하고, keyCode는 키보드에서 기능키 외 다른 키를 감지할 때 사용하는데
  // 숫자키 1은 49부터 시작한다.
    if (event.ctrlKey && event.shiftKey && event.keyCode === 49) { // 49는 숫자 1의 키 코드입니다.
        // donationNumberExpenditure 입력 필드에 포커스를 준다. 커서를 이동시킨다는 뜻이다.
        document.getElementById('donationNumberExpenditure').focus();
      // else...if문으로 두번째 단축키를 정의한다.
    } else if (event.ctrlKey && event.shiftKey && event.keyCode === 50)
        // createDocBtn 버튼의 클릭 이벤트를 트리거한다. 즉 클릭을 발생시킨다는 뜻이다.
        document.getElementById('create_doc_button').click();
    }
});
```