# Three.js

Самая распространённая JavaScript-библиотека для 3D/WebGL в браузере: сцены, камеры, меши, материалы, свет, загрузчики моделей. Фундамент 3D в [[method-premium-creative-web]].

## Содержание

Three.js абстрагирует WebGL: строишь `Scene` с объектами (`Mesh` = геометрия + материал), `Camera`, рендеришь через `WebGLRenderer` в render-loop. Экосистема: GLTF-загрузчики, постпроцессинг, контролы, физика.

- **Где применять:** hero-сцены, product viewer (вращаемая модель), поля частиц, displaced-меши, интерактивные фоны.
- **Кастомные материалы** — через [[concept-glsl-shaders]] (`ShaderMaterial`): именно тут рождается «дорогой» вид (fluid, noise, iridescence).
- В React удобнее декларативно — через [[lib-react-three-fiber]] (JSX вместо императивного API).
- **Перформанс критичен** — см. [[concept-webgl-performance]] (сжатие текстур KTX2/Basis, instancing, LOD, cap pixel ratio).

Инструменты: three.js editor, devtools-расширение, обширные examples. ⚠ API эволюционирует (модули, WebGPU-ренедерер) — сверяйся с threejs.org/docs.

## Карта изучения (подкластер)

| Тема | Страница |
|---|---|
| Сцена, Object3D, камера, трансформации | [[concept-threejs-scene-graph]] |
| Геометрия, BufferGeometry, атрибуты, инстансинг | [[concept-threejs-geometry]] |
| Материалы и PBR (metalness/roughness) | [[concept-threejs-materials]] |
| Свет и тени, HDRI-окружение | [[concept-threejs-lights]] |
| Текстуры, карты, цветовые пространства | [[concept-threejs-textures]] |
| Цикл рендера, delta, ресайз | [[concept-threejs-render-loop]] |
| Загрузка моделей glTF/GLB, Draco | [[concept-threejs-gltf]] |
| Анимация: AnimationMixer, скелет, морфы | [[concept-threejs-animation]] |
| Постпроцессинг: bloom, DOF, эффекты | [[concept-threejs-postprocessing]] |
| Raycasting: клики и hover по 3D | [[concept-threejs-raycasting]] |
| Управление камерой: OrbitControls и др. | [[concept-threejs-controls]] |

**Порядок освоения:** сцена → геометрия → материалы → свет → текстуры → цикл → модели → анимация → интерактив (raycasting + контролы) → постпроцессинг. Дальше — шейдеры ([[concept-glsl-shaders]]) и перформанс ([[concept-webgl-performance]]).

## Связано с

- [[concept-threejs-scene-graph]] · [[concept-threejs-geometry]] · [[concept-threejs-materials]] · [[concept-threejs-lights]] · [[concept-threejs-textures]] · [[concept-threejs-render-loop]] · [[concept-threejs-gltf]] — подкластер изучения
- [[concept-threejs-animation]] · [[concept-threejs-postprocessing]] · [[concept-threejs-raycasting]] · [[concept-threejs-controls]] — продвинутый слой
- [[lib-react-three-fiber]] — React-обёртка над three.js
- [[concept-glsl-shaders]] — кастомные материалы через шейдеры
- [[concept-webgl-performance]] — как не сделать 3D лагающим
- [[concept-scroll-driven-animation]] — WebGL-сцены, привязанные к скроллу
- [[lib-lenis]] — синхронизация 3D-сцены со smooth scroll
- [[lib-drei]] — готовые абстракции поверх примитивов three.js
- [[method-premium-creative-web]] — где three.js в стеке премиум-веба
- [[tool-hyperframes]] — 3D-контент в видео-рендере
- [[method-immersive-web-interaction-patterns]] — приёмы immersive-сцен (Active Theory/Lusion)

## Источник

- raw/threejs-home.md — threejs.org
