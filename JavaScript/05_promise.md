# Promise 정리

---

## 1. 언제 쓰는지 / 왜 쓰는지

- JS는 싱글스레드라 네트워크 요청, 파일 읽기 같은 **시간이 오래 걸리는 작업을 기다리지 않고** 진행하려고 비동기 방식 사용
- 콜백(callback)만 쓰면 코드가 깊어져서 관리하기 힘듦 → 이를 해결하기 위해 **비동기 결과를 표현하는 객체**가 Promise
- 즉, "지금은 값이 없지만 미래에 생길 값"을 다룰 때 씀

---

## 2. 기본 사용법

```js
// Promise 생성
const myPromise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("성공!");
  } else {
    reject("실패!");
  }
});

// Promise 사용
myPromise
  .then((result) => {
    console.log(result); // 성공 시
  })
  .catch((error) => {
    console.error(error); // 실패 시
  });
```

---

## 3. 확장된 사용법

- **체이닝**: then을 이어 붙여 순차 실행
- **Promise.all**: 여러 비동기 작업을 동시에 실행 후 모두 끝나면 결과 반환
- **async/await**: Promise를 더 직관적으로 사용 가능

```js
// 체이닝
fetch("/api/user")
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// Promise.all
Promise.all([fetch("/user"), fetch("/posts")])
  .then(([userRes, postsRes]) => Promise.all([userRes.json(), postsRes.json()]))
  .then(([user, posts]) => {
    console.log(user, posts);
  });

// async/await
async function getData() {
  try {
    const res = await fetch("/api/user");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

---

## 4. 체크리스트

- [ ] 시간이 걸리는 작업 결과를 기다려야 하는가? (네트워크, 파일, 타이머 등)
- [ ] 콜백 대신 then/catch, async/await을 사용했는가?
- [ ] 여러 비동기 작업이 동시에 필요하다면 Promise.all을 고려했는가?

---

## 5. 트러블슈팅

- "Promise {<pending>}"만 보임 → 아직 비동기 작업이 끝나지 않은 상태. await 또는 then으로 결과를 받아야 함
- catch 안 쓰면 에러 누락됨 → 반드시 예외 처리 필요
- async 함수 안에서만 await 사용 가능

---
