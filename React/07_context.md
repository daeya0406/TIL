# React Context 정리

---

## 1. 언제 쓰는지 / 왜 쓰는지

- **props drilling(프로퍼티 뚫어내리기)** 문제를 해결하기 위해 사용
- 여러 컴포넌트에서 같은 값을 공유해야 할 때 유용
- 예: 테마(다크/라이트), 로그인 사용자 정보, 언어 설정

---

## 2. 기본 사용법

### 2-1. Context 생성

```js
import { createContext } from "react";

export const ThemeContext = createContext("light"); // 기본값 "light"
```

### 2-2. Provider로 값 내려주기

```js
import { ThemeContext } from "./ThemeContext";

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Page />
    </ThemeContext.Provider>
  );
}
```

### 2-3. Consumer(또는 useContext)로 값 가져오기

```js
import { useContext } from "react";
import { ThemeContext } from "./ThemeContext";

function Header() {
  const theme = useContext(ThemeContext);
  return <h1>현재 테마: {theme}</h1>;
}
```

---

## 3. 확장된 사용법

### 3-1. 객체/상태 값 내려주기

```js
const UserContext = createContext(null);

function App() {
  const [user, setUser] = useState({ name: "철수", age: 20 });
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Profile />
    </UserContext.Provider>
  );
}

function Profile() {
  const { user, setUser } = useContext(UserContext);
  return (
    <>
      <p>
        {user.name} ({user.age})
      </p>
      <button onClick={() => setUser({ ...user, age: user.age + 1 })}>
        나이 +1
      </button>
    </>
  );
}
```

### 3-2. 여러 Context 동시에 사용

```js
const ThemeContext = createContext("light");
const AuthContext = createContext(null);

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <AuthContext.Provider value={{ loggedIn: true }}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}

function Page() {
  const theme = useContext(ThemeContext);
  const auth = useContext(AuthContext);
  return (
    <p>
      theme: {theme}, loggedIn: {auth.loggedIn.toString()}
    </p>
  );
}
```

---

## 4. 체크리스트

- [ ] props drilling(중첩 전달) 문제를 Context로 해결했는가?
- [ ] Provider가 최상위에 위치해 필요한 하위 컴포넌트가 모두 접근 가능한가?
- [ ] `useContext`로 값을 쉽게 꺼내 썼는가?
- [ ] Context 값이 자주 바뀌면 성능에 영향 줄 수 있다는 점을 고려했는가?

---

## 5. 트러블슈팅

- “값이 안 바뀌어요” → `Provider`의 value가 새로운 객체/값으로 교체되는지 확인
- “컴포넌트마다 값이 달라요” → `Provider` 범위를 잘못 감쌌을 가능성 있음
- “성능이 떨어져요” → Context 값이 자주 바뀌면 모든 Consumer가 리렌더됨 → 필요 시 `memo`나 분리된 Context 사용 고려

---
