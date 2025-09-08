# 리액트 배열 렌더링 정리

## 목차

- [1. map으로 렌더링하기](#1-map으로-렌더링하기)
- [2. map 결과를 변수로 렌더링](#2-map-결과를-변수로-렌더링)
- [3. sort로 정렬하기](#3-sort로-정렬하기)
- [4. filter로 삭제하기](#4-filter로-삭제하기)
- [5. key 꼭 지정하기](#5-key-꼭-지정하기)

---

## 1. map으로 렌더링하기

배열의 각 요소를 **map** 메소드로 변환해 JSX 엘리먼트를 리턴한다.  
리스트에서는 **key prop**을 반드시 지정해야 한다.

**포맷**

```js
{
  배열.map((item) => <JSX key={고유값}>{/* 렌더링할 내용 */}</JSX>);
}
```

**예시**

```js
import items from "./pokemons";

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>
          <Pokemon item={item} />
        </li>
      ))}
    </ul>
  );
}
```

---

## 2. map 결과를 변수로 렌더링

map 결과를 **변수에 담은 뒤 JSX에서 사용**할 수 있다.

**포맷**

```js
const 변수 = 배열.map((item) => <JSX key={고유값} />);

return <ul>{변수}</ul>;
```

**예시**

```js
import items from "./pokemons";

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  const renderedItems = items.map((item) => (
    <li key={item.id}>
      <Pokemon item={item} />
    </li>
  ));

  return <ul>{renderedItems}</ul>;
}
```

---

## 3. sort로 정렬하기

**sort** 메소드로 배열을 정렬한 뒤 map으로 렌더링한다.  
state로 정렬 방향을 관리하면 동적 정렬 가능하다.

**포맷**

```js
const [direction, setDirection] = useState(1);

const sorted = [...배열].sort((a, b) => direction * (a.값 - b.값));

return (
  <ul>
    {sorted.map((item) => (
      <JSX key={item.id} />
    ))}
  </ul>
);
```

**예시**

```js
import { useState } from "react";
import items from "./pokemons";

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  const [direction, setDirection] = useState(1);

  const handleAscClick = () => setDirection(1);
  const handleDescClick = () => setDirection(-1);

  const sortedItems = [...items].sort((a, b) => direction * (a.id - b.id));

  return (
    <div>
      <div>
        <button onClick={handleAscClick}>도감번호 순서대로</button>
        <button onClick={handleDescClick}>도감번호 반대로</button>
      </div>
      <ul>
        {sortedItems.map((item) => (
          <li key={item.id}>
            <Pokemon item={item} />
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 4. filter로 삭제하기

**filter** 메소드로 조건을 만족하지 않는 요소를 제거하고 state를 갱신한다.

**포맷**

```js
const [items, setItems] = useState(초기값);

const handleDelete = (id) => {
  setItems(items.filter((item) => item.id !== id));
};

return (
  <ul>
    {items.map((item) => (
      <JSX key={item.id} onDelete={() => handleDelete(item.id)} />
    ))}
  </ul>
);
```

**예시**

```js
import { useState } from "react";
import mockItems from "./pokemons";

function Pokemon({ item, onDelete }) {
  const handleDeleteClick = () => onDelete(item.id);

  return (
    <div>
      No.{item.id} {item.name}
      <button onClick={handleDeleteClick}>삭제</button>
    </div>
  );
}

function App() {
  const [items, setItems] = useState(mockItems);

  const handleDelete = (id) => {
    const nextItems = items.filter((item) => item.id !== id);
    setItems(nextItems);
  };

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>
          <Pokemon item={item} onDelete={handleDelete} />
        </li>
      ))}
    </ul>
  );
}
```

---

## 5. key 꼭 지정하기

리스트 렌더링 시 **고유한 key**를 각 최상위 요소에 지정해야 한다.  
id, name 등 **고유 식별값**이면 무엇이든 가능하다.

**포맷**

```js
{
  배열.map((item) => <JSX key={고유값} />);
}
```

**예시**

```js
import items from "./pokemons";

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.name}>
          <Pokemon item={item} />
        </li>
      ))}
    </ul>
  );
}
```

---
