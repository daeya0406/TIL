# 배열의 map() 함수

## 목차

- [1. map() 함수란?](#1-map-함수란)
- [2. 기본 사용법](#2-기본-사용법)
- [3. map()과 JSX (React 예시)](#3-map과-jsx-react-예시)
- [4. key 속성 사용](#4-key-속성-사용)
- [5. 조건부 렌더링과 함께 사용](#5-조건부-렌더링과-함께-사용)

---

## 1. map() 함수란?

`map()` 함수는 배열의 각 요소를 순회하면서 콜백 함수를 실행하고,  
그 결과값을 모아 **새로운 배열**을 반환한다.  
원본 배열은 변경되지 않는다.

```js
const nums = [1, 2, 3];
const doubled = nums.map((n) => n \* 2);

console.log(doubled); // [2, 4, 6]
```

---

## 2. 기본 사용법

콜백 함수의 반환값들이 새로운 배열을 구성한다.

```js
const fruits = ["apple", "banana", "cherry"];
const upperFruits = fruits.map((fruit) => fruit.toUpperCase());

console.log(upperFruits); // ["APPLE", "BANANA", "CHERRY"]
```

---

## 3. map()과 JSX (React 예시)

React에서 배열 데이터를 컴포넌트 목록으로 변환할 때 자주 사용된다.

```js
const items = ["🍕 Pizza", "🍔 Burger", "🍣 Sushi"];

function FoodList() {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}
```

---

## 4. key 속성 사용

`map()`을 사용할 때는 각 항목에 고유한 `key` 속성을 부여하는 것이 좋다.  
React가 효율적으로 리스트를 렌더링하고 업데이트하기 위해 필요하다.

```js
const users = [
  { id: 1, name: "DaeYa" },
  { id: 2, name: "Jane" },
];

function UserList() {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## 5. 조건부 렌더링과 함께 사용

`map()` 안에서 `if`문 또는 삼항 연산자를 사용해 조건에 따라 렌더링을 제어할 수 있다.

```js
const scores = [95, 62, 85, 40];

function PassList() {
  return (
    <ul>
      {scores.map((score, i) =>
        score >= 60 ? <li key={i}>Pass: {score}</li> : null
      )}
    </ul>
  );
}
```

---
