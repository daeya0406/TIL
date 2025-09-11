# Cursor 기반 페이지네이션 정리

## 목차

- [1. Cursor란?](#1-cursor란)
- [2. Cursor 방식 동작 흐름](#2-cursor-방식-동작-흐름)
- [3. 코드 예제](#3-코드-예제)
- [4. 주의사항](#4-주의사항)

---

## 1. Cursor란?

- 서버에서 데이터를 나눠서 가져올 때 **"다음 데이터를 어디서부터 가져와야 하는지"**를 알려주는 기준점.
- 흔히 말하는 마우스 커서가 아니라, **데이터 위치를 가리키는 포인터 역할**.
- 예: 데이터가 100개일 때, `cursor=20`이면 21번째부터 데이터를 가져오라는 뜻.

---

## 2. Cursor 방식 동작 흐름

1. 클라이언트가 `cursor=null`로 요청 → 서버가 처음 20개 데이터와 `nextCursor=20`을 줌
2. 클라이언트가 `cursor=20`으로 다시 요청 → 서버가 21~40번째 데이터와 `nextCursor=40`을 줌
3. 이런 식으로 계속 이어서 요청 → 무한 스크롤이나 페이지네이션 구현 가능

---

## 3. 코드 예제

React에서 cursor 기반 데이터 로딩 함수 예시:

```js
const handleLoad = async (options) => {
  const {
    foods, // 받아온 데이터
    paging: { nextCursor }, // 다음 요청 기준점
  } = await getFoods(options);

  if (!options.cursor) {
    // 첫 요청 → 기존 데이터 덮어쓰기
    setItems(foods);
  } else {
    // 추가 요청 → 기존 데이터 뒤에 이어붙이기
    setItems((prevItems) => [...prevItems, ...foods]);
  }

  // 다음 요청을 위해 cursor 업데이트
  setCursor(nextCursor);
};
```

---

## 4. 주의사항

- `cursor`는 마우스 커서와 전혀 관련 없음.
- 단순히 "데이터를 이어 가져올 기준" 역할.
- 서버와 클라이언트가 **같은 규칙으로 cursor를 주고받아야** 정상 동작함.

---
