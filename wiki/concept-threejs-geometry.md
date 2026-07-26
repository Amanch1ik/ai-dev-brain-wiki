# Three.js: геометрия

Форма меша. В основе — **BufferGeometry**: набор типизированных атрибутов (позиции вершин, нормали, UV, цвета), которые уходят на GPU одним куском.

## Содержание

**Встроенные примитивы:** `BoxGeometry`, `SphereGeometry`, `PlaneGeometry`, `CylinderGeometry`, `TorusGeometry`, `TorusKnotGeometry`, `IcosahedronGeometry`, `CircleGeometry`, `RingGeometry`, `TubeGeometry`, `LatheGeometry`, `ExtrudeGeometry` (выдавливание 2D-формы), `TextGeometry` (нужен загруженный шрифт).

Из примитивов быстро собирается «игрушечная» сцена, но органику и сложные модели ими не сделать — там нужен внешний редактор и импорт ([[concept-threejs-gltf]]).

### Атрибуты
```js
const g = new THREE.BufferGeometry();
g.setAttribute('position', new THREE.BufferAttribute(new Float32Array(verts), 3));
g.setAttribute('uv',       new THREE.BufferAttribute(new Float32Array(uvs), 2));
g.computeVertexNormals();   // нормали для корректного света
```
- `position` — координаты вершин (по 3 числа).
- `normal` — нормали (освещение).
- `uv` — текстурные координаты ([[concept-threejs-textures]]).
- `index` — индексы для переиспользования вершин (меньше данных).
- Кастомные атрибуты уходят в шейдер как `attribute` — база для [[concept-glsl-shaders]] (например, displacement по шуму).

### Производительность
- **InstancedMesh** — тысячи копий одной геометрии одним draw call.
- Переиспользуй одну геометрию/материал для многих мешей.
- `geometry.dispose()` при удалении — иначе утечка GPU-памяти.
- Меньше полигонов + LOD для дальних объектов. См. [[concept-webgl-performance]].

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-scene-graph]] — Mesh = геометрия + материал
- [[concept-threejs-materials]] — вторая половина меша
- [[concept-threejs-textures]] — UV-координаты для наложения текстур
- [[concept-glsl-shaders]] — кастомные атрибуты в вершинный шейдер
- [[concept-webgl-performance]] — инстансинг, dispose, LOD

## Источник

- raw/discoverthreejs-first-scene.md · raw/d3-transformations.md — Discover three.js
