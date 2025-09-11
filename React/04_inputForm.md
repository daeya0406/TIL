# 폼 입력 다루기 정리 (onChange, htmlFor, 제어/비제어 컴포넌트, submit)

리액트에서 입력 폼을 다루는 핵심 개념을 여행 검색 예시로 정리.

---

## 목차

- [1. HTML과 다른 점: onChange, htmlFor](#1-html과-다른-점-onchange-htmlfor)
- [2. 기본 패턴: 개별 state로 제어하기](#2-기본-패턴-개별-state로-제어하기)
- [3. 한 객체로 묶어 제어하기](#3-한-객체로-묶어-제어하기)
- [4. 기본 submit 동작 막기](#4-기본-submit-동작-막기)
- [5. 제어 컴포넌트(Controlled)](#5-제어-컴포넌트controlled)
- [6. 비제어 컴포넌트(Uncontrolled)](#6-비제어-컴포넌트uncontrolled)
- [7. 언제 무엇을 쓰나 + 체크리스트](#7-언제-무엇을-쓰나--체크리스트)

---

## 1. HTML과 다른 점: onChange, htmlFor

리액트의 `onChange`는 **입력 값이 바뀔 때마다** 실행된다. (브라우저의 `oninput`과 비슷한 타이밍)

`<label>`의 `for` 속성은 JS 키워드와 겹치므로 리액트에선 **`htmlFor`** 사용.

```js
<label htmlFor="location">위치</label>
<input id="location" name="location" onChange={handleChange} />
```

---

## 2. 기본 패턴: 개별 state로 제어하기

입력마다 state 1개, `value`와 `onChange`로 연결. 예측 가능하고 가장 직관적.

```js
import { useState } from "react";

function TripSearchForm() {
  const [location, setLocation] = useState("Seoul");
  const [checkIn, setCheckIn] = useState("2022-01-01");
  const [checkOut, setCheckOut] = useState("2022-01-02");

  return (
    <form>
      <h1>검색 시작하기</h1>

      <label htmlFor="location">위치</label>
      <input
        id="location"
        name="location"
        value={location}
        placeholder="어디로 여행가세요?"
        onChange={(e) => setLocation(e.target.value)}
      />

      <label htmlFor="checkIn">체크인</label>
      <input
        id="checkIn"
        type="date"
        name="checkIn"
        value={checkIn}
        onChange={(e) => setCheckIn(e.target.value)}
      />

      <label htmlFor="checkOut">체크아웃</label>
      <input
        id="checkOut"
        type="date"
        name="checkOut"
        value={checkOut}
        onChange={(e) => setCheckOut(e.target.value)}
      />

      <button type="submit">검색</button>
    </form>
  );
}
```

---

## 3. 한 객체로 묶어 제어하기

`name`과 `value`를 이용해 **하나의 객체 state**로 관리. 필드가 많을 때 깔끔.

```js
import { useState } from "react";

function TripSearchForm() {
  const [values, setValues] = useState({
    location: "Seoul",
    checkIn: "2022-01-01",
    checkOut: "2022-01-02",
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues((prev) => ({ ...prev, [name]: value }));
  };

  return (
    <form>
      <h1>검색 시작하기</h1>

      <label htmlFor="location">위치</label>
      <input
        id="location"
        name="location"
        value={values.location}
        placeholder="어디로 여행가세요?"
        onChange={handleChange}
      />

      <label htmlFor="checkIn">체크인</label>
      <input
        id="checkIn"
        type="date"
        name="checkIn"
        value={values.checkIn}
        onChange={handleChange}
      />

      <label htmlFor="checkOut">체크아웃</label>
      <input
        id="checkOut"
        type="date"
        name="checkOut"
        value={values.checkOut}
        onChange={handleChange}
      />

      <button type="submit">검색</button>
    </form>
  );
}
```

---

## 4. 기본 submit 동작 막기

HTML 폼은 기본적으로 페이지 이동을 한다. **`e.preventDefault()`**로 막고 JS로 처리.

```js
const handleSubmit = (e) => {
  e.preventDefault();
  // 여기서 values로 검색 요청 등 수행
};
```

---

## 5. 제어 컴포넌트(Controlled)

`value`를 리액트가 **지정**하고 `onChange`로 **동기화**. 값이 항상 state/prop과 일치 → 예측 가능.

```js
function TripSearchForm({ values, onChange, onSubmit }) {
  return (
    <form onSubmit={onSubmit}>
      <h1>검색 시작하기</h1>

      <label htmlFor="location">위치</label>
      <input
        id="location"
        name="location"
        value={values.location}
        placeholder="어디로 여행가세요?"
        onChange={onChange}
      />

      <label htmlFor="checkIn">체크인</label>
      <input
        id="checkIn"
        type="date"
        name="checkIn"
        value={values.checkIn}
        onChange={onChange}
      />

      <label htmlFor="checkOut">체크아웃</label>
      <input
        id="checkOut"
        type="date"
        name="checkOut"
        value={values.checkOut}
        onChange={onChange}
      />

      <button type="submit">검색</button>
    </form>
  );
}
```

**장점**

- UI = 상태(Source of Truth) → 검증/포맷팅/동기화가 쉽다
- 다른 컴포넌트/전역 상태와 연동 용이

---

## 6. 비제어 컴포넌트(Uncontrolled)

`value`를 지정하지 않는다. 필요 시 **제출 시점**에 DOM에서 값을 읽는다.

```js
function TripSearchForm({ onSubmit }) {
  return (
    <form onSubmit={onSubmit}>
      <h1>검색 시작하기</h1>

      <label htmlFor="location">위치</label>
      <input id="location" name="location" placeholder="어디로 여행가세요?" />

      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" />

      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" />

      <button type="submit">검색</button>
    </form>
  );
}

// 사용 예: 제출 시 값 읽기
const handleSubmit = (e) => {
  e.preventDefault();
  const form = e.target;
  const location = form["location"].value;
  const checkIn = form["checkIn"].value;
  const checkOut = form["checkOut"].value;
  console.log({ location, checkIn, checkOut });
};
```

`FormData`로 한 번에 읽을 수도 있다.

```js
const handleSubmit = (e) => {
  e.preventDefault();
  const fd = new FormData(e.target);
  const payload = Object.fromEntries(fd.entries());
  console.log(payload); // { location: "...", checkIn: "...", checkOut: "..." }
};
```

**언제 필요한가?**

- 파일 입력(`type="file"`)처럼 값을 state로 직접 들고 있지 않는 게 자연스러운 경우
- 간단한 폼, 제출 시점에만 값이 필요한 경우

---

## 7. 언제 무엇을 쓰나 + 체크리스트

**권장:** 기본은 **제어 컴포넌트**

- 입력 검증, 조건부 활성화, 실시간 미리보기, 서버 동기화 등 **동적인 폼 로직**이 있을 때 강력

**비제어 사용 예:**

- 파일 업로드, 매우 단순한 폼, 외부 라이브러리와 빠르게 맞붙일 때

**체크리스트**

- [ ] `label`은 `htmlFor` 사용했나?
- [ ] 제어 컴포넌트라면 `value`와 `onChange`가 연결되어 있나?
- [ ] 제출 시 페이지 이동을 막아야 하면 `preventDefault()` 호출했나?
- [ ] 필드가 많을 땐 **객체 state + name 기반 업데이트**로 단순화했나?
- [ ] 비제어라면 `FormData`나 `ref`로 값 읽는 흐름을 정했나?

---
