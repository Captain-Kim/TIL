## 코드

```javascript
window.location.href = "login.html";
```

## 예시

```javascript
document
    .getElementById("logoutButton")
    .addEventListener("click", function () {
          // 로그아웃: 로컬 스토리지에서 로그인 상태 제거
        localStorage.removeItem("isLoggedIn");
          // 로그아웃 후 login.html 페이지로 이동
        window.location.href = "login.html";
    });
```

HTML에서 logoutButton이라는 ID를 가진 버튼을 클릭하면 