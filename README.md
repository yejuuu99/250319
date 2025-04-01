# LocalStorage 메모장과 카운트다운 타이머 구현

## 메모장 기능

### 개요
브라우저의 `localStorage`를 활용하여 간단한 메모장을 구현했다. 사용자가 입력한 내용은 브라우저를 새로고침해도 유지되며, 초기화 버튼을 누르면 저장된 내용을 삭제할 수 있다.

### 주요 기능
- `localStorage`를 사용해 사용자가 입력한 메모 내용을 저장한다.
- 새로고침 후에도 입력한 내용이 유지된다.
- 초기화 버튼을 누르면 저장된 데이터가 삭제된다.

### localStorage 사용법 정리
- 데이터 저장: `localStorage.setItem("key", "value")`
- 데이터 조회: `localStorage.getItem("key")`
- 특정 데이터 삭제: `localStorage.removeItem("key")`
- 전체 데이터 삭제: `localStorage.clear()`

### 코드 예시
```html
<textarea id="memo" rows="10" cols="50" placeholder="메모를 입력하세요!"></textarea>
<button id="clear">초기화</button>

<script>
  const memo = document.getElementById("memo");
  const clearBtn = document.getElementById("clear");
  const STORAGE_KEY = "userMemo";

  memo.value = localStorage.getItem(STORAGE_KEY) || "";

  memo.addEventListener("input", () => {
    localStorage.setItem(STORAGE_KEY, memo.value);
  });

  clearBtn.addEventListener("click", () => {
    localStorage.removeItem(STORAGE_KEY);
    memo.value = "";
  });
</script>
```

### JSON 형식으로 객체 저장하기
`localStorage`는 문자열만 저장할 수 있으므로, 객체는 JSON 형식으로 변환하여 저장해야 한다.

```javascript
const user = { name: "홍길동", age: 30 };
localStorage.setItem("userInfo", JSON.stringify(user));

const storedUser = JSON.parse(localStorage.getItem("userInfo"));
console.log(storedUser.name); // "홍길동"
```

### localStorage와 sessionStorage 비교
| 항목 | localStorage | sessionStorage |
|------|--------------|----------------|
| 데이터 유지 기간 | 브라우저를 종료해도 유지 | 탭을 닫으면 삭제됨 |
| 새로고침 시 유지 여부 | 유지됨 | 유지됨 |
| 여러 탭 간 공유 | 가능 | 불가능 |

---

## 카운트다운 타이머 기능

### 개요
10초부터 1초씩 감소하는 카운트다운 타이머를 구현했다. 시작, 일시정지, 리셋 기능을 제공한다.

### 주요 기능
- 타이머 시작: 1초 간격으로 숫자가 감소하며 화면에 표시된다.
- 일시정지: 타이머의 진행을 멈춘다.
- 리셋: 타이머를 초기값(10초)으로 되돌린다.

### 작동 방식 설명
- **시작 버튼을 클릭하면**, `setInterval()`을 통해 1초마다 `time` 변수를 1씩 줄이고, 그 값을 화면에 표시한다.
- **일시정지 버튼을 클릭하면**, `clearInterval()`로 반복 작업을 중단하고, `interval` 변수에 `null`을 할당하여 멈춘 상태로 만든다.
- **다시 시작 버튼을 누르면**, `interval` 값이 비어 있는 경우에만 `setInterval()`이 다시 실행되므로, 중복 실행 없이 이전 상태에서 이어서 진행된다.
- **리셋 버튼을 클릭하면**, 현재 동작 중인 타이머를 멈추고 `time` 값을 10으로 초기화한 뒤, 화면에 초기값을 다시 표시한다.

### 코드 설명
```html
<h1>카운트다운 타이머</h1>
<div id="timer">10</div>
<button onclick="startTimer()">시작</button>
<button onclick="pauseTimer()">일시정지</button>
<button onclick="resetTimer()">리셋</button>

<script>
  let time = 10;
  let interval = null;
  let isPaused = false;

  function updateDisplay() {
    document.getElementById("timer").textContent = time > 0 ? time : "타이머 종료!";
  }

  function startTimer() {
    if (interval) return;

    isPaused = false;
    interval = setInterval(() => {
      if (time > 0) {
        time--;
        updateDisplay();
      }
      if (time === 0) {
        clearInterval(interval);
        interval = null;
      }
    }, 1000);
  }

  function pauseTimer() {
    if (interval) {
      clearInterval(interval);
      interval = null;
      isPaused = true;
    }
  }

  function resetTimer() {
    clearInterval(interval);
    interval = null;
    time = 10;
    isPaused = false;
    updateDisplay();
  }
</script>
```

### 주요 개념 정리
- `let` 키워드: 변수를 선언할 때 사용하며, 값 변경이 가능하다.
- `function`: 특정 작업을 수행하는 코드 블록을 정의한다.
- `setInterval()`: 지정한 시간 간격(밀리초)마다 콜백 함수를 반복 실행한다.
- `clearInterval()`: `setInterval()`로 시작한 반복 작업을 중단한다.
- `textContent`: 요소 내부의 텍스트를 설정하거나 가져온다.

---

## 정리
- `localStorage`는 데이터를 브라우저에 영구 저장하며, 새로고침 및 브라우저 재실행 시에도 유지된다.
- 카운트다운 타이머는 setInterval과 clearInterval을 활용하여 시간 흐름을 제어하며, UI에 즉시 반영되도록 구현할 수 있다.
- 타이머는 상태 변수(`interval`, `isPaused`)를 활용해 시작, 정지, 리셋 동작을 명확히 구분한다.

