# Three.js: текстуры

Изображения, управляющие параметрами материала. Накладываются по **UV**-координатам геометрии ([[concept-threejs-geometry]]).

## Содержание

```js
const loader = new THREE.TextureLoader();
const map = loader.load('/textures/wood_albedo.jpg');
map.colorSpace = THREE.SRGBColorSpace;   // ⚠ только для цветовых карт
map.wrapS = map.wrapT = THREE.RepeatWrapping;
map.repeat.set(4, 4);
material.map = map;
```

### Типы карт (для PBR — [[concept-threejs-materials]])
- **`map`** (albedo/base color) — базовый цвет. **sRGB**.
- **`normalMap`** — имитация рельефа без полигонов. Линейное пространство.
- **`roughnessMap`** / **`metalnessMap`** — шероховатость/металличность. Линейное.
- **`aoMap`** — ambient occlusion (затенение в щелях); требует второго набора UV.
- **`emissiveMap`** — свечение. sRGB.
- **`displacementMap`** — реально смещает вершины (нужна плотная сетка).
- **`alphaMap`** — прозрачность.

### Цветовое пространство — частая ошибка
Цветовые карты (`map`, `emissiveMap`) должны быть в **sRGB**, а карты данных (normal/roughness/metalness/ao) — в **линейном**. Перепутаешь — получишь вымытые или пересвеченные материалы. Плюс `renderer.outputColorSpace` и tone mapping. ⚠ API цветовых пространств менялось между версиями three.js (`encoding` → `colorSpace`) — сверяйся с докой.

### Производительность
- Сжимай в **KTX2/Basis** (GPU-сжатие) вместо PNG/JPEG — меньше и памяти, и времени загрузки.
- Держи разумные размеры (2K вместо 4K там, где не видно разницы), степень двойки для mipmaps.
- Переиспользуй текстуры между материалами; `texture.dispose()` при удалении.
- Асинхронная загрузка + плейсхолдер: см. [[concept-webgl-performance]].

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-materials]] — карты управляют параметрами материала
- [[concept-threejs-geometry]] — UV-координаты
- [[concept-webgl-performance]] — KTX2, размеры, dispose
- [[concept-threejs-gltf]] — текстуры внутри моделей

## Источник

- raw/d3-textures.md — Discover three.js, «Textures Introduction»
