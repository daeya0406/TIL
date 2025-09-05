# 배열(Array)

## 목차

- [1. 배열이란?](#1-배열이란)
- [2. 배열 선언](#2-배열-선언)
- [3. 배열 접근](#3-배열-접근)
- [4. 배열 길이 확인](#4-배열-길이-확인)
- [5. 배열 수정 (추가, 변경, 삭제)](#5-배열-수정-추가-변경-삭제)
- [6. 배열 반복문 (for, forEach)](#6-배열-반복문-for-foreach)
- [7. 주요 배열 메서드](#7-주요-배열-메서드)

---

## 1. 배열이란?

배열은 여러 개의 값을 **순차적으로 저장**하는 자료구조다.  
JavaScript 배열은 다양한 타입의 값을 함께 저장할 수 있다.

```js
const arr = [1, "hello", true];
console.log(arr); // [1, "hello", true]
```

---

## 2. 배열 선언

```js
const numbers = [10, 20, 30];
const fruits = new Array("apple", "banana", "cherry");

console.log(numbers); // [10, 20, 30]
console.log(fruits); // ["apple", "banana", "cherry"]
```

---

## 3. 배열 접근

인덱스(index)는 0부터 시작하며, `[]`를 사용해 접근한다.

```js
const colors = ["red", "green", "blue"];

console.log(colors[0]); // red
console.log(colors[2]); // blue
```

---

## 4. 배열 길이 확인

`.length` 속성을 통해 배열의 길이를 확인할 수 있다.

```js
const items = [1, 2, 3, 4, 5];

console.log(items.length); // 5
```

---

## 5. 배열 수정 (추가, 변경, 삭제)

```js
let arr = [1, 2, 3];

// 추가
arr.push(4); // 끝에 추가
arr.unshift(0); // 앞에 추가

// 변경
arr[1] = 20;

// 삭제
arr.pop(); // 마지막 제거
arr.shift(); // 첫 번째 제거

console.log(arr); // [0, 20, 3]
```

---

## 6. 배열 반복문 (for, forEach)

```js
const names = ["Alice", "Bob", "Charlie"];

// 일반 for문
for (let i = 0; i < names.length; i++) {
  console.log(names[i]);
}

// forEach
names.forEach(function (name) {
  console.log(name);
});
```

---

## 7. 주요 배열 메서드

### map()

```js
const nums = [1, 2, 3];
const doubled = nums.map((n) => n * 2);

console.log(doubled); // [2, 4, 6]
```

### filter()

```js
const nums = [1, 2, 3, 4, 5];
const even = nums.filter((n) => n % 2 === 0);

console.log(even); // [2, 4]
```

### find()

```js
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" },
];

const result = users.find((user) => user.id === 2);
console.log(result); // { id: 2, name: "B" }
```

### includes()

```js
const letters = ["a", "b", "c"];

console.log(letters.includes("b")); // true
console.log(letters.includes("z")); // false
```

### join()

```js
const words = ["Hello", "World"];
const sentence = words.join(" ");

console.log(sentence); // "Hello World"
```

### sort()

```js
const nums = [4, 2, 5, 1];
nums.sort();

console.log(nums); // [1, 2, 4, 5]
```

---
