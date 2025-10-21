## keyof & typeof

### keyof

- **정의**: 타입의 키(key)를 유니언 타입으로 만들어주는 연산자
- **용도**: 특정 타입의 속성 이름들을 타입 수준에서 활용할 때 사용

```ts
interface Product {
  id: string;
  name: string;
  price: number;
}

type ProductKeys = keyof Product; // "id" | "name" | "price"
```

- 인터페이스뿐만 아니라 타입(alias)에도 사용 가능

```ts
type User = {
  username: string;
  email: string;
  age: number;
};

type UserKeys = keyof User; // "username" | "email" | "age"
```

- 실제 객체에서 사용할 때는 `typeof`와 함께 사용

```ts
const settings = {
  darkMode: true,
  version: 1.0,
};

type SettingsKeys = keyof typeof settings; // "darkMode" | "version"
```

### typeof

- **정의**: 존재하는 값(변수, 객체 등)의 타입을 가져오는 연산자
- **용도**: 이미 선언된 값의 타입을 기반으로 다른 타입을 정의할 때 사용

```ts
const product = {
  id: "c001",
  name: "블랙 후드 집업",
  price: 129000,
};

type ProductType = typeof product;
// {
//   id: string;
//   name: string;
//   price: number;
// }
```

- `keyof typeof`와 함께 사용하면 **실제 객체의 키를 타입으로 가져올 수 있음**
```ts
type ProductKeys = keyof typeof product; // "id" | "name" | "price"
```

### 정리

- `keyof` → 타입의 속성 이름(key)들을 유니언으로 뽑는다.
- `typeof` → 값(변수, 객체)의 타입을 가져온다.
- 둘을 함께 쓰면 **실제 객체의 속성 이름을 타입으로 활용 가능**.
