# 객체(Object)

## 목차

- [1. 객체 선언](#1-객체-선언)
- [2. 프로퍼티 접근](#2-프로퍼티-접근)
- [3. 프로퍼티 추가](#3-프로퍼티-추가)
- [4. 프로퍼티 수정](#4-프로퍼티-수정)
- [5. 프로퍼티 삭제](#5-프로퍼티-삭제)
- [6. 프로퍼티 존재 확인](#6-프로퍼티-존재-확인)
- [7. const 객체와 프로퍼티 조작](#7-const-객체와-프로퍼티-조작)
- [8. 객체에 메서드 정의](#8-객체에-메서드-정의)

---

## 1. 객체 선언

객체는 `{}` 안에 키와 값을 넣어 선언한다.

```js
let person = {
  name: "Alice",
  age: 30,
  hobby: "cycling",
};

console.log("초기 person:", person);
// 초기 person: { name: 'Alice', age: 30, hobby: 'cycling' }
```

---

## 2. 프로퍼티 접근

또는 []로 프로퍼티에 접근할 수 있다.

```js
let name = person.name;
let age = person["age"];
let property = "hobby";
let hobby = person[property];

console.log("n[접근]");
console.log("name:", name); // Alice
console.log("age:", age); // 30
console.log("hobby:", hobby); // cycling
```

---

## 3. 프로퍼티 추가

없는 키에 값을 넣으면 새로운 프로퍼티가 추가된다.

```js
person.job = "backend developer";
person["favoriteFood"] = "pizza";

console.log("n[추가]");
console.log(person);
// { name: 'Alice', age: 30, hobby: 'cycling', job: 'backend developer', favoriteFood: 'pizza' }
```

---

## 4. 프로퍼티 수정

이미 존재하는 프로퍼티에 값을 넣으면 수정된다.

```js
person.job = "designer";
person["favoriteFood"] = "sushi";

console.log("n[수정]");
console.log(person);
// { name: 'Alice', age: 30, hobby: 'cycling', job: 'designer', favoriteFood: 'sushi' }
```

---

## 5. 프로퍼티 삭제

delete 키워드로 프로퍼티를 제거할 수 있다.

```js
delete person.job;
delete person["favoriteFood"];

console.log("n[삭제]");
console.log(person);
// { name: 'Alice', age: 30, hobby: 'cycling' }
```

---

## 6. 프로퍼티 존재 확인

in 연산자로 특정 프로퍼티가 있는지 확인할 수 있다.

```js
let result1 = "name" in person;
let result2 = "dog" in person;

console.log("n[존재 확인 - in]");
console.log('"name" in person ->', result1); // true
console.log('"dog" in person  ->', result2); // false
```

---

## 7. const 객체와 프로퍼티 조작

const로 선언한 객체도 내부 프로퍼티는 추가/수정/삭제가 가능하다.

```js
const animal = {
  type: "고양이",
  name: "나비",
  color: "black",
};

console.log("n[const 객체 초기값]");
console.log(animal);
// { type: '고양이', name: '나비', color: 'black' }

animal.age = 2; // 추가
animal.name = "까망이"; // 수정
delete animal.color; // 삭제

console.log("n[const 객체 조작 후]");
console.log(animal);
// { type: '고양이', name: '까망이', age: 2 }
```

---

## 8. 객체에 메서드 정의

객체 안에 함수를 넣으면 메서드라고 부른다.

```js
const user = {
  name: "김정대",
  sayHi() {
    console.log("안녕!");
  },
};

console.log("\n[메서드 실행]");
user.sayHi(); // 안녕!
user["sayHi"](); // 안녕!
```
