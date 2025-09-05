# 콜백 함수 (Callback Function)

## 목차

- [1. 콜백 함수란?](#1-콜백-함수란)
- [2. 콜백 함수 기본 사용법](#2-콜백-함수-기본-사용법)
- [3. 함수에 함수 전달하기](#3-함수에-함수-전달하기)
- [4. 익명 함수 콜백](#4-익명-함수-콜백)
- [5. 화살표 함수 콜백](#5-화살표-함수-콜백)
- [6. 콜백 함수와 배열 메서드](#6-콜백-함수와-배열-메서드)
- [7. 콜백 지옥 (Callback Hell)](#7-콜백-지옥-callback-hell)

---

## 1. 콜백 함수란?

> **콜백 함수**란, 다른 함수의 **인자로 전달되어 나중에 실행되는 함수**를 말한다.

자바스크립트는 함수를 값처럼 다룰 수 있어서, 함수를 다른 함수에 인자로 넘길 수 있다.

```js
function greet(name) {
  console.log("Hello, " + name);
}

function processUser(callback) {
  const name = "DaeYa";
  callback(name);
}

processUser(greet); // Hello, DaeYa
```

---

## 2. 콜백 함수 기본 사용법

일반적으로 함수에 **함수를 인자로 넘겨 호출하는 형태**다.

```js
function sayHello() {
  console.log("Hello!");
}

function executeCallback(callback) {
  callback();
}

executeCallback(sayHello); // Hello!
```

---

## 3. 함수에 함수 전달하기

함수는 변수처럼 전달할 수 있고, 인자로 받은 함수를 원하는 시점에 실행할 수 있다.

```js
function print(callback) {
  console.log("Before callback");
  callback();
  console.log("After callback");
}

function done() {
  console.log("Callback executed");
}

print(done);
// Before callback
// Callback executed
// After callback
```

---

## 4. 익명 함수 콜백

콜백 함수를 굳이 따로 선언하지 않고, 인자 자리에 바로 **익명 함수**를 전달할 수도 있다.

```js
setTimeout(function () {
  console.log("1초 뒤 실행됨");
}, 1000);
```

---

## 5. 화살표 함수 콜백

익명 함수를 **화살표 함수**로 간단히 표현할 수 있다.

```js
setTimeout(() => {
  console.log("화살표 함수 콜백 실행");
}, 1000);
```

---

## 6. 콜백 함수와 배열 메서드

`map`, `filter`, `forEach` 등의 배열 메서드는 콜백 함수를 인자로 받는다.

```js
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(function (num) {
  return num * 2;
});

console.log(doubled); // [2, 4, 6, 8]
```

---

## 7. 콜백 지옥 (Callback Hell)

콜백 함수를 **중첩해서 사용하면 코드가 매우 복잡해지는 현상**이 생긴다. 이를 "콜백 지옥"이라고 부른다.

```js
setTimeout(() => {
  console.log("1초 후 실행");
  setTimeout(() => {
    console.log("2초 후 실행");
    setTimeout(() => {
      console.log("3초 후 실행");
    }, 1000);
  }, 1000);
}, 1000);
```

> 이런 문제는 나중에 **Promise**나 **async/await**로 해결한다.

---
