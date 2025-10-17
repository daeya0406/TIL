## Interface (인터페이스)

- **정의**: 객체의 구조(모양)를 정의하는 타입  
- **용도**: 여러 객체가 같은 속성과 타입을 가질 때 **재사용**하기 위해 사용  

```ts
interface Product {
  id: string;
  name: string;
  price: number;
  membersOnly?: boolean;
}
```

```ts
const product1: Product = {
  id: "c001",
  name: "코드잇 블랙 후드 집업",
  price: 129000,
  membersOnly: true,
};

const product2: Product = {
  id: "d001",
  name: "코드잇 텀블러",
  price: 25000,
};
```

함수와 함께 사용할 수도 있다.
```ts
interface PrintProductFunction {
  (product: Product): void;
}

const printProduct: PrintProductFunction = (product) => {
  console.log(`${product.name}의 가격은 ${product.price}원입니다.`);
};

printProduct(product1);
printProduct(product2);
```

### 인터페이스 확장 (extends)

- 기존 인터페이스를 **상속받아 확장**할 수 있다.  
- 중복되는 속성을 줄이고, 공통 구조를 재사용하기 좋다.

```ts
interface ClothingProduct extends Product {
  size: "S" | "M" | "L" | "XL";
}

const hoodie: ClothingProduct = {
  id: "c002",
  name: "코드잇 후드티",
  price: 89000,
  size: "L",
};
```

**정리**
- 인터페이스로 객체 구조를 정의한 것을 재사용할 수 있다.(유지보수도 용이)
- `extends`로 인터페이스를 확장할 수 있다.
- 함수와 함께 사용할 수도 있다.
