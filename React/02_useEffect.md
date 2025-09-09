(이 줄은 복사 후 삭제)

# useEffect 정리하기

## 목차

- [1. 처음 한 번만 실행하기](#1-처음-한-번만-실행하기)
- [2. 값이-바뀔-때마다-실행하기](#2-값이-바뀔-때마다-실행하기)
- [3. 실험으로-확인하기](#3-실험으로-확인하기)

---

## 1. 처음 한 번만 실행하기

컴포넌트가 처음 렌더링 되고 나면 리액트가 콜백 함수를 기억해뒀다가 실행한다.  
그 이후로는 콜백을 실행하지 않는다.

```js
useEffect(() => {
  // 실행할 코드
}, []);
```

---

## 2. 값이 바뀔 때마다 실행하기

컴포넌트가 처음 렌더링 되고 나면 콜백을 한 번 실행한다.  
그 이후 렌더링 때마다 **디펜던시 리스트의 값**을 비교해, 하나라도 바뀌면 콜백을 다시 실행한다.

```js
useEffect(() => {
  // 실행할 코드
}, [dep1, dep2, dep3]);
```

---

## 3. 실험으로 확인하기

아래 코드는 `useEffect` 동작을 간단히 실험할 수 있는 예제다.  
디펜던시 배열을 `[]`, `[first]`, `[first, second]`로 바꿔보면서 콘솔 로그가 어떻게 달라지는지 확인해본다.

```js
import { useEffect, useState } from "react";

function App() {
  const [first, setFirst] = useState(1);
  const [second, setSecond] = useState(1);

  const handleFirstClick = () => setFirst(first + 1);
  const handleSecondClick = () => setSecond(second + 1);

  useEffect(() => {
    console.log("렌더링 이후 실행:", first, second);
  }, []); // [] → 처음 한 번만 실행

  // [first] 또는 [first, second]로 바꿔보면서 로그 확인해보기

  console.log("렌더링:", first, second);

  return (
    <div>
      <h1>
        {first}, {second}
      </h1>
      <button onClick={handleFirstClick}>First</button>
      <button onClick={handleSecondClick}>Second</button>
    </div>
  );
}

export default App;
```

---

(이 줄도 복사 후 삭제)
