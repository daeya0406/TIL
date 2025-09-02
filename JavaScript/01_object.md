# 객체(Object)

## 1. 객체 선언

```js
let person = {
  name: "Alice",
  age: 30,
  hobby: "cycling",
};

console.log("초기 person:", person);
```

## 2. 프로퍼티 접근 (점 표기법, 대괄호 표기법)

```js
let name = person.name;
let age = person["age"];

let property = "hobby";
let hobby = person[property];

console.log("\n[접근]");
console.log("name:", name);
console.log("age:", age);
console.log("hobby:", hobby);
```

## 3. 프로퍼티 추가

```js
person.job = "backend developer";
person["favoriteFood"] = "pizza";

console.log("\n[추가]");
console.log(person);
```

## 4. 프로퍼티 수정

```js
person.job = "designer";
person["favoriteFood"] = "sushi";

console.log("\n[수정]");
console.log(person);
```

## 5. 프로퍼티 삭제

```js
delete person.job;
delete person["favoriteFood"];

console.log("\n[삭제]");
console.log(person);
```

## 6. 프로퍼티 존재 확인 (in 연산자)

```js
let result1 = "name" in person;
let result2 = "dog" in person;

console.log("\n[존재 확인 - in]");
console.log('"name" in person ->', result1);
console.log('"dog" in person  ->', result2);
```

## 7. const 객체와 프로퍼티 조작

```js
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

## 8. 객체에 메서드 정의

```js
const user = {
  name: "김정대",
  sayHi() {
    console.log("안녕!");
  },
};

console.log("\n[메서드 실행]");
user.sayHi();
user["sayHi"]();
```
