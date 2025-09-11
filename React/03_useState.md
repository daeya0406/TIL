# useState 뽀개기

## 목차

- [1. 초깃값 지정하기](#1-초깃값-지정하기)
- [2. 콜백으로 초깃값 지정하기](#2-콜백으로-초깃값-지정하기)
- [3. Setter 함수 기본 사용](#3-setter-함수-기본-사용)
- [4. 참조형 State 다루기](#4-참조형-state-다루기)
- [5. 콜백으로 State 변경하기](#5-콜백으로-state-변경하기)

---

## 1. 초깃값 지정하기

`useState` 함수에 값을 전달하면 해당 값이 초깃값으로 지정됨.

```js
const [state, setState] = useState(initialState);
```

---

## 2. 콜백으로 초깃값 지정하기

`useState`에 콜백을 전달하면, 초깃값을 **처음 렌더링할 때 한 번만 계산**해서 세팅함.

```js
function ReviewForm() {
  const [values, setValues] = useState(() => {
    const savedValues = getSavedValues(); // 처음 렌더링 시에만 실행
    return savedValues;
  });
}
```

- 일반 방식: 렌더링마다 `getSavedValues()` 실행 → 불필요한 연산 발생
- 콜백 방식: **처음 렌더링 한 번만 실행** → 성능 최적화
- 주의: 콜백 실행이 오래 걸리면 초기 렌더링이 늦어질 수 있음

---

## 3. Setter 함수 기본 사용

`setState`에 값을 전달하면 새로운 state로 교체됨.

```js
const [count, setCount] = useState(0);

const handleAddClick = () => {
  setCount(count + 1);
};
```

---

## 4. 참조형 State 다루기

배열, 객체 같은 참조형을 다룰 때는 **무조건 새로운 값(복사본)을 만들어야 함**.

❌ 잘못된 예시 (원본 직접 수정):

```js
const [state, setState] = useState({ count: 0 });

const handleAddClick = () => {
  state.count += 1; // 원본 수정
  setState(state); // 참조는 그대로라 React가 변경 감지 못함
};
```

✅ 올바른 예시 (새 객체 생성):

```js
const [state, setState] = useState({ count: 0 });

const handleAddClick = () => {
  setState({ ...state, count: state.count + 1 }); // 새로운 객체로 교체
};
```

---

## 5. 콜백으로 State 변경하기

이전 state 값을 기반으로 다음 state를 만들 때는 콜백 형태를 써야 안정적임.  
비동기 코드에서 최신 state 보장을 위해 꼭 콜백 패턴을 쓰는 습관을 들이는 게 좋음.

```js
const [count, setCount] = useState(0);

const handleAddClick = async () => {
  await addCount(); // 비동기 동작
  setCount((prevCount) => prevCount + 1); // 항상 최신 값 보장
};
```

---
