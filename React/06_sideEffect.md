# ⚡️ 사이드 이펙트(Side Effect)와 useEffect 정리

리액트에서 **사이드 이펙트(Side Effect)** 와 이를 다루는 `useEffect` 훅에 대해 정리.

---

## 목차

- [1. 사이드 이펙트란?](#1-사이드-이펙트란)
- [2. 사이드 이펙트 예시](#2-사이드-이펙트-예시)
- [3. useEffect 기본 사용법](#3-useeffect-기본-사용법)
- [4. 동기화(Synchronization) 활용](#4-동기화-synchronization-활용)
- [5. 정리 함수(Cleanup Function)](#5-정리-함수cleanup-function)
- [6. 체크리스트](#6-체크리스트)

---

## 1. 사이드 이펙트란?

- 본래 의미: **부작용** (약 먹고 생기는 예기치 못한 효과)
- 프로그래밍 의미: 함수 실행이 **함수 외부의 상태를 바꾸는 것**

```js
let count = 0;

function add(a, b) {
  const result = a + b;
  count += 1; // 🔴 외부 변수 변경 = 사이드 이펙트
  return result;
}
```

- 대표적인 예: `console.log` (리턴 대신 콘솔에 출력)

---

## 2. 사이드 이펙트 예시

리액트 컴포넌트 안에서 자주 만나는 사이드 이펙트들:

- **DOM 직접 조작**

  ```js
  useEffect(() => {
    document.title = title;
  }, [title]);
  ```

- **네트워크 요청**

  ```js
  useEffect(() => {
    fetch("https://example.com/data")
      .then((res) => res.json())
      .then((body) => setData(body));
  }, []);
  ```

- **브라우저 저장소 사용**

  ```js
  useEffect(() => {
    localStorage.setItem("theme", theme);
  }, [theme]);
  ```

- **타이머**
  ```js
  useEffect(() => {
    const timerId = setInterval(() => {
      setSecond((s) => s + 1);
    }, 1000);
    return () => clearInterval(timerId);
  }, []);
  ```

---

## 3. useEffect 기본 사용법

```js
useEffect(() => {
  // 사이드 이펙트 실행
  return () => {
    // 필요하다면 정리(cleanup)
  };
}, [dep1, dep2]);
```

- `[dep1, dep2]` → 이 값들이 바뀔 때마다 콜백 실행
- `return` → 정리 함수. 다음 이펙트 실행 전이나 컴포넌트 언마운트 시 실행

---

## 4. 동기화(Synchronization) 활용

핸들러에서 직접 외부 상태를 바꾸면 **반복 코드**가 생긴다.

```js
// ❌ 모든 곳에서 document.title을 직접 변경해야 함
setTitle(nextTitle);
document.title = nextTitle;
```

→ `useEffect`로 분리하면 간단하고 예측 가능.

```js
// ✅ title이 바뀔 때마다 document.title 자동 동기화
useEffect(() => {
  document.title = title;
}, [title]);
```

장점:

- 코드 반복 줄어듦
- setTitle만 써도 항상 document.title이 맞춰짐
- “이 컴포넌트는 title과 document.title을 동기화한다”는 의도가 명확

---

## 5. 정리 함수(Cleanup Function)

- 사이드 이펙트 실행 후 **뒷정리**가 필요한 경우 사용
- 새로운 이펙트 실행 전에 / 컴포넌트가 사라질 때 실행

### 예: 타이머

```js
useEffect(() => {
  const id = setInterval(() => {
    console.log("타이머 실행중...");
    setSecond((s) => s + 1);
  }, 1000);

  console.log("타이머 시작 🏁");

  return () => {
    clearInterval(id);
    console.log("타이머 멈춤 ✋");
  };
}, []);
```

→ Timer 컴포넌트가 언마운트되면 자동으로 `clearInterval`.

---

## 6. 체크리스트

- [ ] 컴포넌트 함수 안에서 **외부 상태를 변경하는 코드**는 useEffect 안에 두었는가?
- [ ] 같은 동작을 여러 이벤트 핸들러에서 반복하고 있진 않은가? → useEffect로 동기화하자
- [ ] `useEffect`의 **의존성 배열**에 필요한 state/prop을 빠짐없이 넣었는가?
- [ ] 메모리나 리소스를 점유하는 이펙트라면 **정리 함수(cleanup)**를 작성했는가?
- [ ] 네트워크/스토리지/타이머 같은 “리액트 외부와 상호작용”은 useEffect에서 처리했는가?

---
