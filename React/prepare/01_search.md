# 🔎 검색 기능 정리 (cursor 기반)

검색 기능을 정렬/더보기 기능과 함께 동작하도록 구현하는 방법을 정리.

---

## 목차

- [1. 목표](#1-목표)
- [2. API 구성](#2-api-구성)
- [3. 상태 정의](#3-상태-정의)
- [4. 검색 제출 흐름](#4-검색-제출-흐름)
- [5. 데이터 로드 함수](#5-데이터-로드-함수)
- [6. 자동 로드(useEffect)](#6-자동-로드useeffect)
- [7. 더보기 버튼](#7-더보기-버튼)
- [8. 렌더 예시](#8-렌더-예시)
- [9. 체크리스트](#9-체크리스트)
- [10. 트러블슈팅](#10-트러블슈팅)

---

## 1. 목표

- 입력 폼으로 검색어 입력
- 서버에 `search` 쿼리 포함해 요청
- 새 검색이면 리스트 교체, 더보기면 이어붙이기
- 정렬과 검색이 동시에 적용

---

## 2. API 구성

```js
// api.js
export async function getFoods({
  order = "",
  cursor = "",
  limit = 10,
  search = "",
}) {
  const query = `order=${order}&cursor=${cursor}&limit=${limit}&search=${search}`;
  const response = await fetch(`https://learn.codeit.kr/api/foods?${query}`);
  if (!response.ok) throw new Error("데이터를 불러오는데 실패했습니다");
  return response.json(); // { foods: [...], paging: { nextCursor } }
}
```

---

## 3. 상태 정의

```js
const [order, setOrder] = useState("createdAt");
const [items, setItems] = useState([]);
const [cursor, setCursor] = useState("");
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState(null);
const [search, setSearch] = useState("");
const LIMIT = 10;
```

---

## 4. 검색 제출 흐름

```js
const handleSearchSubmit = (e) => {
  e.preventDefault();
  const keyword = e.target["search"].value.trim();
  setSearch(keyword); // 검색어 변경 → useEffect에서 새 요청
  setCursor(""); // 새 검색이면 첫 페이지부터
};
```

---

## 5. 데이터 로드 함수

```js
const loadFoods = async ({ order, cursor, limit, search }) => {
  try {
    setError(null);
    setIsLoading(true);

    const { foods, paging } = await getFoods({ order, cursor, limit, search });
    const nextCursor = paging?.nextCursor ?? "";

    if (!cursor) setItems(foods); // 교체
    else setItems((prev) => [...prev, ...foods]); // 추가

    setCursor(nextCursor);
  } catch (err) {
    setError(err);
  } finally {
    setIsLoading(false);
  }
};
```

---

## 6. 자동 로드(useEffect)

```js
useEffect(() => {
  loadFoods({ order, cursor: "", limit: LIMIT, search });
}, [order, search]);
```

정렬이나 검색어가 바뀔 때마다 **처음부터 다시 불러오기**.

---

## 7. 더보기 버튼

```js
const handleLoadMore = () => {
  if (!cursor || isLoading) return;
  loadFoods({ order, cursor, limit: LIMIT, search });
};
```

---

## 8. 렌더 예시

```jsx
<form onSubmit={handleSearchSubmit}>
  <input name="search" placeholder="예: 토마토" />
  <button type="submit" disabled={isLoading}>검색</button>
</form>

<FoodList items={items} />

{cursor && (
  <button onClick={handleLoadMore} disabled={isLoading}>
    {isLoading ? "로딩 중..." : "더보기"}
  </button>
)}

{error?.message && <p style={{ color: "crimson" }}>{error.message}</p>}
```

---

## 9. 체크리스트

- [ ] 새 검색/정렬 시 `cursor`를 초기화했는가?
- [ ] 더보기 요청에도 `search`, `order`를 포함했는가?
- [ ] `isLoading`으로 중복 요청 방지했는가?
- [ ] 응답 구조 `{ foods, paging.nextCursor }`를 확인했는가?

---

## 10. 트러블슈팅

- 결과가 계속 이어붙여짐 → 새 검색 시 `cursor` 초기화 확인
- 더보기가 안 뜸 → 응답에 `paging.nextCursor`가 오는지 확인
- 검색어 한글 → `fetch`가 자동으로 인코딩해주므로 그대로 써도 문제 없음

---
