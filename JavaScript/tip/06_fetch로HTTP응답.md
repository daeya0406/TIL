# 프론트엔드 HTTP 요청 치트시트 (fetch + JSON)

필요할 때 바로 복붙해서 쓰는 **JIT(Just-In-Time)** 가이드.  
**요청 포맷 1줄**만 기억하면 대부분 해결된다.

---

## 0) 기본 포맷 (무조건 머리에 박을 것)

```js
fetch("URL", {
  method: "HTTP 메서드",     // GET(조회), POST(생성), PUT(전체 수정), PATCH(부분 수정), DELETE(삭제)
  headers: { "Content-Type": "application/json" }, // JSON 전송 시
  body: JSON.stringify({ ... }) // POST/PUT/PATCH에서 보낼 데이터
});
```

- GET/DELETE는 보통 `body` 없음
- 서버가 **JSON 응답**이면 `await res.json()`으로 파싱

---

## 1) GET — 데이터 조회

```js
const url = new URL("https://api.example.com/posts");
url.searchParams.set("page", 1); // append(key, value) : 값 추가, get(key) : 첫 번째 값 가져오기, has : 키 존재 여부 확인, delete(key)
url.searchParams.set("limit", 10);

const res = await fetch(url.toString());
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();
```

---

## 2) POST — 데이터 생성 (JSON)

```js
const res = await fetch("https://api.example.com/posts", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "새 글", content: "내용", userId: 1 }),
});
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();
```

---

## 3) PUT / PATCH — 수정

```js
// PUT: 전체 치환
await fetch("https://api.example.com/posts/1", {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "전체 수정된 제목", content: "전부 교체" }),
});

// PATCH: 일부 변경
await fetch("https://api.example.com/posts/1", {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "제목만 수정" }),
});
```

---

## 4) DELETE — 삭제

```js
const res = await fetch("https://api.example.com/posts/1", {
  method: "DELETE",
});
if (!res.ok) throw new Error(`HTTP ${res.status}`);
```

---

## 5) 헤더(Headers) — 자주 쓰는 것들

```js
const headers = {
  "Content-Type": "application/json", // JSON 보낼 때
  Authorization: "Bearer 토큰값", // 인증 필요 시
  Accept: "application/json", // JSON 응답 기대
};
```

---

## 6) 응답 파싱(Responses)

```js
const res = await fetch("/api/file");
const asJson = await res.json(); // 응답이 JSON일 때
const asText = await res.text(); // 평문
const asBlob = await res.blob(); // 파일/이미지
const asArrayBuffer = await res.arrayBuffer(); // 바이너리
```

---

## 7) 에러 처리 & finally (권장 템플릿)

```js
async function fetchJson(url, options) {
  const res = await fetch(url, options);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

try {
  const data = await fetchJson("/api/posts");
  // 성공 처리
} catch (e) {
  // 에러 처리
} finally {
  // 로딩 끄기 / 자원 정리
}
```

---

## 8) 요청 취소(AbortController) — 리액트에서 특히 중요

```js
const ac = new AbortController();
try {
  const res = await fetch("/api/search?q=react", { signal: ac.signal });
  const data = await res.json();
} catch (e) {
  if (e.name !== "AbortError") throw e;
}
// 필요 시
ac.abort();
```

---

## 9) URLSearchParams — 쿼리스트링 다루기

```js
const url = new URL("https://api.example.com/search");
url.searchParams.set("q", "react");
url.searchParams.set("page", 2);
url.searchParams.has("q"); // 존재 여부
url.searchParams.get("page"); // 값 읽기
url.searchParams.delete("page"); // 삭제
```

---

## 10) FormData — 파일 업로드/폼 전송

```js
const form = new FormData();
form.append("file", fileInput.files[0]);
form.append("title", "이미지 업로드");

// 주의: FormData는 Content-Type을 직접 쓰지 말 것 (브라우저가 자동 설정)
const res = await fetch("/api/upload", { method: "POST", body: form });
```

---

## 11) 인증 토큰 붙이기

```js
const token = localStorage.getItem("access_token");
const res = await fetch("/api/me", {
  headers: { Authorization: `Bearer ${token}` },
});
```

---

## 12) 페이지네이션/필터 — 패턴

```js
function buildUrl(base, params = {}) {
  const url = new URL(
    base,
    typeof window !== "undefined" ? window.location.origin : undefined
  );
  Object.entries(params).forEach(([k, v]) => {
    if (v === undefined || v === null || v === "") return;
    Array.isArray(v)
      ? v.forEach((val) => url.searchParams.append(k, String(val)))
      : url.searchParams.set(k, String(v));
  });
  return url.toString();
}

const url = buildUrl("/api/posts", {
  page: 1,
  limit: 10,
  tag: ["react", "js"],
});
const data = await fetchJson(url);
```

---

## 13) 여러 요청 병렬(성능↑)

```js
const [user, posts] = await Promise.all([
  fetchJson("/api/user/1"),
  fetchJson("/api/user/1/posts"),
]);
```

---

## 14) 재시도(간단 유틸)

```js
async function retry(fn, retries = 3, delay = 300) {
  let lastErr;
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (e) {
      lastErr = e;
      await new Promise((r) => setTimeout(r, delay));
    }
  }
  throw lastErr;
}

const data = await retry(() => fetchJson("/api/flaky"));
```

---

## 15) 리액트에서의 표준 패턴 (로딩/성공/에러)

```js
import { useEffect, useState } from "react";

function useApi(url, deps = []) {
  const [state, setState] = useState({
    loading: true,
    data: null,
    error: null,
  });

  useEffect(() => {
    const ac = new AbortController();
    (async () => {
      try {
        setState((s) => ({ ...s, loading: true, error: null }));
        const res = await fetch(url, { signal: ac.signal });
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const data = await res.json();
        setState({ loading: false, data, error: null });
      } catch (e) {
        if (e.name !== "AbortError")
          setState({ loading: false, data: null, error: e });
      }
    })();
    return () => ac.abort();
  }, deps);

  return state;
}

// 사용 예시
function PostList({ page }) {
  const url = buildUrl("https://jsonplaceholder.typicode.com/posts", {
    _start: (page - 1) * 10,
    _limit: 10,
  });
  const { loading, data, error } = useApi(url, [url]);

  if (loading) return <p>로딩…</p>;
  if (error) return <p>에러: {String(error.message || error)}</p>;
  return (
    <ul>
      {data.map((p) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}
```

---

## 16) CORS 간단 메모

- **CORS 에러**는 코드가 틀린 게 아니라, **서버가 허용(Access-Control-Allow-Origin)** 안 한 것
- 로컬 개발은 프록시/백엔드 설정으로 해결 (프론트에서 임의 해결 불가)

---

## 17) 체크리스트 (요청 전)

- [ ] **URL** 맞나? 쿼리스트링은 `URLSearchParams`로 붙였나?
- [ ] **method** 올바른가? (GET/POST/PUT/PATCH/DELETE)
- [ ] **headers** 설정했나? (JSON → `Content-Type`)
- [ ] **body** JSON이면 `JSON.stringify` 했나?
- [ ] **에러 처리**(res.ok 체크) 했나?
- [ ] **취소/중복 방지**가 필요한가? (AbortController)
- [ ] **병렬 가능**한가? (`Promise.all`)
- [ ] **민감정보**(토큰/키) 안전하게 관리 중인가?

---

## 18) 가장 많이 쓰는 2줄

```js
const res = await fetch(url, options);
const data = await res.json();
```

이 두 줄 + 기본 포맷만 기억해도,  
**“데이터 받아오기/보내기/활용”은 대부분 커버**된다.
