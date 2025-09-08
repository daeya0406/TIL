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

```js
if (조건) {
  // 실행
} else {
  // 다른 조건
}
```

---

## 2. 삼항 연산자

한 줄로 조건 분기할 수 있다.

```js
const result = score > 60 ? "pass" : "fail";
```

---

## 3. for 반복문

일반적인 카운터 기반 반복문이다.

```js
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

---

## 4. for...of

배열의 **값**을 순회한다.

```js
for (const item of array) {
  console.log(item);
}
```

---

## 5. for...in

객체의 **key**를 순회한다.

```js
for (let key in object) {
  console.log(key, object[key]);
}
```

---

## 6. forEach

배열 전용 메서드로, 각 요소마다 함수를 실행한다.

```js
array.forEach((item, index) => {
  console.log(index, item);
});
```

---

## 7. map

배열을 순회하면서 **새로운 배열을 반환**한다.

```js
const doubled = array.map((item) => item \* 2);
```

---

## 8. filter

조건에 맞는 요소만 걸러서 새로운 배열을 만든다.

```js
const even = array.filter((n) => n % 2 === 0);
```

---

## 9. find

조건에 맞는 **첫 번째 요소**를 반환한다.

```js
const found = array.find((n) => n > 10);
```

---

## 10. 객체 키/값 다루기

```js
Object.keys(obj); // ["name", "age"]
Object.values(obj); // ["Alice", 25]
Object.entries(obj); // [["name", "Alice"], ["age", 25]]
```

---

## 11. 함수 선언

```js
function add(a, b) {
  return a + b;
}
```

---

## 12. 화살표 함수

```js
const add = (a, b) => a + b;
```

---

## 13. 객체 선언 & 접근

```js
const user = {
  name: "Alice",
  age: 25,
};

console.log(user.name);
console.log(user["age"]);
```

---

## 14. 배열 선언 & 접근

```js
const arr = [10, 20, 30];
console.log(arr[0]); // 10
```

---

## 15. 배열 조작 메서드

```js
arr.push(4); // 끝에 추가
arr.pop(); // 끝에서 제거
arr.unshift(0); // 앞에 추가
arr.shift(); // 앞에서 제거
```

---

## 16. 문자열 메서드

```js
str.length;
str.toUpperCase();
str.toLowerCase();
str.includes("word");
str.indexOf("a");
str.slice(0, 3);
str.split(",");
```

---

## 17. 템플릿 문자열

```js
const name = "Alice";
console.log(`안녕, ${name}님`);
```

---

## 18. try...catch

에러 발생 가능성이 있는 코드를 안전하게 처리한다.

```js
try {
  // 위험한 코드
} catch (e) {
  console.log("에러 발생:", e);
}
```

---

## 19. setTimeout / setInterval

```js
setTimeout(() => {
  console.log("1초 후 실행");
}, 1000);

setInterval(() => {
  console.log("반복 실행");
}, 1000);
```

---

## 20. JSON

객체와 문자열 간 변환

```js
const jsonStr = JSON.stringify({ name: "Alice" }); // 객체 → 문자열
const obj = JSON.parse(jsonStr); // 문자열 → 객체
```

---
