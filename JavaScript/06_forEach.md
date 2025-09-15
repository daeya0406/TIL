# forEach() 함수

## 목차

- [1. forEach() 함수란?](#1-foreach-함수란)
- [2. 기본 문법](#2-기본-문법)
- [3. 콜백 함수 구조](#3-콜백-함수-구조)
- [4. for 반복문과 차이점](#4-for-반복문과-차이점)
- [5. 인덱스와 함께 사용하기](#5-인덱스와-함께-사용하기)
- [6. 객체 배열에서 사용하기](#6-객체-배열에서-사용하기)
- [7. return 불가: map()과의 차이](#7-return-불가-map과의-차이)

---

## 1. forEach() 함수란?

`forEach()`는 배열의 각 요소를 순회하면서 주어진 **콜백 함수를 실행**하는 배열 메서드다.
**값을 반환하지 않고** 단순 반복 작업에 적합하다.

```js
const numbers = [1, 2, 3];

numbers.forEach(function (num) {
  console.log(num);
});
// 1
// 2
// 3
```

---

## 2. 기본 문법

```js
array.forEach(function (value, index, array) {
  // 실행할 코드
});
```

- `value`: 현재 요소
- `index`: 현재 인덱스
- `array`: 원본 배열

---

## 3. 콜백 함수 구조

```js
const fruits = ["apple", "banana", "cherry"];

fruits.forEach(function (fruit, i) {
  console.log(i + ":", fruit);
});
// 0: apple
// 1: banana
// 2: cherry
```

---

## 4. for 반복문과 차이점

`forEach()`는 배열 전용이며, 종료(`break`, `return`)할 수 없다.

```js
const arr = [10, 20, 30];

arr.forEach((value) => {
  console.log(value);
});
// 10
// 20
// 30

// 아래는 안 됨! → SyntaxError
// return 또는 break 불가
```

---

## 5. 인덱스와 함께 사용하기

```js
const animals = ["dog", "cat", "rabbit"];

animals.forEach((animal, index) => {
  console.log(`${index}: ${animal}`);
});
// 0: dog
// 1: cat
// 2: rabbit
```

---

## 6. 객체 배열에서 사용하기

```js
const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
];

users.forEach((user) => {
  console.log(`${user.name} is ${user.age} years old`);
});
// Alice is 25 years old
// Bob is 30 years old
```

---

## 7. return 불가: map()과의 차이

`forEach()`는 값을 반환하지 않지만, `map()`은 **새 배열을 반환**한다.

```js
const nums = [1, 2, 3];

// forEach → undefined 반환
const result1 = nums.forEach((n) => n \* 2);
console.log(result1); // undefined

// map → 새 배열 반환
const result2 = nums.map((n) => n \* 2);
console.log(result2); // [2, 4, 6]
```

---
