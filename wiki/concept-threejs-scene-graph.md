# Three.js: сцена и граф объектов

Базовая модель [[lib-three-js]]: **Scene** (корень) → объекты (**Object3D**) → рендер через **WebGLRenderer** с точки зрения **Camera**. Минимальное приложение — меньше 20 строк.

## Содержание

```js
import { Scene, PerspectiveCamera, BoxGeometry, MeshBasicMaterial, Mesh, WebGLRenderer, Color } from 'three';

const scene = new Scene();
scene.background = new Color('skyblue');

// fov, aspect, near/far — плоскости отсечения
const camera = new PerspectiveCamera(35, w/h, 0.1, 100);
camera.position.set(0, 0, 10);          // всё создаётся в (0,0,0) — камеру отодвигаем

const mesh = new Mesh(new BoxGeometry(2,2,2), new MeshBasicMaterial());
scene.add(mesh);

const renderer = new WebGLRenderer();
renderer.setSize(w, h);
renderer.setPixelRatio(window.devicePixelRatio);  // ⚠ на Retina лучше кап — см. perf
container.append(renderer.domElement);            // three сам создаёт <canvas>
renderer.render(scene, camera);
```

### Ключевые сущности
- **Object3D** — базовый класс всего, что живёт в сцене. Даёт `position`, `rotation`, `scale`, `quaternion`, `add()/remove()`, `visible`.
- **Mesh** = **геометрия** ([[concept-threejs-geometry]]) + **материал** ([[concept-threejs-materials]]).
- **Group** — контейнер для логической группировки; трансформация родителя применяется к детям.
- **Scene** — корневой Object3D; хранит `background`, `environment`, `fog`.
- **Camera** — `PerspectiveCamera(fov, aspect, near, far)` для 3D-перспективы; `OrthographicCamera` для изометрии/2D.

### Граф и трансформации
Сцена — **дерево**: трансформы наследуются от родителя к детям (локальные координаты × матрица родителя = мировые). Отсюда приём: вращать `Group`, чтобы вращать всё содержимое; или вложить объект в пустой `Object3D`-«пивот», чтобы вращать вокруг смещённой оси.

⚠ **Устаревшее в старых туториалах:** `BoxBufferGeometry` → теперь просто `BoxGeometry` (Buffer-варианты слиты в основные). Сверяйся с актуальной докой — типичный случай [[concept-training-cutoff]].

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-geometry]] · [[concept-threejs-materials]] — из чего состоит Mesh
- [[concept-threejs-render-loop]] — как это анимировать
- [[lib-react-three-fiber]] — тот же граф, но декларативно в JSX
- [[concept-webgl-performance]] — cap pixelRatio и прочее

- [[concept-threejs-gltf]] — загруженная модель как Object3D-граф
- [[concept-threejs-lights]] — свет тоже Object3D в сцене

- [[concept-threejs-animation]] — что анимируем
- [[concept-threejs-controls]] — управляем камерой сцены
- [[concept-threejs-raycasting]] — по чему пускаем луч

## Источник

- raw/discoverthreejs-first-scene.md — Discover three.js, «Your First three.js Scene»
- raw/d3-transformations.md — Discover three.js, «Transformations»
