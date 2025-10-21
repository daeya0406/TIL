## Union Type

- **정의**: 여러 타입 중 **하나의 타입만** 가질 수 있도록 정의하는 타입  
- **용도**: 함수나 변수에 다양한 타입을 유연하게 적용할 때 사용  

```ts
interface Equipment {
  id: string;
  name: string;
  price: number;
}

interface Weapon extends Equipment {
  attack: number;
}

interface Armor extends Equipment {
  defence: number;
}
```

유니온 타입을 사용해 **Weapon** 또는 **Armor** 타입을 받을 수 있다.

```ts
function printEquipment(equipment: Weapon | Armor) {
  console.log(`이름: ${equipment.name}`);

  if ("attack" in equipment) {
    console.log(`이 장비는 공격력을 ${equipment.attack} 증가 시킵니다.`);
  }

  if ("defence" in equipment) {
    console.log(`이 장비는 방어력을 ${equipment.defence} 증가 시킵니다.`);
  }
}
```

```ts
const item1: Weapon = {
  id: "w001",
  name: "전쟁 도끼",
  price: 100,
  attack: 15,
};

const item2: Armor = {
  id: "a001",
  name: "사슬 갑옷",
  price: 200,
  defence: 52,
};

printEquipment(item1);
printEquipment(item2);
```

### 타입 내로잉 (Type Narrowing)

- `Weapon | Armor` 타입처럼 여러 타입이 있을 때,  
  코드 실행 중 **특정 타입으로 좁혀서(type narrowing)** 처리해야 함.
- `"속성명" in 객체`를 사용하면 해당 속성이 존재하는 타입으로 구분할 수 있음.

```ts
if ("attack" in equipment) {
  // Weapon 타입으로 좁혀짐
}
if ("defence" in equipment) {
  // Armor 타입으로 좁혀짐
}
```

---

**정리**
- 유니온(`|`)으로 여러 타입을 하나로 묶을 수 있다.  
- 타입 내로잉을 통해 런타임에서 타입을 구분할 수 있다.  
- `"속성명" in 객체` 문법을 자주 사용한다.  
- 코드의 **유연성과 안전성**을 동시에 높여준다.
