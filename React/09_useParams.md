# React Router `useParams`

---

## 1. 언제 쓰는지 / 왜 쓰는지

- URL에 **동적 경로(파라미터)** 를 포함시켜 페이지를 구분하고 싶을 때 사용
- 예: `/courses/react`, `/courses/vue` 같은 상세 페이지
- `useParams()`를 사용하면 `path`에서 정의한 변수 값을 쉽게 가져올 수 있음

---

## 2. 기본 사용법

```js
import { Route, Routes, useParams } from "react-router-dom";

function CourseDetail() {
  const { slug } = useParams(); // URL에서 slug 값 가져오기
  return <h2>Course: {slug}</h2>;
}

export default function App() {
  return (
    <Routes>
      <Route path="courses/:slug" element={<CourseDetail />} />
    </Routes>
  );
}
```

- `/courses/react` → `{ slug: "react" }`
- `/courses/vue` → `{ slug: "vue" }`

---

## 3. 확장된 사용법

### 여러 개의 파라미터

```js
<Route path="users/:userId/posts/:postId" element={<PostDetail />} />;

function PostDetail() {
  const { userId, postId } = useParams();
  console.log(userId, postId);
}
// /users/10/posts/99 → { userId: "10", postId: "99" }
```

### 타입 변환

- `useParams()` 값은 항상 문자열
- 숫자 필요하면 변환 필수

```js
const { userId } = useParams();
const id = Number(userId);
```

---

## 4. 체크리스트

- [ ] 라우트 path에 `:paramName`을 정의했는가?
- [ ] 컴포넌트에서 `useParams()`로 꺼내 썼는가?
- [ ] 숫자/boolean 값은 변환했는가?
- [ ] 존재하지 않는 경로일 때 `undefined` 처리했는가?

---

## 5. 트러블슈팅

- `params`가 `{}` 비어있음 → 라우트 정의에서 `:파라미터` 누락
- 값이 항상 문자열 → 타입 변환 필요
- 같은 이름의 param 중복 사용 불가 (고유해야 함)

---

## 🚧 보너스: `search params`와 차이

- **useParams** → 경로 기반 값 (`/users/10`)
- **useSearchParams** → 쿼리스트링 값 (`/users?tab=posts`)

```js
import { useSearchParams } from "react-router-dom";

function UserPage() {
  const [searchParams] = useSearchParams();
  const tab = searchParams.get("tab");
  return <div>Current tab: {tab}</div>;
}
```

---
