# 자바스크립트 비동기 처리 발전사

---

## 1. 콜백 (Callback)

### 언제/왜

- 오래 걸리는 작업(네트워크, 파일 등) 끝난 후 실행할 코드를 함수로 넘겨줌
- 초기에는 단순한 비동기 흐름 제어 방식으로 사용됨

### 예시

```js
function getUser(id, callback) {
  setTimeout(() => {
    callback({ id, name: "Alice" });
  }, 1000);
}

getUser(1, (user) => {
  console.log(user); // {id: 1, name: "Alice"}
});
```

---

## 2. 콜백 헬 (Callback Hell)

### 언제/왜

- 여러 비동기 작업을 순차적으로 이어가려다보면 함수 안에 함수가 계속 중첩됨
- 가독성 ↓, 에러 처리 어렵고 유지보수 힘듦

### 예시

```js
login("id", "pw", (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      console.log(comments);
    });
  });
});
```

---

## 3. Promise (`.then` 체이닝)

### 언제/왜

- 콜백 지옥 문제를 해결하기 위해 ES6에서 도입
- "미래에 생길 값"을 표현하는 객체
- `then/catch` 체이닝으로 흐름을 일렬로 표현 가능

### 예시

```js
login("id", "pw")
  .then((user) => getPosts(user.id))
  .then((posts) => getComments(posts[0].id))
  .then((comments) => console.log(comments))
  .catch((err) => console.error(err));
```

---

## 4. async/await

### 언제/왜

- Promise 체이닝도 길어지면 여전히 복잡
- `async/await`은 동기 코드처럼 작성 가능 → 가독성 대폭 상승
- 에러 처리도 try/catch로 깔끔하게

### 예시

```js
async function load() {
  try {
    const user = await login("id", "pw");
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    console.log(comments);
  } catch (err) {
    console.error(err);
  }
}
```

---

## 5. 체크리스트

- [ ] 콜백이 깊게 중첩되고 있진 않은가?
- [ ] 단순 순차 실행이면 Promise 체이닝으로 풀 수 있는가?
- [ ] 더 직관적 가독성이 필요하면 async/await으로 바꿨는가?
- [ ] 에러 처리는 catch 또는 try/catch로 모았는가?

---

## 6. 트러블슈팅

- 콜백: 에러가 깊은 곳에서 발생하면 상위로 전달하기 힘듦
- Promise: return 안 하면 다음 then으로 값이 안 넘어감
- async/await: 반드시 async 함수 안에서만 await 사용 가능

---
