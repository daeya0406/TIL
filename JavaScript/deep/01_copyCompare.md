# 깊은 복사(Deep Copy)와 얕은 복사(Shallow Copy)

## 목차

- [1. 복사의 개념](#1-복사의-개념)
- [2. 얕은 복사란?](#2-얕은-복사란)
- [3. 깊은 복사란?](#3-깊은-복사란)
- [4. 얕은 복사의 예시](#4-얕은-복사의-예시)
- [5. 깊은 복사의 예시](#5-깊은-복사의-예시)
- [6. 깊은 복사 vs 얕은 복사 비교](#6-깊은-복사-vs-얕은-복사-비교)
- [7. 깊은 복사 시 주의사항](#7-깊은-복사-시-주의사항)

---

## 1. 복사의 개념

JavaScript에서 객체나 배열을 복사할 때,
**값 자체를 복사하는지**, 아니면 **주소(참조)를 복사하는지**에 따라
얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)로 나뉜다.

---

## 2. 얕은 복사란?

얕은 복사는 객체의 **1차 속성만 복사**하고,
**중첩된 객체나 배열은 주소만 복사**해서 참조를 공유하게 된다.
→ 복사본을 수정하면 원본에도 영향을 줄 수 있다.

---

## 3. 깊은 복사란?

깊은 복사는 중첩된 모든 속성까지 **재귀적으로 새로 복사**하는 방식이다.
→ 복사본과 원본은 **완전히 독립된 메모리**를 가진다.

---

## 4. 얕은 복사의 예시

```js
const original = {
  name: "Alice",
  hobby: {
    type: "cycling",
  },
};

const shallow = { ...original };

shallow.name = "Bob";
shallow.hobby.type = "swimming";

console.log(original.name); // "Alice" → 영향 없음
console.log(original.hobby.type); // "swimming" → 영향 있음 (참조 공유)
```

---

## 5. 깊은 복사의 예시

```js
const original = {
  name: "Alice",
  hobby: {
    type: "cycling",
  },
};

// JSON 방식 (단순 객체에만 사용 가능)
const deep = JSON.parse(JSON.stringify(original));

deep.hobby.type = "swimming";

console.log(original.hobby.type); // "cycling" → 영향 없음
```

---

## 6. 깊은 복사 vs 얕은 복사 비교

- **얕은 복사 (Shallow Copy)**

  - 객체의 1차 속성만 복사함
  - 중첩 객체는 **참조만 복사**
  - 복사본을 수정하면 **원본에 영향 줄 수 있음**
  - 예: `Object.assign()`, spread 연산자 (`{ ...obj }`)

- **깊은 복사 (Deep Copy)**
  - 객체의 모든 속성을 **재귀적으로 복사**
  - 중첩 객체까지 **완전히 새로운 메모리로 생성**
  - 복사본을 수정해도 **원본에 영향 없음**
  - 예: `JSON.parse(JSON.stringify(obj))`, `structuredClone()`, `cloneDeep()`

---

## 7. 깊은 복사 시 주의사항

- `JSON.parse(JSON.stringify(obj))`는 다음을 복사하지 못함:
  - `undefined`, 함수, `Symbol`, 순환 참조 등
- 대안:
  - `structuredClone()` (최신 브라우저 지원)
  - `lodash` 라이브러리의 `cloneDeep()`

```js
// 최신 브라우저 지원
const deep = structuredClone(original);

// lodash 사용
import cloneDeep from "lodash/cloneDeep";
const deep2 = cloneDeep(original);
```

---
