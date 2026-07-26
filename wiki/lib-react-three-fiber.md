# React Three Fiber (R3F)

React-рендерер для [[lib-three-js]] (`@react-three/fiber`): строишь 3D-сцену **декларативно** переиспользуемыми компонентами, реагирующими на состояние. От pmndrs.

## Содержание

`<mesh />` в JSX динамически превращается в `new THREE.Mesh()` — R3F просто выражает three.js через JSX.

```jsx
import { Canvas, useFrame } from '@react-three/fiber'
function Box(props) {
  const ref = useRef()
  useFrame((state, delta) => (ref.current.rotation.x += delta)) // render-loop
  return <mesh ref={ref} {...props}><boxGeometry/><meshStandardMaterial/></mesh>
}
// <Canvas><Box position={[0,0,0]}/></Canvas>
```

Факты из доки:
- **Без ограничений:** всё, что работает в three.js, работает здесь.
- **Не медленнее** plain three.js — компоненты рендерятся вне React, на масштабе даже быстрее за счёт планировщика React.
- **Успевает за three.js:** новые фичи three.js доступны сразу, без обновления R3F.
- **Версии парятся с React:** `@react-three/fiber@8` ↔ react@18, `@9` ↔ react@19.
- `useFrame` подписывает компонент на render-loop; `useRef` даёт прямой доступ к `THREE.Mesh`.
- Экосистема pmndrs: **drei** (хелперы, `shaderMaterial`, контролы, лоадеры), postprocessing, rapier (физика).

Ставится: `npm install three @types/three @react-three/fiber`. Дефолт для React-проектов в [[method-premium-creative-web]] (с [[lib-vite]]).

## Связано с

- [[lib-three-js]] — R3F это React-обёртка над ним
- [[concept-webgl-performance]] — cap `dpr`, suspense/лоадеры, инстансинг
- [[lib-vite]] — build-инструмент для R3F-проектов
- [[lib-drei]] — готовые хелперы и абстракции для R3F
- [[method-premium-creative-web]] — дефолтный 3D-слой стека

- [[concept-threejs-render-loop]] — `useFrame` как цикл рендера
- [[concept-threejs-scene-graph]] — тот же граф, но в JSX

- [[concept-threejs-postprocessing]] — `@react-three/postprocessing`
- [[concept-threejs-raycasting]] — `onClick`/`onPointerOver` поверх raycasting

- [[method-immersive-web-interaction-patterns]] — hover/pointer-lerp/scroll-uniform в R3F
- [[method-webgl-performance-degradation]] — `frameloop="demand"`, `invalidate`, PerformanceMonitor
- [[method-webgl-mobile-rn-porting]] — что из R3F не переносится на RN

## Источник

- raw/r3f-readme.md — pmndrs/react-three-fiber (README)
