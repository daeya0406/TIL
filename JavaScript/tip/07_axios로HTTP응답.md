(이 줄은 복사 후 삭제)

# 🌐 Axios HTTP 요청 치트시트

`axios`는 `fetch`와 같은 역할(HTTP 요청)을 하지만,  
자동 JSON 처리, 에러 핸들링, 인터셉터 등 **편의 기능**이 많아서 실무에서 자주 사용된다.

---

## 1) 기본 GET 요청

```js
const res = await axios.get("/api/posts");
console.log(res.status); // 상태 코드
console.log(res.data); // 서버에서 준 실제 데이터
```

- fetch는 `await res.json()`이 필요했지만, axios는 `res.data`에 바로 들어있음.

---

## 2) POST 요청

```js
const res = await axios.post("/api/posts", {
  title: "새 글",
  content: "내용입니다",
});
console.log(res.data);
```

- fetch는 `headers + JSON.stringify` 필요했지만, axios는 객체 그대로 넘기면 자동 변환됨.

---

## 3) PUT / PATCH 요청

```js
// PUT: 전체 수정
await axios.put("/api/posts/1", { title: "수정된 글", content: "전부 교체" });

// PATCH: 일부 수정
await axios.patch("/api/posts/1", { title: "제목만 수정" });
```

---

## 4) DELETE 요청

```js
await axios.delete("/api/posts/1");
```

---

## 5) 에러 처리

```js
try {
  await axios.get("/api/없는주소");
} catch (err) {
  console.log(err.response.status); // 상태 코드
  console.log(err.message); // 에러 메시지
}
```

- fetch는 `if (!res.ok)`로 직접 체크해야 했지만,
- axios는 상태 코드가 200~299가 아니면 자동으로 throw → `catch`에서 바로 처리.

---

## 6) 기본 설정 (Base URL, Headers)

```js
const api = axios.create({
  baseURL: "https://api.example.com",
  timeout: 3000, // 요청 제한 시간(ms)
  headers: { Authorization: "Bearer 토큰값" },
});

const res = await api.get("/posts"); // baseURL + 헤더 자동 적용
```

---

## 7) 요청/응답 인터셉터

```js
// 요청 직전에 가로채기
axios.interceptors.request.use((config) => {
  console.log("요청 전:", config);
  return config;
});

// 응답 직후에 가로채기
axios.interceptors.response.use(
  (response) => response,
  (error) => {
    console.error("응답 에러:", error);
    return Promise.reject(error);
  }
);
```

---

## 8) 요청 취소 (AbortController)

```js
const controller = new AbortController();
axios.get("/api/posts", { signal: controller.signal });
controller.abort(); // 요청 취소
```

---

## 9) params (쿼리스트링 자동 처리)

```js
const res = await axios.get("/api/posts", {
  params: { page: 1, limit: 10, search: "react" },
});
// 실제 요청: /api/posts?page=1&limit=10&search=react
```

---

## 10) 여러 요청 동시에 (Promise.all)

```js
const [user, posts] = await Promise.all([
  axios.get("/api/user/1"),
  axios.get("/api/user/1/posts"),
]);
console.log(user.data, posts.data);
```

---

## 11) 공통 인스턴스 + API 함수 패턴

```js
// 인스턴스 생성
const instance = axios.create({
  baseURL: "https://learn.codeit.kr/api",
  timeout: 3000,
  headers: { "Content-Type": "application/json" },
});

// API 함수 정의
export async function getColorSurveys(params = {}) {
  const res = await instance.get("/color-surveys", { params });
  return res.data;
}

export async function createAvatar(avatarData) {
  const res = await instance.post("/avatars", avatarData);
  return res.data;
}
```

- 공통 설정을 인스턴스로 묶고, API 함수로 모듈화하면 유지보수 👍

---

## ✅ 정리

- **fetch**: 브라우저 기본 제공 → 가볍고 표준적이지만 불편한 부분이 있음
- **axios**: 설치 필요하지만, 자동 JSON, 에러 자동 throw, 인스턴스, 인터셉터, params 지원, 요청 취소 등 실무 기능 풍부
- 큰 규모 프로젝트에선 axios가 훨씬 편하다.

(이 줄은 복사 후 삭제)
