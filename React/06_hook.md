# 🪝 React Hook & 커스텀 훅 정리

---

## 1. 언제 쓰는지 / 왜 쓰는지

- React에서 함수형 컴포넌트만 사용하면서, **상태 관리 / 사이드 이펙트 / Context** 같은 기능을 함수 안에서 다룰 필요가 생김
- Hook은 이런 기능을 제공하는 **특별한 함수**
- **커스텀 훅(Custom Hook)** 은 여러 컴포넌트에서 반복되는 로직을 함수로 뽑아 **재사용**하기 위함

---

## 2. 기본 내장 Hook

### 2-1. useState

상태(state) 관리

```js
const [count, setCount] = useState(0);
```

### 2-2. useEffect

렌더링 이후 실행해야 하는 코드

```js
useEffect(() => {
  console.log("렌더링 이후 실행");
  return () => console.log("정리(clean-up)");
}, []);
```

### 2-3. useRef

렌더링 없이 값 저장 / DOM 접근

```js
const inputRef = useRef(null);
<input ref={inputRef} />;
```

### 2-4. useContext

Context 값 접근

```js
const theme = useContext(ThemeContext);
```

---

## 3. 확장된 Hook

- **useReducer** → 복잡한 상태 관리
- **useMemo** → 연산 결과 캐싱
- **useCallback** → 함수 캐싱
- **useLayoutEffect** → DOM 업데이트 직후 실행

---

## 4. 커스텀 훅 (Custom Hook)

### 4-1. 언제 쓰는지

- 여러 컴포넌트에서 **같은 로직을 중복해서 쓰고 있다면**
- 공통 로직을 하나의 함수로 추출 → 재사용, 코드 간결화

---

### 4-2. 기본 사용법

- 이름은 반드시 `use`로 시작 (`useSomething`)
- 내부에서 다른 Hook들을 자유롭게 사용 가능
- **상태/이펙트 묶음 → 반환값으로 제공**

```js
// ✅ 커스텀 훅 정의
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return width;
}

// ✅ 사용
function MyComponent() {
  const width = useWindowWidth();
  return <p>현재 창 크기: {width}px</p>;
}
```

---

### 4-3. 여러 값 반환하기

객체나 배열로 여러 값을 반환 가능

```js
function useCounter(initial = 0) {
  const [count, setCount] = useState(initial);
  const increase = () => setCount((c) => c + 1);
  const decrease = () => setCount((c) => c - 1);
  const reset = () => setCount(initial);

  return { count, increase, decrease, reset };
}

function Counter() {
  const { count, increase, decrease, reset } = useCounter(5);
  return (
    <div>
      <p>{count}</p>
      <button onClick={increase}>+1</button>
      <button onClick={decrease}>-1</button>
      <button onClick={reset}>reset</button>
    </div>
  );
}
```

---

## 5. 체크리스트

- [ ] Hook은 항상 함수 컴포넌트나 커스텀 훅 최상위에서만 호출했는가?
- [ ] 반복되는 로직이 있다면 커스텀 훅으로 뽑아냈는가?
- [ ] 커스텀 훅 이름을 `use`로 시작했는가?
- [ ] 커스텀 훅은 **상태 + 이펙트 + 헬퍼 함수**를 묶어 반환하도록 구성했는가?

---

## 6. 트러블슈팅

- “Hook은 최상위에서만 호출해야 한다” → 조건문/반복문 안에서 호출했을 가능성
- “커스텀 훅 이름 규칙 에러” → 반드시 `use`로 시작해야 함
- “state가 컴포넌트별로 공유돼요” → 커스텀 훅은 **함수라서 실행 시마다 독립적인 state 생성**됨. 공유되지 않음

---
