# async/await

---

## 1. 언제 쓰는지 / 왜 쓰는지

- Promise 체이닝도 길어지면 여전히 복잡
- `async/await`은 동기 코드처럼 직관적으로 작성 가능
- try/catch로 에러를 일반적인 방식으로 다룰 수 있음

---

## 2. 기본 사용법

```js
async function getUserData() {
  try {
    const res = await fetch("/api/user");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error("에러:", err);
  }
}

getUserData();
```

---

## 3. 확장된 사용법

- 여러 요청을 병렬로 처리하려면 `Promise.all`과 함께 사용
- 루프 안에서 비동기 처리도 깔끔하게 가능

```js
// 병렬
async function loadData() {
  const [user, posts] = await Promise.all([
    fetch("/user").then((r) => r.json()),
    fetch("/posts").then((r) => r.json()),
  ]);
  console.log(user, posts);
}

// 반복
async function loadUsers(ids) {
  for (const id of ids) {
    const res = await fetch(`/user/${id}`);
    console.log(await res.json());
  }
}
```

---

## 4. 체크리스트

- [ ] 함수에 async를 붙였는가?
- [ ] Promise 결과를 쓰려면 await을 붙였는가?
- [ ] 에러는 try/catch로 처리했는가?
- [ ] 동시에 실행 가능한 건 Promise.all과 함께 썼는가?

---

## 5. 트러블슈팅

- await은 async 함수 안에서만 가능
- await을 루프 안에 잘못 쓰면 병렬 대신 순차 실행돼 성능 저하 가능
- async 함수는 항상 Promise를 반환함 (return 값 주의)

---
