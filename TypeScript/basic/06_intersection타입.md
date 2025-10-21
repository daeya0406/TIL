## Intersection Type (교차 타입)

- **정의**: 여러 타입을 **합쳐서 하나의 타입**으로 만드는 것  
- **용도**: 여러 타입의 속성을 모두 가져야 할 때 사용  

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

// Weapon과 Armor의 속성을 모두 가져야 하는 타입
function printEquipment(equipment: Weapon & Armor) {
  console.log(`이름: ${equipment.name}`);
  console.log(`이 장비는 공격력을 ${equipment.attack}, 방어력을 ${equipment.defence} 증가 시킵니다.`);
}

const item1: Weapon & Armor = {
  id: 'g001',
  name: '서리불꽃 글러브',
  price: 100,
  attack: 5,
  defence: 42,
};

printEquipment(item1);
```

**정리**
- `&`를 사용해 여러 타입을 합칠 수 있음  
- 합쳐진 타입은 **모든 속성을 갖춰야 함**  
- 유니온(`|`)과 달리, **하나라도 빠지면 타입 에러**  
