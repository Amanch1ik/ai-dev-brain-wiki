# Three.js: загрузка моделей (glTF)

Примитивами ([[concept-threejs-geometry]]) органику не сделать — сложные модели готовят в Blender и импортируют. Стандарт обмена для веба — **glTF**.

## Содержание

**glTF** (GL Transmission Format, от Khronos — тех же, кто делает WebGL) называют «**JPEG для 3D**». С 2017 стал де-факто стандартом: спроектирован под веб, файлы маленькие, грузятся быстро. Старые форматы проигрывают: OBJ не поддерживает анимацию, FBX — закрытый формат Autodesk, Collada (DAE) переусложнён и тяжёл.

> Всегда используй **glTF 2** — версия 1 не поддерживается three.js.

### Два вида файлов
- **`.gltf`** — несжатый JSON, может тянуть отдельный `.bin` и внешние текстуры. Читается глазами → удобно для отладки.
- **`.glb`** — бинарный, всё в одном файле, **заметно меньше** → это и бери в продакшн.

glTF может содержать не только меш: материалы, текстуры, анимации, скелет, камеры, свет — **целую сцену**.

### Загрузка
```js
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const loader = new GLTFLoader();
const gltf = await loader.loadAsync('/models/bird.glb');
scene.add(gltf.scene);              // модель — обычный Object3D-граф
// анимации: gltf.animations → THREE.AnimationMixer
```
Все загрузчики three.js работают одинаково, так что `FBXLoader`/`OBJLoader` подключаются тем же способом.

### Сжатие и производительность
- **Draco** (`DRACOLoader`) — сжатие геометрии; **Meshopt** — альтернатива.
- **KTX2/Basis** для текстур внутри модели ([[concept-threejs-textures]]).
- Прогоняй ассеты через `gltf-transform` / `gltfpack`: чистка, сжатие, объединение.
- Low-poly модели тянут даже слабые мобильные. Ленивая загрузка + лоадер-плейсхолдер — [[concept-webgl-performance]].

В R3F: `useGLTF('/model.glb')` из [[lib-drei]] (с кэшем и `preload`).

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-geometry]] — почему примитивов не хватает
- [[concept-threejs-textures]] — текстуры внутри моделей
- [[concept-webgl-performance]] — Draco/KTX2, ленивая загрузка
- [[lib-drei]] — `useGLTF`
- [[concept-threejs-scene-graph]] — загруженная модель как Object3D-граф

- [[concept-threejs-animation]] — клипы приходят внутри моделей

## Источник

- raw/d3-load-models.md — Discover three.js, «Load 3D Models (glTF)»
