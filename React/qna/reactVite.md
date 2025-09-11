# CRA vs Vite 정리

내가 궁금했던 **CRA(Webpack)** 과 **Vite(ESM)** 차이를 정리한 노트

---

## 목차

- [1. index.html에서 본 차이](#1-indexhtml에서-본-차이)
- [2. ESM이 뭔가?](#2-esm이-뭔가)
- [3. CRA(Webpack) 방식](#3-crawebpack-방식)
- [4. Vite(ESM) 방식](#4-viteesm-방식)
- [5. .js vs .jsx 차이](#5-js-vs-jsx-차이)
- [6. 요약](#6-요약)

---

## 1. index.html에서 본 차이

CRA에서는 `<head>` 끝자락에 script가 있고 `defer`가 붙어 있다.

```html
<script src="/static/js/bundle.js" defer></script>
```

Vite에서는 `<body>` 끝자락에 script가 있고, `type="module"`만 있다.

```html
<script type="module" src="/src/main.jsx"></script>
```

→ Vite는 구조상 DOM 파싱이 끝난 뒤 실행되므로 `defer` 필요 없음.  
→ CRA는 번들 방식이라 head에 올려두고 `defer`를 써서 실행 순서를 제어.

---

## 2. ESM이 뭔가?

ESM(ES Modules)은 자바스크립트의 공식 모듈 시스템.  
`export` / `import` 문법이 바로 ESM이다.

```js
// App.jsx
export default function App() {}

// main.jsx
import App from "./App.jsx";
```

---

## 3. CRA(Webpack) 방식

- 개발할 땐 `import`를 쓰지만, Webpack이 모든 걸 **bundle.js 하나**로 묶는다.
- 브라우저는 실제로 `import`를 해석하지 않고, 그냥 큰 번들 파일만 실행한다.

---

## 4. Vite(ESM) 방식

- 개발할 때 쓴 `import`를 브라우저가 직접 이해할 수 있도록 남겨둔다.
- 브라우저가 `App.jsx` 요청 → Vite dev 서버가 그 파일만 변환해서 내려줌.
- 즉, 브라우저가 직접 모듈 그래프를 따라가며 실행한다.

---

## 5. .js vs .jsx 차이

- 둘 다 결국 ESM 모듈일 수 있다.
- `.jsx`는 “이 안에 JSX 문법이 들어 있다”는 표시.
- Webpack이나 Vite가 보고 **JSX → JS 코드**로 변환한다.
- 문법 차이가 아니라, 사람/도구가 구분하기 위한 **컨벤션** 차이.

---

## 6. 요약

- `import` / `export` 문법 = ESM
- CRA(Webpack): import는 개발 편의용, 빌드 시 번들 하나로 압축 → 브라우저는 ESM 직접 안 씀
- Vite(ESM): import를 브라우저가 직접 따라가며 모듈 단위로 로딩
- `.js` vs `.jsx`: 확장자 구분일 뿐, 문법 차이는 아님

**짧게**

- CRA: “우리가 import라고 쓰지만, Webpack이 다 압축 → 브라우저는 큰 번들만 본다”
- Vite: “우리가 import라고 쓰면, 브라우저가 직접 따라가서 파일을 하나씩 가져온다”

---
