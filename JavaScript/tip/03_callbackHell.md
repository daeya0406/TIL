# 콜백 지옥 vs Promise

---

## 1. 언제 쓰는지 / 왜 쓰는지

- **콜백만 사용**하면 코드가 중첩되어 가독성이 떨어지고 에러 처리 힘듦
- Promise는 콜백 지옥을 줄이고 **순차적 로직을 일렬 체인**으로 표현할 수 있게 해줌

---

## 2. 기본 사용법

```js
// 콜백 지옥
login("id", "pw", (user) => {
  getPosts(user, (posts) => {
    getComments(posts[0], (comments) => {
      console.log(comments);
    });
  });
});

// Promise 체이닝
login("id", "pw")
  .then((user) => getPosts(user))
  .then((posts) => getComments(posts[0]))
  .then((comments) => console.log(comments))
  .catch((err) => console.error(err));
```

---

## 3. 확장된 사용법

- 여러 비동기 작업을 동시에 처리할 땐 `Promise.all`
- 병렬 실행 후 결과를 모아서 사용 가능

```js
Promise.all([fetchUser(), fetchPosts()]).then(([user, posts]) => {
  console.log(user, posts);
});
```

---

## 4. 체크리스트

- [ ] 비동기 흐름이 중첩되어 복잡해지고 있진 않은가?
- [ ] 순차 실행 가능하다면 Promise 체이닝으로 바꿨는가?
- [ ] 동시에 실행 가능한 건 Promise.all을 고려했는가?

---

## 5. 트러블슈팅

- 콜백에서 에러 발생 → 위로 전파 어렵고 중첩 콜백마다 처리해야 함
- Promise는 체인 어디서든 에러 발생 시 마지막 catch에서 한 번에 처리 가능

---
