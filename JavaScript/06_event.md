# 자바스크립트 이벤트 정리

## 목차

- [1. 이벤트란?](#1-이벤트란)
- [2. 이벤트 등록 방법](#2-이벤트-등록-방법)
- [3. 이벤트 객체](#3-이벤트-객체)
- [4. target과 currentTarget](#4-target과-currenttarget)
- [5. 이벤트 흐름 (캡처링과 버블링)](#5-이벤트-흐름-캡처링과-버블링)
- [6. 이벤트 위임](#6-이벤트-위임)
- [7. 리액트에서의 SyntheticEvent](#7-리액트에서의-syntheticevent)

---

## 1. 이벤트란?

이벤트는 사용자의 행동(클릭, 입력, 스크롤 등)이나 시스템 동작(페이지 로드 등)에 의해 발생하는 신호.  
자바스크립트는 이벤트를 감지해서 원하는 코드를 실행할 수 있게 해줌.

---

## 2. 이벤트 등록 방법

자바스크립트에서 이벤트를 등록하는 방법은 여러 가지가 있음.

```js
// 1. 인라인 방식 (비추천)
<button onclick="alert('클릭!')">버튼</button>;

// 2. 프로퍼티 방식
const btn = document.querySelector("button");
btn.onclick = () => {
  console.log("버튼 클릭됨");
};

// 3. addEventListener (가장 권장되는 방식)
btn.addEventListener("click", () => {
  console.log("버튼 클릭됨");
});
```

---

## 3. 이벤트 객체

이벤트가 발생하면 브라우저는 자동으로 이벤트 객체를 핸들러에 전달.  
이 객체 안에는 이벤트와 관련된 정보가 들어 있음.

```js
document.querySelector("button").addEventListener("click", (e) => {
  console.log(e.type); // "click"
  console.log(e.target); // 클릭된 요소
});
```

자주 쓰는 속성과 메서드:

- `e.type` → 이벤트 타입
- `e.target` → 이벤트가 발생한 실제 요소
- `e.currentTarget` → 핸들러가 달린 요소
- `e.preventDefault()` → 기본 동작 막기
- `e.stopPropagation()` → 이벤트 전파 막기

---

## 4. target과 currentTarget

둘은 헷갈리기 쉬움.

```js
document.querySelector("button").addEventListener("click", (e) => {
  console.log("target:", e.target);
  console.log("currentTarget:", e.currentTarget);
});
```

- `e.target` → 실제 이벤트가 발생한 요소 (예: 버튼 안쪽의 `<span>`)
- `e.currentTarget` → 이벤트 핸들러가 달린 요소 (예: `<button>`)

---

## 5. 이벤트 흐름 (캡처링과 버블링)

브라우저는 이벤트가 발생하면 아래 순서로 전달:

1. 캡처링 단계 (최상위 → 타깃 요소까지 내려감)
2. 타깃 단계 (실제 타깃 요소에서 이벤트 실행)
3. 버블링 단계 (타깃 → 최상위까지 올라감)

```js
div.addEventListener("click", () => console.log("DIV"), true); // 캡처링
button.addEventListener("click", () => console.log("BUTTON")); // 버블링
```

---

## 6. 이벤트 위임

여러 자식 요소에 각각 핸들러를 붙이지 않고, 부모 요소에 하나의 핸들러를 달아 처리하는 방식.

```js
document.querySelector("ul").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("클릭된 아이템:", e.target.textContent);
  }
});
```

장점:

- 성능에 유리
- 동적으로 추가된 요소에도 적용 가능

---

## 7. 리액트에서의 SyntheticEvent

리액트는 브라우저 이벤트를 **SyntheticEvent**라는 객체로 감싸서 전달.  
사용법은 일반 JS 이벤트 객체와 거의 동일.

```js
function App() {
  const handleClick = (e) => {
    console.log(e.type); // "click"
    console.log(e.target.value);
    e.preventDefault();
  };

  return (
    <form onSubmit={handleClick}>
      <input type="text" />
      <button type="submit">제출</button>
    </form>
  );
}
```

주의사항:

- SyntheticEvent는 **풀링(pooling)** 때문에 비동기 코드에서 바로 쓰면 값이 사라질 수 있음.  
  → `e.persist()` 또는 필요한 값만 미리 변수에 저장해두면 안전.

---
