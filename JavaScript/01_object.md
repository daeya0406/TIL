# 객체(Object) 프로퍼티 다루기 — 한 코드 프레임 예시

```js
// ---------------------------------------------------------
// 1. 객체 선언
let person = {
  name: "Alice",
  age: 30,
  hobby: "cycling",
};

console.log("초기 person:", person);
```

```js
// ---------------------------------------------------------
// 2. 프로퍼티 접근 (점 표기법, 대괄호 표기법)
let name = person.name;
let age = person["age"];

let property = "hobby";
let hobby = person[property];

console.log("\n[접근]");
console.log("name:", name);
console.log("age:", age);
console.log("hobby:", hobby);
```

```js
// ---------------------------------------------------------
// 3. 프로퍼티 추가
person.job = "backend developer";
person["favoriteFood"] = "pizza";

console.log("\n[추가]");
console.log(person);
```

```js
// ---------------------------------------------------------
// 4. 프로퍼티 수정
person.job = "designer";
person["favoriteFood"] = "sushi";

console.log("\n[수정]");
console.log(person);
```

```js
// ---------------------------------------------------------
// 5. 프로퍼티 삭제
delete person.job;
delete person["favoriteFood"];

console.log("\n[삭제]");
console.log(person);

// ---------------------------------------------------------
// 6. 프로퍼티 존재 확인 (in 연산자)
let result1 = "name" in person;
let result2 = "dog" in person;

console.log("\n[존재 확인 - in]");
console.log('"name" in person ->', result1);
console.log('"dog" in person  ->', result2);
```

```js
// ---------------------------------------------------------
// 7. const 객체와 프로퍼티 조작
const animal = {
  type: "고양이",
  name: "나비",
  color: "black",
};

console.log("\n[const 객체 초기값]");
console.log(animal);

animal.age = 2; // 추가
animal.name = "까망이"; // 수정
delete animal.color; // 삭제

console.log("\n[const 객체 조작 후]");
console.log(animal);
```

```js
// ---------------------------------------------------------
// 8. 객체에 메서드 정의
const user = {
  name: "이정환",
  sayHi() {
    console.log("안녕!");
  },
};

console.log("\n[메서드 실행]");
user.sayHi();
user["sayHi"]();
```
