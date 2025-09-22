# useEffect로 라이프사이클 제어하기

---

## 1. 언제 쓰는지 / 왜 쓰는지

- 리액트 함수형 컴포넌트에는 클래스 컴포넌트의 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount` 같은 라이프사이클 메서드가 없음
- 대신 `useEffect`를 사용해서 컴포넌트의 **탄생(마운트), 변화(업데이트), 죽음(언마운트)** 시점에 코드를 실행할 수 있음
- API 호출, 이벤트 리스너 등록/해제, 타이머 설정/정리 등 “컴포넌트 생명주기와 맞물린 작업”에 필요함

---

## 2. 기본 사용법

### 마운트 (처음 나타날 때 한 번 실행)

```js
useEffect(() => {
  console.log("mount");
}, []); // 의존성 배열이 빈 배열이면 딱 한 번만
```

### 업데이트 (상태/props 바뀔 때 실행)

```js
useEffect(() => {
  console.log("update: count 변경됨");
}, [count]); // count가 변할 때마다 실행
```

### 언마운트 (사라질 때 실행)

```js
useEffect(() => {
  return () => {
    console.log("unmount");
  };
}, []); // cleanup 함수가 언마운트 시 실행
```

---

## 3. 확장된 사용법

### 업데이트만 실행 (마운트는 제외)

```js
const isMount = useRef(true);

useEffect(() => {
  if (isMount.current) {
    isMount.current = false; // 첫 실행은 스킵
    return;
  }
  console.log("update only");
}, [value]);
```

### 외부 리소스 관리 (예: 이벤트 리스너)

```js
useEffect(() => {
  window.addEventListener("resize", handleResize);

  return () => {
    // 언마운트 시 정리(cleanup)
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

---

## 4. 체크리스트

- [ ] 한 번만 실행해야 한다면 의존성 배열은 `[]`로 두었는가?
- [ ] 특정 값 변경 시 실행해야 한다면 의존성 배열에 그 값을 넣었는가?
- [ ] 언마운트 시 정리해야 하는 리소스(이벤트, 타이머)가 있다면 cleanup 함수를 작성했는가?
- [ ] 마운트 시 실행을 제외하고 싶다면 `isMount` 같은 ref로 제어했는가?

---

## 5. 트러블슈팅

- “계속 실행돼요” → 의존성 배열이 빠졌거나 잘못 작성했을 가능성
- “값이 최신이 아니에요” → 의존성 배열에 필요한 변수를 빠뜨렸을 수 있음
- “언마운트 때 정리가 안 돼요” → cleanup 함수(`return () => { ... }`)를 넣었는지 확인
- “업데이트만 실행하고 싶은데 마운트 때도 실행돼요” → `isMount` ref로 첫 실행 스킵 필요

---
