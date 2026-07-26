# drei (@react-three/drei)

Растущая коллекция хелперов и готовых абстракций для [[lib-react-three-fiber]]. Резко сокращает бойлерплейт при 3D в React. От pmndrs.

## Содержание

Готовые компоненты и хуки, которых нет «из коробки» в чистом three.js/R3F:
- **Камеры/контролы:** `PerspectiveCamera`, `OrbitControls`, `CameraControls`.
- **Загрузка/produktivность:** `useGLTF`, `Loader`, `Preload`, `useTexture`, `Detailed` (LOD) — прямо про [[concept-webgl-performance]].
- **Материалы/шейдеры:** `shaderMaterial` (удобная обёртка для [[concept-glsl-shaders]]), `MeshTransmissionMaterial`, `Environment` (HDRI).
- **Эффекты «дорогого» вида:** `Float`, `Sparkles`, `MeshDistortMaterial`, `Text`/`Text3D`, `Html` (встроить DOM в 3D), `ScrollControls` (3D по скроллу — мостик к [[concept-scroll-driven-animation]]).

Использует standalone `three-stdlib` вместо `three/examples/jsm`. `native`-роут не экспортирует `Html`/`Loader`. Установка: `npm install @react-three/drei`. ⚠ Компоненты добавляются/меняются — сверяйся с pmndrs.github.io/drei.

## Связано с

- [[lib-react-three-fiber]] — drei дополняет R3F готовыми абстракциями
- [[lib-three-js]] — обёртки над примитивами three.js
- [[concept-webgl-performance]] — лоадеры, LOD, Preload, инстансинг
- [[concept-glsl-shaders]] — `shaderMaterial` как удобный вход в шейдеры
- [[method-premium-creative-web]] — ускоряет сборку 3D-сцен

- [[concept-threejs-gltf]] — `useGLTF` с кэшем
- [[concept-threejs-lights]] — `Environment` и готовые сетапы света

- [[concept-threejs-controls]] — React-обёртки контролов камеры

- [[method-immersive-web-interaction-patterns]] — `useCursor`, `MeshDistortMaterial`, `Float`
- [[method-webgl-performance-degradation]] — `PerformanceMonitor`, `Detailed`, грабли `Instances`

## Источник

- raw/drei-readme.md — pmndrs/drei (README)
