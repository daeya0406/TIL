# 🛣️ React Router 정리

---

## 1. 언제 쓰는지 / 왜 쓰는지

- React는 SPA(Single Page Application)라서 기본적으로 **페이지 이동이 없음**
- URL을 바꿔도 새로고침 없이 다른 화면을 보여주고 싶을 때 `React Router` 사용
- 예: `/home`, `/about`, `/profile/:id` 같은 경로에 따라 다른 컴포넌트 렌더링

---

## 2. 기본 사용법

### 2-1. 설치

```bash
npm install react-router-dom
```

### 2-2. 라우터 세팅

```js
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## 3. 확장된 사용법

### 3-1. 네비게이션(Link)

```js
import { Link } from "react-router-dom";

function Nav() {
  return (
    <nav>
      <Link to="/">홈</Link>
      <Link to="/about">소개</Link>
    </nav>
  );
}
```

### 3-2. 파라미터 사용

```js
import { useParams } from "react-router-dom";

function Profile() {
  const { id } = useParams();
  return <h2>프로필 ID: {id}</h2>;
}

// App.js
<Route path="/profile/:id" element={<Profile />} />;
```

### 3-3. 쿼리스트링 사용

```js
import { useSearchParams } from "react-router-dom";

function Search() {
  const [searchParams] = useSearchParams();
  const keyword = searchParams.get("q");
  return <p>검색어: {keyword}</p>;
}
```

### 3-4. 프로그래밍 네비게이션

```js
import { useNavigate } from "react-router-dom";

function LoginButton() {
  const navigate = useNavigate();
  const handleLogin = () => {
    // 로그인 처리 후 이동
    navigate("/profile/123");
  };
  return <button onClick={handleLogin}>로그인</button>;
}
```

### 3-5. 중첩 라우팅

```js
<Route path="/dashboard" element={<Dashboard />}>
  <Route path="stats" element={<Stats />} />
  <Route path="settings" element={<Settings />} />
</Route>
```

➡️ `/dashboard/stats`, `/dashboard/settings`로 접근 가능

---

## 4. 체크리스트

- [ ] `BrowserRouter`로 앱 전체를 감쌌는가?
- [ ] `Route`는 `Routes` 안에서 정의했는가?
- [ ] URL 파라미터는 `useParams`로 가져왔는가?
- [ ] 쿼리스트링은 `useSearchParams`로 처리했는가?
- [ ] 코드에서 이동하려면 `useNavigate`를 썼는가?

---

## 5. 트러블슈팅

- “라우터가 안 먹혀요” → `BrowserRouter`로 감쌌는지 확인
- “페이지가 새로고침돼요” → `<a>` 대신 `<Link>` 사용
- “파라미터가 undefined에요” → `Route` 경로에 `:id` 같은 패턴이 있는지 확인
- “상대 경로 이동이 이상해요” → `navigate` 기본은 상대 경로, 절대 경로 쓰려면 `/`로 시작

---
