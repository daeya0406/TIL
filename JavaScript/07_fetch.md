# 🌐 fetch() 정리

---

## 1. 언제 쓰는지 / 왜 쓰는지

- 브라우저에서 **네트워크 요청(HTTP 요청)** 을 보낼 때 사용
- 서버에서 데이터 가져오기(GET) 또는 데이터 보내기(POST 등)에 활용
- Ajax 라이브러리(jQuery, axios) 없이 **내장된 표준 API** 로 가능
- Promise 기반 → 비동기 처리 필수(async/await 또는 then 사용)

---

## 2. 기본 사용법

```js
// GET 요청 → 데이터 불러오기
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((res) => res.json()) // 응답을 JSON으로 변환
  .then((data) => {
    console.log("서버 데이터:", data);
  });
```

---

## 3. 확장된 사용법

### 3-1. POST 요청 (데이터 보내기)

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST", // 요청 방식
  headers: {
    "Content-Type": "application/json", // 보낼 데이터 형식
  },
  body: JSON.stringify({
    title: "새 글",
    body: "내용입니다",
    userId: 1,
  }),
})
  .then((res) => res.json())
  .then((data) => console.log("서버 응답:", data));
```

### 3-2. async/await 버전

```js
async function getPost(id) {
  try {
    const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
    if (!res.ok) throw new Error(`HTTP 오류: ${res.status}`);
    const data = await res.json();
    console.log("데이터:", data);
  } catch (err) {
    console.error("에러 발생:", err);
  }
}

getPost(1);
```

---

## 4. 체크리스트

- [ ] GET 요청이면 `.json()` 또는 `.text()`로 응답 파싱했는가?
- [ ] POST/PUT 요청이면 `body`를 `JSON.stringify` 했는가?
- [ ] `response.ok`로 HTTP 상태 코드 확인했는가?
- [ ] async/await 사용 시 try/catch로 에러 처리했는가?

---

## 5. 트러블슈팅

- “에러가 안 잡혀요” → fetch는 네트워크 실패만 reject → `response.ok` 직접 확인 필요
- “한글 깨짐” → `headers: { "Content-Type": "application/json; charset=utf-8" }` 확인
- “데이터가 안 보임” → `.json()` 호출 안 했을 가능성 있음
- “CORS 에러” → 브라우저 보안 정책 문제 → 서버에서 CORS 허용 필요

---
