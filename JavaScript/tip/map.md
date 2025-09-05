# map 함수 JSX 템플릿

React에서 배열 데이터를 JSX로 변환할 때 사용하는 map 템플릿이다.
**"map 어떻게 시작하지?"** 싶을 때 이 구조로 시작하려고 정리해보았다.

---

## 기본 템플릿 (JSX 리스트 렌더링)

```js
const result = items.map((item, index) => {
  return <div key={index}>{item}</div>;
});
```

- `item`: 현재 요소
- `index`: 인덱스 (React에서는 key로 자주 사용됨)
- `key={index}`는 필수는 아니지만, React에서 리스트를 렌더링할 때 경고 방지용으로 추천

---

## 예시 1: 문자열 배열을 `<li>`로 만들기

```js
const fruits = ["apple", "banana", "cherry"];

const fruitList = fruits.map((fruit, index) => {
  return <li key={index}>{fruit}</li>;
});
```

---

## 예시 2: 객체 배열을 카드로 렌더링

```js
const users = [
  { id: 1, name: "Jane" },
  { id: 2, name: "DaeYa" },
];

const userCards = users.map((user) => {
  return (
    <div key={user.id} className="card">
      <p>{user.name}</p>
    </div>
  );
});
```

---

## 예시 3: 조건부 렌더링과 map 조합

```js
const scores = [80, 55, 92];

const passed = scores.map((score, i) => {
  return score >= 60 ? <li key={i}>Pass: {score}</li> : null;
});
```

---

## 참고 팁

- map을 쓸 때는 항상 **“무엇으로 바꿀지”** 를 먼저 생각하고 시작
- `return` 안에 바꾸고 싶은 구조를 작성
- React에선 `key` 속성 잊지 말기!

---
