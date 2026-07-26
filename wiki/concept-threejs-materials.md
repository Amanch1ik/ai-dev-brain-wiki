# Three.js: материалы и PBR

Как поверхность реагирует на свет. Индустриальный стандарт — **PBR** (physically based rendering): расчёт по реальной физике, «убирает угадайку» из настройки материалов и света.

## Содержание

### Типы материалов
| Материал | Свет нужен? | Когда |
|---|---|---|
| `MeshBasicMaterial` | нет | плоский цвет/текстура, UI-элементы, отладка |
| `MeshLambertMaterial` | да | дешёвое диффузное освещение |
| `MeshPhongMaterial` | да | блики (specular), старый нефизичный подход |
| **`MeshStandardMaterial`** | да | **основной PBR** — metalness/roughness |
| `MeshPhysicalMaterial` | да | PBR+ : clearcoat, transmission (стекло), iridescence |
| `ShaderMaterial` | — | свой GLSL ([[concept-glsl-shaders]]) |
| `MeshNormalMaterial` / `MeshDepthMaterial` | нет | отладка нормалей/глубины |

### PBR на практике
Ключевые параметры `MeshStandardMaterial`: **`metalness`** (0 — диэлектрик, 1 — металл) и **`roughness`** (0 — зеркало, 1 — матовое). Плюс карты: `map` (albedo), `normalMap`, `roughnessMap`, `metalnessMap`, `aoMap`, `emissiveMap`, `displacementMap` — см. [[concept-threejs-textures]].

Главная выгода PBR: **хорошо сделанный материал выглядит правдоподобно при любом освещении**. Чтобы переключить день на ночь, достаточно выключить «солнце» и включить лампы — не нужно перенастраивать все материалы (в нефизичном рендере пришлось бы всё перекручивать).

PBR **требует света в сцене** — `MeshBasicMaterial` виден без света, `MeshStandardMaterial` без источника будет чёрным ([[concept-threejs-lights]]).

⚠ **Версии:** в старых туториалах включают `renderer.physicallyCorrectLights = true`. В современных версиях three.js физически корректное освещение — поведение по умолчанию, а этот флаг убран/переименован. Сверяйся с актуальной докой ([[concept-training-cutoff]]). Также важна цветовая пайплайн-настройка (`outputColorSpace`, tone mapping).

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-lights]] — PBR-материалы бессмысленны без света
- [[concept-threejs-textures]] — карты, управляющие параметрами материала
- [[concept-threejs-geometry]] — вторая половина меша
- [[concept-glsl-shaders]] — когда нужен свой материал
- [[concept-oklch-color]] — корректная работа с цветом

- [[concept-threejs-scene-graph]] — материал как часть Mesh

## Источник

- raw/d3-pbr-materials.md — Discover three.js, «Physically Based Rendering»
