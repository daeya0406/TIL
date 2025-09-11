# 📌 useRef로 DOM 노드 참조하기 정리

리액트에서 `useRef`를 이용해 DOM 노드에 직접 접근하는 방법 정리.

---

## 목차

- [1. Ref 객체 생성](#1-ref-객체-생성)
- [2. ref Prop 사용하기](#2-ref-prop-사용하기)
- [3. DOM 노드 참조하기](#3-dom-노드-참조하기)
- [4. 예시: 이미지 크기 구하기](#4-예시-이미지-크기-구하기)
- [5. 체크리스트](#5-체크리스트)

---

## 1. Ref 객체 생성

```js
import { useRef } from "react";

const ref = useRef();
```

- `useRef()` → `{ current: null }` 같은 **Ref 객체**를 반환.
- `current` 프로퍼티에 DOM 노드가 연결된다.

---

## 2. ref Prop 사용하기

```js
const ref = useRef();

<div ref={ref}>내용</div>;
```

- JSX에서 `ref={ref}` 지정하면 → 해당 DOM 노드가 `ref.current`에 들어감.

---

## 3. DOM 노드 참조하기

```js
const node = ref.current;
if (node) {
  // node를 안전하게 사용 가능
}
```

- 반드시 **존재 여부 검사** 후 사용.
- 예: `node.value`, `node.scrollTop`, `node.width` 등 직접 접근 가능.

---

## 4. 예시: 이미지 크기 구하기

```js
import { useRef } from "react";

function Image({ src }) {
  const imgRef = useRef();

  const handleSizeClick = () => {
    const imgNode = imgRef.current;
    if (!imgNode) return;

    const { width, height } = imgNode;
    console.log(`${width} x ${height}`);
  };

  return (
    <div>
      <img src={src} ref={imgRef} alt="크기를 구할 이미지" />
      <button onClick={handleSizeClick}>크기 구하기</button>
    </div>
  );
}
```

- 버튼 클릭 → `imgRef.current`로 `<img>` DOM 참조.
- `width`, `height` 속성 값 가져와 출력.

---

## 5. 체크리스트

- [ ] `useRef`로 Ref 객체 생성했나?
- [ ] JSX에서 `ref={ref}`를 지정했나?
- [ ] `ref.current` 접근 시 값이 있는지 확인했나? (`if (!ref.current) return;`)
- [ ] 꼭 필요한 경우에만 DOM 직접 접근했나? (리액트의 선언적 흐름 우선)

---
