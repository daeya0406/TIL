# 콜백 함수(Callback) 정리 – JavaScript/React

콜백 함수는 **다른 함수에 인자로 전달되어, 특정 시점에 호출되는 함수**. 동기/비동기 흐름 제어, 이벤트 처리, 고차함수(map/filter), React props 등에서 핵심적으로 사용됨.

---

## 1. 콜백의 정의와 형태

설명: 함수(또는 함수 참조)를 인자로 넘기고, **호출 책임은 받은 쪽**이 갖는다.

```js
function doSomething(task, onDone) {
  const result = task(); // 작업 실행
  onDone(result); // 완료 후 콜백 호출
}

doSomething(
  () => "작업 결과",
  (res) => console.log("완료:", res)
);
```

---

## 2. 동기 콜백 vs 비동기 콜백

설명: **동기**는 즉시 실행, **비동기**는 나중에(이벤트 루프 이후) 실행.

```js
// 동기 콜백: Array.prototype.map
const doubled = [1, 2, 3].map((n) => n * 2); // 즉시 실행

// 비동기 콜백: setTimeout
setTimeout(() => {
  console.log("1초 후 실행");
}, 1000);
```

---

## 3. 에러 우선 콜백 패턴(Node 스타일)

설명: `(err, data)` 시그니처로 에러를 첫 번째 인자로 전달.

```js
function readData(cb) {
  try {
    const data = "OK";
    cb(null, data); // 성공: err = null
  } catch (e) {
    cb(e, null); // 실패: err = Error
  }
}

readData((err, data) => {
  if (err) return console.error("에러:", err);
  console.log("데이터:", data);
});
```

---

## 4. 콜백 지옥(Callback Hell)과 해결

설명: 콜백 중첩은 가독성과 에러 처리에 불리. → **Promise/async-await**로 리팩터링.

```js
// 콜백 지옥
step1((e, r1) => {
  if (e) return handle(e);
  step2(r1, (e, r2) => {
    if (e) return handle(e);
    step3(r2, (e, r3) => {
      if (e) return handle(e);
      console.log("완료", r3);
    });
  });
});

// Promise 버전(개념 예시)
step1P()
  .then(step2P)
  .then(step3P)
  .then((r3) => console.log("완료", r3))
  .catch(handle);

// async/await
(async () => {
  try {
    const r1 = await step1P();
    const r2 = await step2P(r1);
    const r3 = await step3P(r2);
    console.log("완료", r3);
  } catch (e) {
    handle(e);
  }
})();
```

---

## 5. 고차함수에서의 콜백(map/filter/reduce)

설명: 배열 메서드는 콜백으로 **변환/선택/누적** 로직을 주입받는다.

```js
const nums = [1, 2, 3, 4, 5];

const squared = nums.map((n) => n * 2); // 변환
const even = nums.filter((n) => n % 2 === 0); // 선택
const sum = nums.reduce((acc, n) => acc + n, 0); // 누적

console.log(squared, even, sum);
```

---

## 6. this 바인딩 주의(일반 함수 vs 화살표 함수)

설명: **화살표 함수는 자신만의 this를 갖지 않고** 상위 스코프의 this를 캡처.

```js
const obj = {
  value: 42,
  a() {
    setTimeout(function () {
      console.log(this.value);
    }, 0);
  }, // this: window/undefined
  b() {
    setTimeout(() => {
      console.log(this.value);
    }, 0);
  }, // this: obj
};
obj.a(); // undefined (또는 에러)
obj.b(); // 42
```

---

## 7. 콜백의 계약(시그니처) 설계

설명: 호출측/피호출측 사이 **인자 순서, 동기/비동기, 에러 처리 방식**을 명확히.

```js
// 계약: (value) => void
function forEach(arr, cb) {
  for (let i = 0; i < arr.length; i += 1) cb(arr[i]);
}

forEach(["a", "b"], (v) => console.log(v)); // a, b
```

---

## 8. 이벤트 기반 콜백(DOM/브라우저)

설명: 이벤트 발생 시점에 콜백이 실행.

```js
document.getElementById("btn").addEventListener("click", (e) => {
  e.preventDefault();
  console.log("클릭!", e.target.id);
});
```

---

## 9. React에서의 콜백

설명: **부모→자식으로 함수 props 전달**, 상태 변경 시 **업데이터 함수 콜백** 활용.

```js
// 부모
function Parent() {
  const [count, setCount] = React.useState(0);

  const handlePlus = () => setCount((prev) => prev + 1); // 업데이터 콜백

  return <Child onPlus={handlePlus} count={count} />;
}

// 자식
function Child({ onPlus, count }) {
  return <button onClick={onPlus}>{count} 증가</button>;
}
```

---

## 10. 성능 팁(불필요한 재생성 방지)

설명: React에서 콜백을 **useCallback**으로 메모이즈해 불필요한 리렌더링 감소.

```js
function List({ items, onSelect }) {
  return items.map((it) => (
    <button key={it.id} onClick={() => onSelect(it.id)}>
      {it.label}
    </button>
  ));
}

function Page() {
  const [selected, setSelected] = React.useState(null);

  const handleSelect = React.useCallback((id) => {
    setSelected(id);
  }, []);

  return <List items={[{ id: 1, label: "A" }]} onSelect={handleSelect} />;
}
```

---

## 체크리스트

- [ ] 콜백이 **언제/누가 호출**하는지 명확한가?
- [ ] **시그니처(인자, 반환, 에러 처리)** 계약이 문서화됐는가?
- [ ] 비동기 콜백에서 **에러 처리(try/catch 불가)** 전략을 갖췄는가?
- [ ] 중첩 콜백은 **Promise/async-await**로 평탄화했는가?
- [ ] React에서 콜백 props가 **불필요하게 재생성**되지 않도록 관리(useCallback 등)했는가?
- [ ] `this` 문맥이 중요한 경우 **화살표 함수/바인딩** 차이를 이해했는가?

---

## 트러블슈팅

- “에러가 사라짐/전파 안 됨” → 콜백 내부 예외는 호출 스택을 벗어나므로, **에러 우선 패턴** 또는 **Promise**로 옮겨 처리.
- “this가 undefined” → 콜백에서 **화살표 함수** 사용 또는 `fn.bind(this)`로 바인딩.
- “콜백이 여러 번 실행” → 등록 해제(`removeEventListener`) 또는 **정상/중복 호출 가드** 추가.
- “React에서 핸들러 때문에 리렌더링 반복” → `useCallback`, `useMemo`, **의존성 배열** 점검.
