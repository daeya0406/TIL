# 리액트 배열 렌더링 정리

## 목차

- [1. map으로 렌더링하기](#1-map으로-렌더링하기)
- [2. map 결과를 변수로 렌더링](#2-map-결과를-변수로-렌더링)
- [3. sort로 정렬하기](#3-sort로-정렬하기)
- [4. filter로 삭제하기](#4-filter로-삭제하기)
- [5. key 꼭 지정하기](#5-key-꼭-지정하기)

---

## 1. map으로 렌더링하기

배열의 각 요소를 **map** 메소드로 변환해 JSX 엘리먼트를 리턴하면 된다. key prop을 반드시 지정해야 한다.

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

export default App;
```

---

## 2. map 결과를 변수로 렌더링

map 결과를 **변수에 담아도 JSX에서 동일하게 렌더링**된다.

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

export default App;
```

---

## 3. sort로 정렬하기

**sort** 메소드를 사용하면 배열을 정렬한 뒤 map으로 렌더링할 수 있다. state로 정렬 방향을 관리하면 동적으로 제어 가능하다.

```js
import { useState } from 'react';
import items from './pokemons';

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

const sortedItems = [...items].sort((a, b) => direction \* (a.id - b.id));

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

export default App;
```

---

## 4. filter로 삭제하기

**filter** 메소드를 사용하면 특정 조건을 만족하지 않는 요소를 제거할 수 있다. 이 원리를 활용해 삭제 기능을 구현한다.

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

export default App;
```

---

## 5. key 꼭 지정하기

리스트 렌더링 시 **고유한 key**를 각 최상위 요소에 지정해야 한다. id, name 등 고유한 값이면 무엇이든 가능하다.

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

export default App;
```

---
