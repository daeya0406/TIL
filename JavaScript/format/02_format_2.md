# 자바스크립트 고급 문법 포맷 정리

## 목차

- [1. 구조 분해 할당 (Destructuring)](#1-구조-분해-할당-destructuring)
- [2. 스프레드 연산자 (...)](#2-스프레드-연산자-)
- [3. 나머지 매개변수 (...args)](#3-나머지-매개변수-args)
- [4. 콜백 함수](#4-콜백-함수)
- [5. Promise](#5-promise)
- [6. async / await](#6-async--await)
- [7. 클로저 (Closure)](#7-클로저-closure)
- [8. 고차 함수 (Higher-order function)](#8-고차-함수-higher-order-function)
- [9. 커링 (Currying)](#9-커링-currying)
- [10. this 바인딩](#10-this-바인딩)
- [11. 클래스와 상속](#11-클래스와-상속)
- [12. 모듈 시스템](#12-모듈-시스템)
- [13. 에러 핸들링 심화](#13-에러-핸들링-심화)
- [14. 옵셔널 체이닝](#14-옵셔널-체이닝)
- [15. 널 병합 연산자](#15-널-병합-연산자)

---

## 1. 구조 분해 할당 (Destructuring)

객체나 배열에서 값을 변수로 간단히 추출하는 문법입니다.

```js
const user = { name: "Alice", age: 25 };
const { name, age } = user;

const colors = ["red", "green"];
const [first, second] = colors;
```

---

## 2. 스프레드 연산자 (...)

복사하거나 합칠 때 사용하며, 나머지 값 수집도 가능합니다.

```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3]; // [1, 2, 3]

const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }
```

---

## 3. 나머지 매개변수 (...args)

가변 인자를 받을 수 있는 함수

```js
function sum(...numbers) {
  return numbers.reduce((acc, cur) => acc + cur, 0);
}
```

---

## 4. 콜백 함수

다른 함수의 인자로 넘겨지는 함수

```js
function greet(name, callback) {
  callback(`Hello, ${name}`);
}

greet("Alice", (msg) => console.log(msg));
```

---

## 5. Promise

비동기 코드를 체인 방식으로 작성할 수 있습니다.

```js
fetch("/api/data")
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

---

## 6. async / await

Promise를 더 읽기 쉽게 사용할 수 있는 문법입니다.

```js
async function fetchData() {
  try {
    const res = await fetch("/api/data");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

---

## 7. 클로저 (Closure)

함수가 외부 스코프의 변수를 기억하는 성질

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

---

## 8. 고차 함수 (Higher-order function)

함수를 인자로 받거나, 함수를 반환하는 함수

```js
function repeat(n, callback) {
  for (let i = 0; i < n; i++) {
    callback(i);
  }
}

repeat(3, console.log);
```

---

## 9. 커링 (Currying)

함수가 인자를 나눠서 여러 번 받는 형태

```js
function multiply(a) {
  return function (b) {
    return a * b;
  };
}

const double = multiply(2);
console.log(double(5)); // 10
```

---

## 10. this 바인딩

함수 호출 방식에 따라 `this`가 가리키는 대상이 달라집니다.

```js
const user = {
  name: "Alice",
  sayHi() {
    console.log(`Hi, I'm ${this.name}`);
  },
};

user.sayHi(); // Alice

const fn = user.sayHi;
fn(); // undefined or window (strict mode: undefined)
```

---

## 11. 클래스와 상속

객체지향 방식으로 코드를 작성할 수 있게 해주는 문법

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog("Rex");
dog.speak(); // Rex barks.
```

---

## 12. 모듈 시스템

파일 간의 코드를 분리해서 import/export로 관리

```js
// hello.js
export const sayHello = () => console.log("Hello!");

// main.js
import { sayHello } from "./hello.js";
sayHello();
```

---

## 13. 에러 핸들링 심화

try/catch/finally 구문 + 커스텀 에러

```js
try {
  throw new Error("Something went wrong");
} catch (e) {
  console.error(e.message);
} finally {
  console.log("항상 실행됨");
}
```

---

## 14. 옵셔널 체이닝

값이 없는 객체를 안전하게 탐색

```js
const user = {};
console.log(user?.profile?.name); // undefined (에러 없음)
```

---

## 15. 널 병합 연산자

null 또는 undefined 일 때만 기본값을 사용

```js
const name = null;
const displayName = name ?? "익명";
console.log(displayName); // 익명
```

---
