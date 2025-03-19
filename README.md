# 📝 LocalStorage를 활용한 간단한 메모장 구현

## 📌 개요
이 프로젝트는 `localStorage`를 활용하여 간단한 메모장을 구현한 것이다.  
입력한 내용이 브라우저를 새로고침해도 유지되며, 버튼을 눌러 초기화할 수도 있다.

## 🚀 기능
- `localStorage`를 이용하여 **사용자가 입력한 내용을 저장**할 수 있다.
- **페이지를 새로고침해도 데이터가 유지**된다.
- **초기화 버튼을 클릭하면 입력 내용이 삭제**된다.

## 📌 배운 점

### 1. LocalStorage란?
- LocalStorage는 브라우저에 데이터를 저장할 수 있는 기능이다.
- 브라우저를 껐다가 다시 켜도 데이터가 유지된다.
- 웹사이트마다 따로 저장되므로 다른 웹사이트에서는 저장한 데이터를 볼 수 없다.

### 2. LocalStorage 사용법
- 데이터를 저장: `localStorage.setItem("키", "값")`
- 저장된 데이터 가져오기: `localStorage.getItem("키")`
- 특정 데이터 삭제: `localStorage.removeItem("키")`
- 모든 데이터 삭제: `localStorage.clear()`

### 3. 메모장 기능 구현
- `textarea`에 입력한 내용을 실시간으로 저장하여 새로고침해도 유지되도록 설정했다.
- "초기화" 버튼을 누르면 저장된 내용을 삭제하도록 구현했다.

---

## 🛠 코드 설명

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>간단한 메모</title>
</head>
<body>
    <h2>메모장</h2>
    <textarea id="memo" rows="10" cols="50" placeholder="메모를 입력하세요!"></textarea>
    <br>
    <button id="clear">초기화</button>

    <script>
        const memo = document.getElementById("memo");
        const clearBtn = document.getElementById("clear");
        const STORAGE_KEY = "userMemo";

        // 저장된 메모 불러오기
        memo.value = localStorage.getItem(STORAGE_KEY) || "";

        // 입력할 때마다 localStorage에 저장
        memo.addEventListener("input", () => {
            localStorage.setItem(STORAGE_KEY, memo.value);
        });

        // 초기화 버튼 클릭 시 localStorage 데이터 삭제 및 입력창 비우기
        clearBtn.addEventListener("click", () => {
            localStorage.removeItem(STORAGE_KEY);
            memo.value = "";
        });
    </script>
</body>
</html>
```

---

## 🔥 추가 학습할 내용

### 1. `localStorage`와 `sessionStorage`의 차이점
| 기능 | `localStorage` | `sessionStorage` |
|------|--------------|----------------|
| 데이터 유지 기간 | 영구적 (삭제 전까지 유지) | 브라우저 탭을 닫으면 삭제됨 |
| 새로고침 시 데이터 유지 | 유지됨 | 유지됨 |
| 여러 탭에서 공유 가능 여부 | 가능 | 불가능 |

### 2. `localStorage`에 여러 개의 값 저장하기
- `localStorage`는 문자열만 저장할 수 있으므로, 여러 개의 데이터를 저장하려면 JSON 형식으로 변환해야 한다.

📌 **예제 코드**:
```js
const user = { name: "홍길동", age: 30 };
localStorage.setItem("userInfo", JSON.stringify(user));

const storedUser = JSON.parse(localStorage.getItem("userInfo"));
console.log(storedUser.name); // "홍길동"
```

---

## ✅ 정리
- `localStorage`는 브라우저에서 데이터를 저장하는 기능으로, **새로고침해도 데이터가 유지**된다.
- `setItem()`과 `getItem()`을 사용해 데이터를 저장하고 불러올 수 있다.
- JSON 데이터를 저장할 때는 `JSON.stringify()`와 `JSON.parse()`를 활용해야 한다.
- `localStorage`와 `sessionStorage`는 데이터 유지 기간이 다르므로 **상황에 맞게 사용해야 한다**.

