# CRA vs Vite

---

## 목차

- [1. 왜 index.html script 태그가 다를까?](#1-왜-indexhtml-script-태그가-다를까)
- [2. defer 없는 건 문제 없을까?](#2-defer-없는-건-문제-없을까)
- [3. 여기서 나온 번들이 뭔가?](#3-여기서-나온-번들이-뭔가)
- [4. ESM은 뭐길래?](#4-esm은-뭐길래)
- [5. CRA(Webpack) vs Vite(ESM)](#5-crawebpack-vs-viteesm)
- [6. .js vs .jsx는 왜 나눠 쓰는 걸까?](#6-js-vs-jsx는-왜-나눠-쓰는-걸까)
- [7. 결론](#7-결론)

---

## 1. 왜 index.html script 태그가 다를까?

CRA에서는 `<head>`에 있고 `defer`가 붙어 있다:

```html
<script src="/static/js/bundle.js" defer></script>
```

Vite에서는 `<body>` 끝에 있고 `type="module"`만 있다:

```html
<script type="module" src="/src/main.jsx"></script>
```

---

## 2. defer 없는 건 문제 없을까?

- CRA는 번들 파일을 head에 넣으니까 **defer**로 실행 시점 조절이 필요.
- Vite는 body 끝에 두고, 게다가 `type="module"`은 자동으로 defer처럼 동작 → 굳이 필요 없음.

---

## 3. 여기서 나온 번들이 뭔가?

- CRA(Webpack)에서는 우리가 `import`를 써도 전부 **bundle.js 하나**로 합쳐진다.
- 브라우저는 실제로 `import`를 해석하지 않고, 그냥 번들 파일 하나만 실행한다.

---

## 4. ESM은 뭐길래?

- ES Modules = 자바스크립트 공식 모듈 시스템.
- `export` / `import` 문법이 바로 ESM.

```js
// App.jsx
export default function App() {}

// main.jsx
import App from "./App.jsx";
```

---

## 5. CRA(Webpack) vs Vite(ESM)

- CRA: Webpack이 import들을 다 묶어서 번들 하나로 만든 뒤, 브라우저는 그거만 실행.
- Vite: 브라우저가 실제로 `import`를 직접 따라가며 필요한 파일을 하나씩 요청.

---

## 6. .js vs .jsx는 왜 나눠 쓰는 걸까?

- 둘 다 ESM 모듈 가능.
- `.jsx`는 JSX 문법이 들어있다는 표시 → 도구가 변환할 때 참고.
- 사실상 컨벤션 차이, 문법 차이는 아님.

---

## 7. 결론

- CRA: `'큰 번들 파일 실행'`
- Vite: `'브라우저가 import 직접 해석'`
- `.js` vs `.jsx`: 확장자 구분일 뿐.

---
