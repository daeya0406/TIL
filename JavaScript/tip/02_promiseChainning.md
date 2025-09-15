# Promise 체이닝 & return

---

## 1. 언제 쓰는지 / 왜 쓰는지

- 여러 비동기 작업을 순서대로 실행해야 할 때 사용
- `return`으로 값을 넘기면 다음 `.then()`으로 깔끔하게 이어짐
- 콜백 지옥처럼 중첩되지 않고 **일렬 체인 구조**로 관리 가능

---

## 2. 기본 사용법

```js
fetchUser()
  .then((user) => {
    return fetchPosts(user.id); // return 필수
  })
  .then((posts) => {
    console.log(posts); // 위 return 값이 넘어옴
  });
```

---

## 3. 확장된 사용법

- `return 값`이 일반 값이면 → Promise로 감싸져서 넘어감
- `return 값`이 Promise면 → resolve될 때까지 기다린 후 넘어감

```js
// 일반 값
Promise.resolve(1)
  .then((n) => n + 1) // return 안 쓰면 undefined
  .then((n) => console.log(n)); // 2

// Promise 반환
fetchUser()
  .then((user) => fetchPosts(user.id)) // Promise 반환
  .then((posts) => console.log(posts));
```

---

## 4. 체크리스트

- [ ] 다음 `.then()`에 넘겨야 한다면 꼭 `return` 했는가?
- [ ] 단순히 로그만 찍고 끝내려면 `return` 필요 없음
- [ ] 에러 처리는 마지막 `.catch()` 하나로 모았는가?

---

## 5. 트러블슈팅

- return 안 하면 → 다음 then에 `undefined` 넘어옴
- 체인 중간에서 에러 발생하면 → 아래 catch에서 한 번에 처리 가능

---
