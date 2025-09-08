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

객체나 배열에서 값을 변수로 간단히 추출하는 문법이다.

**포맷**

```js
const { 키1, 키2: 별칭, ...rest } = 객체;
const [첫, 둘, ...나머지] = 배열;
```

**예시**

```js
const user = { name: "Alice", age: 25 };
const { name, age } = user;

const colors = ["red", "green"];
const [first, second] = colors;
```

---

## 2. 스프레드 연산자 (...)

복사하거나 합칠 때 사용하며, 나머지 값 수집도 가능하다.

**포맷**

```js
const 새배열 = [...배열1, ...배열2];
const 새객체 = { ...객체1, ...객체2, 덮어쓸키: 값 };
```

**예시**

```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3]; // [1,2,3]

const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a:1, b:2 }
```

---

## 3. 나머지 매개변수 (...args)

가변 인자를 받을 수 있는 함수.

**포맷**

```js
function 함수명(고정, ...가변) {
  /* 가변은 배열 */
}
```

**예시**

```js
function sum(...numbers) {
  return numbers.reduce((acc, cur) => acc + cur, 0);
}
```

---

## 4. 콜백 함수

다른 함수의 인자로 넘겨지는 함수.

**포맷**

```js
function 상위(인자, 콜백) {
  /* 필요 시 콜백(값) 호출 */
}
상위(값, (결과) => {
  /* 후속 처리 */
});
```

**예시**

```js
function greet(name, callback) {
  callback(`Hello, ${name}`);
}
greet("Alice", (msg) => console.log(msg));
```

---

## 5. Promise

비동기 코드를 체인 방식으로 작성할 수 있다.

**포맷**

```js
비동기함수()
  .then(결과 => /* 처리 */)
  .catch(에러 => /* 처리 */)
  .finally(() => /* 항상 */);
```

**예시**

```js
fetch("/api/data")
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

---

## 6. async / await

Promise를 더 읽기 쉽게 사용할 수 있는 문법이다.

**포맷**

```js
async function 함수명() {
  try {
    const 결과 = await 프로미스();
    /* 사용 */
  } catch (e) {
    /* 에러 처리 */
  }
}
```

**예시**

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

함수가 외부 스코프의 변수를 기억하는 성질.

**포맷**

```js
function 바깥() {
  let 숨김 = 초기값;
  return function 안() {
    /* 숨김 접근 가능 */
  };
}
const f = 바깥();
```

**예시**

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}
const counter = outer();
counter(); // 1
counter(); // 2
```

---

## 8. 고차 함수 (Higher-order function)

함수를 인자로 받거나, 함수를 반환하는 함수.

**포맷**

```js
function HOF(fn) {
  return (x) => fn(x);
} // 함수를 인자로
function maker(a) {
  return (b) => a + b;
} // 함수를 반환
```

**예시**

```js
function repeat(n, callback) {
  for (let i = 0; i < n; i++) callback(i);
}
repeat(3, console.log);
```

---

## 9. 커링 (Currying)

함수가 인자를 나눠서 여러 번 받는 형태.

**포맷**

```js
const curried = (a) => (b) => (c) => /* 결과 */;
```

**예시**

```js
function multiply(a) {
  return function (b) {
    return a * b;
  };
}
const double = multiply(2);
double(5); // 10
```

---

## 10. this 바인딩

함수 호출 방식에 따라 `this`가 가리키는 대상이 달라진다.

**포맷**

```js
const obj = {
  x: 1,
  f() {
    return this.x;
  },
};
obj.f(); // obj
const g = obj.f;
g(); // undefined(엄격모드) / window
g.call(obj); // this 수동 바인딩
```

**예시**

```js
const user = {
  name: "Alice",
  sayHi() {
    console.log(`Hi, I'm ${this.name}`);
  },
};
user.sayHi(); // Alice

const fn = user.sayHi;
fn(); // undefined or window
```

---

## 11. 클래스와 상속

객체지향 방식으로 코드를 작성할 수 있게 해주는 문법.

**포맷**

```js
class 부모 {
  constructor(매개변수) {
    this.프로퍼티 = 매개변수;
  }
  메서드() {
    /* 기본 동작 */
  }
}
class 자식 extends 부모 {
  constructor(매개변수) {
    super(매개변수); // 부모 초기화
  }
  메서드() {
    /* 오버라이드 */
  }
}
const obj = new 자식("값");
obj.메서드();
```

**예시**

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
new Dog("Rex").speak(); // Rex barks.
```

---

## 12. 모듈 시스템

파일 간의 코드를 분리해서 import/export로 관리.

**포맷**

```js
// export
export const 변수 = 값;
export function 함수() {}
export default 기본값;

// import
import 기본값, { 변수, 함수 as 별칭 } from "./모듈.js";
```

**예시**

```js
// hello.js
export const sayHello = () => console.log("Hello!");

// main.js
import { sayHello } from "./hello.js";
sayHello();
```

---

## 13. 에러 핸들링 심화

try/catch/finally 구문 + 커스텀 에러.

**포맷**

```js
try {
  /* 시도 */
} catch (e) {
  /* 에러 처리 */
} finally {
  /* 항상 */
}
throw new Error("메시지");
```

**예시**

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

값이 없는 객체를 안전하게 탐색.

**포맷**

```js
obj?.a?.b?.c;
함수참조?.(인자);
배열참조?.[인덱스];
```

**예시**

```js
const user = {};
console.log(user?.profile?.name); // undefined
```

---

## 15. 널 병합 연산자

null 또는 undefined 일 때만 기본값을 사용.

**포맷**

```js
const 결과 = 값 ?? 기본값;
```

**예시**

```js
const name = null;
const displayName = name ?? "익명";
console.log(displayName); // 익명
```

---
