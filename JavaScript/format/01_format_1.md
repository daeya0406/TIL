# 자바스크립트 필수 문법 포맷 정리

## 목차

- [1. 조건문](#1-조건문)
- [2. 삼항 연산자](#2-삼항-연산자)
- [3. for 반복문](#3-for-반복문)
- [4. for...of](#4-forof)
- [5. for...in](#5-forin)
- [6. forEach](#6-foreach)
- [7. map](#7-map)
- [8. filter](#8-filter)
- [9. find](#9-find)
- [10. 객체 키/값 다루기](#10-객체-키값-다루기)
- [11. 함수 선언](#11-함수-선언)
- [12. 화살표 함수](#12-화살표-함수)
- [13. 객체 선언 & 접근](#13-객체-선언--접근)
- [14. 배열 선언 & 접근](#14-배열-선언--접근)
- [15. 배열 조작 메서드](#15-배열-조작-메서드)
- [16. 문자열 메서드](#16-문자열-메서드)
- [17. 템플릿 문자열](#17-템플릿-문자열)
- [18. try...catch](#18-trycatch)
- [19. setTimeout / setInterval](#19-settimeout--setinterval)
- [20. JSON](#20-json)

---

## 1. 조건문

조건에 따라 코드 실행을 분기한다.

**포맷**

```js
if (조건) {
  // 실행
} else if (다른조건) {
  // 실행
} else {
  // 나머지
}
```

**예시**

```js
if (score >= 60) {
  console.log("Pass");
} else {
  console.log("Fail");
}
```

---

## 2. 삼항 연산자

한 줄로 조건 분기할 수 있다.

**포맷**

```js
const 변수 = 조건 ? 값1 : 값2;
```

**예시**

```js
const result = score > 60 ? "pass" : "fail";
```

---

## 3. for 반복문

일반적인 카운터 기반 반복문이다.

**포맷**

```js
for (let i = 0; i < n; i++) {
  // 실행
}
```

**예시**

```js
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

---

## 4. for...of

배열의 **값**을 순회한다.

**포맷**

```js
for (const 값 of 배열) {
  // 실행
}
```

**예시**

```js
for (const item of ["a", "b", "c"]) {
  console.log(item);
}
```

---

## 5. for...in

객체의 **key**를 순회한다.

**포맷**

```js
for (const 키 in 객체) {
  // 실행
}
```

**예시**

```js
const user = { name: "Alice", age: 25 };
for (let key in user) {
  console.log(key, user[key]);
}
```

---

## 6. forEach

배열 전용 메서드로, 각 요소마다 함수를 실행한다.

**포맷**

```js
배열.forEach((값, 인덱스) => {
  /* 실행 */
});
```

**예시**

```js
[10, 20, 30].forEach((item, index) => {
  console.log(index, item);
});
```

---

## 7. map

배열을 순회하면서 **새로운 배열을 반환**한다.

**포맷**

```js
const 새배열 = 배열.map((값, 인덱스) => 변환값);
```

**예시**

```js
const doubled = [1, 2, 3].map((n) => n * 2);
console.log(doubled); // [2, 4, 6]
```

---

## 8. filter

조건에 맞는 요소만 걸러서 새로운 배열을 만든다.

**포맷**

```js
const 새배열 = 배열.filter((값) => 조건);
```

**예시**

```js
const even = [1, 2, 3, 4, 5].filter((n) => n % 2 === 0);
console.log(even); // [2, 4]
```

---

## 9. find

조건에 맞는 **첫 번째 요소**를 반환한다.

**포맷**

```js
const 결과 = 배열.find((값) => 조건);
```

**예시**

```js
const found = [5, 12, 8].find((n) => n > 10);
console.log(found); // 12
```

---

## 10. 객체 키/값 다루기

**포맷**

```js
Object.keys(객체);
Object.values(객체);
Object.entries(객체);
```

**예시**

```js
const user = { name: "Alice", age: 25 };
console.log(Object.keys(user)); // ["name","age"]
console.log(Object.values(user)); // ["Alice",25]
console.log(Object.entries(user)); // [["name","Alice"],["age",25]]
```

---

## 11. 함수 선언

**포맷**

```js
function 함수명(매개변수) {
  return 결과;
}
```

**예시**

```js
function add(a, b) {
  return a + b;
}
```

---

## 12. 화살표 함수

**포맷**

```js
const 함수명 = (매개변수) => 결과;
```

**예시**

```js
const add = (a, b) => a + b;
```

---

## 13. 객체 선언 & 접근

**포맷**

```js
const 객체 = { 키: 값, 키2: 값2 };
객체.키;
객체["키2"];
```

**예시**

```js
const user = { name: "Alice", age: 25 };
console.log(user.name);
console.log(user["age"]);
```

---

## 14. 배열 선언 & 접근

**포맷**

```js
const 배열 = [값1, 값2, 값3];
배열[인덱스];
```

**예시**

```js
const arr = [10, 20, 30];
console.log(arr[0]); // 10
```

---

## 15. 배열 조작 메서드

**포맷**

```js
배열.push(값); // 끝에 추가
배열.pop(); // 끝에서 제거
배열.unshift(값); // 앞에 추가
배열.shift(); // 앞에서 제거
```

**예시**

```js
const arr = [1, 2];
arr.push(3); // [1,2,3]
arr.shift(); // [2,3]
```

---

## 16. 문자열 메서드

**포맷**

```js
문자열.length;
문자열.toUpperCase();
문자열.toLowerCase();
문자열.includes("값");
문자열.indexOf("값");
문자열.slice(시작, 끝);
문자열.split("구분자");
```

**예시**

```js
const str = "Hello JS";
console.log(str.includes("JS")); // true
console.log(str.slice(0, 5)); // Hello
```

---

## 17. 템플릿 문자열

**포맷**

```js
`문자열 ${변수} 문자열`;
```

**예시**

```js
const name = "Alice";
console.log(`안녕, ${name}님`);
```

---

## 18. try...catch

에러 발생 가능성이 있는 코드를 안전하게 처리한다.

**포맷**

```js
try {
  // 시도
} catch (e) {
  // 에러 처리
}
```

**예시**

```js
try {
  JSON.parse("잘못된 JSON");
} catch (e) {
  console.log("에러 발생:", e.message);
}
```

---

## 19. setTimeout / setInterval

**포맷**

```js
setTimeout(() => {
  실행;
}, 밀리초);
setInterval(() => {
  실행;
}, 밀리초);
```

**예시**

```js
setTimeout(() => {
  console.log("1초 후 실행");
}, 1000);

setInterval(() => {
  console.log("반복 실행");
}, 2000);
```

---

## 20. JSON

객체와 문자열 간 변환.

**포맷**

```js
JSON.stringify(객체); // 객체 → 문자열
JSON.parse(문자열); // 문자열 → 객체
```

**예시**

```js
const jsonStr = JSON.stringify({ name: "Alice" });
console.log(jsonStr); // '{"name":"Alice"}'

const obj = JSON.parse(jsonStr);
console.log(obj.name); // Alice
```

---
