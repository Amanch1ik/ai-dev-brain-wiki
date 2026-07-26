# Three.js: свет и тени

Без света PBR-материалы ([[concept-threejs-materials]]) не видны. Свет и материалы в графике неразделимы — настраиваются вместе.

## Содержание

### Типы источников
| Свет | Что имитирует | Тени |
|---|---|---|
| `AmbientLight` | равномерная подсветка со всех сторон (заполняющий) | нет |
| `HemisphereLight` | небо сверху / отражение от земли снизу | нет |
| **`DirectionalLight`** | далёкий источник — **солнце**, параллельные лучи | да |
| `PointLight` | лампочка: во все стороны, затухает с расстоянием | да |
| `SpotLight` | прожектор: конус с углом и затуханием | да |
| `RectAreaLight` | софтбокс/окно (только Standard/Physical материалы) | нет |

Практика: **не полагайся на один AmbientLight** — сцена станет плоской. Классика — «солнце» (`DirectionalLight`) + мягкий заполняющий (`Hemisphere`/`Ambient`), при необходимости акцентные `PointLight`.

### Тени
```js
renderer.shadowMap.enabled = true;
light.castShadow = true;
mesh.castShadow = true;      // кто отбрасывает
floor.receiveShadow = true;  // кто принимает
// качество/охват теневой камеры:
light.shadow.mapSize.set(1024, 1024);
light.shadow.camera.near = 1; light.shadow.camera.far = 50;
```
Тени дорогие: ограничивай `mapSize`, сужай теневую камеру под сцену, включай `castShadow` только там, где нужно ([[concept-webgl-performance]]).

### Освещение окружением (часто важнее ламп)
`scene.environment` + HDRI-карта (`RGBELoader`, в R3F — `<Environment />` из [[lib-drei]]) даёт реалистичные отражения и мягкий свет. Для «дорогого» вида это обычно даёт больше, чем добавление ещё одного источника.

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-materials]] — свет и материалы неразделимы
- [[concept-threejs-scene-graph]] — свет тоже Object3D в сцене
- [[concept-webgl-performance]] — тени как источник просадок
- [[lib-drei]] — `Environment`, готовые сетапы света

## Источник

- raw/d3-pbr-materials.md — Discover three.js (DirectionalLight, свет + PBR)
