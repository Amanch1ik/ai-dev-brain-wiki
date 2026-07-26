# Three.js: raycasting и интерактив

Как узнать, на какой 3D-объект пользователь навёл или кликнул: пускаем **луч** из камеры через позицию курсора и смотрим, что он пересёк. Основа всех кликов, hover и drag в 3D.

## Содержание

```js
const raycaster = new THREE.Raycaster();
const pointer = new THREE.Vector2();

window.addEventListener('pointermove', (e) => {
  // ⚠ нормализованные координаты устройства: X и Y в диапазоне -1..1, Y инвертирован
  pointer.x =  (e.clientX / window.innerWidth)  * 2 - 1;
  pointer.y = -(e.clientY / window.innerHeight) * 2 + 1;
});

function pick() {
  raycaster.setFromCamera(pointer, camera);
  const hits = raycaster.intersectObjects(scene.children, true); // true = рекурсивно по детям
  if (hits.length) {
    const { object, point, distance, face, uv } = hits[0];  // ближайший — первый
    // object — что попало, point — точка попадания в мировых координатах, uv — координата текстуры
  }
}
```

### Что даёт пересечение
Массив отсортирован **по возрастанию расстояния**, поэтому `hits[0]` — ближайший объект. Внутри: `object` (меш), `point` (мировая точка), `distance`, `face` (полигон), `uv` (текстурная координата — можно «рисовать» по модели).

### Практика и грабли
- **Не гоняй raycast каждый кадр по всей сцене** — это дорого. Вызывай на событие (`pointermove` с троттлингом) или ограничивай список: `intersectObjects([меши_кандидаты])` вместо `scene.children`.
- Исключай из проверки то, что кликать нельзя (`object.raycast = () => {}` или отдельный массив/слой).
- Курсор → `canvas`-координаты, а не `window`, если канвас не на весь экран (используй `getBoundingClientRect`).
- Для сложной геометрии полезен bounding-box/сфера предварительной проверкой.
- **UX:** давай визуальный отклик на hover (подсветка, курсор `pointer`) — это эвристика «видимость состояния системы» из [[concept-usability-heuristics]]; такие микроинтеракции — часть [[method-premium-creative-web]].

В React всё это встроено: в [[lib-react-three-fiber]] у мешей есть `onClick`, `onPointerOver`, `onPointerOut` — R3F делает raycasting под капотом.

⚠ Импорты/утилиты three.js между версиями меняются (`three/addons/…`) — сверяйся с актуальной докой ([[concept-training-cutoff]]).

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-scene-graph]] — по чему пускаем луч
- [[concept-threejs-render-loop]] — когда вызывать raycast
- [[lib-react-three-fiber]] — `onClick`/`onPointerOver` поверх raycasting
- [[concept-usability-heuristics]] — отклик на наведение
- [[method-premium-creative-web]] — микроинтеракции в 3D
- [[concept-training-cutoff]] — версии API

- [[concept-threejs-controls]] — общий поток событий указателя

- [[method-immersive-web-interaction-patterns]] — hover через onPointerOver + useCursor
- [[method-webgl-mobile-rn-porting]] — hit-testing на RN через gesture-handler + Skia touch

## Источник

- Публичный API three.js (`Raycaster`) — стабильная часть библиотеки
- raw/r3f-readme.md — R3F (события мешей)
