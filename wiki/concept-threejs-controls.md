# Three.js: управление камерой

Готовые контролы избавляют от ручной математики камеры. Базовый и самый частый — **OrbitControls**: вращение вокруг точки, зум, панорамирование.

## Содержание

```js
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const controls = new OrbitControls(camera, renderer.domElement);
controls.target.set(0, 1, 0);      // точка, вокруг которой вращаемся
controls.enableDamping = true;      // инерция — ощущается дороже, чем резкие рывки
controls.dampingFactor = 0.05;
controls.minDistance = 2;  controls.maxDistance = 20;
controls.maxPolarAngle = Math.PI / 2;   // не давать уйти под пол

// ⚠ при enableDamping ОБЯЗАТЕЛЬНО в цикле:
controls.update();
```

### Набор контролов
| Контрол | Для чего |
|---|---|
| **OrbitControls** | осмотр объекта/сцены (продуктовые вьюеры) — дефолт |
| `MapControls` | навигация «по карте» (пан вместо орбиты) |
| `TrackballControls` | свободное вращение без «верха» |
| `FlyControls` / `FirstPersonControls` | полёт/от первого лица |
| `PointerLockControls` | шутерное управление с захватом курсора |
| `TransformControls` | гизмо для перемещения объектов (редакторы) |
| `DragControls` | перетаскивание мешей мышью |

Отдельно — библиотека **camera-controls** (yomotsu): плавные анимированные перелёты камеры, `fitToBox`, лучше для «кинематографичных» переходов, чем OrbitControls.

### Практика
- `enableDamping` + `controls.update()` в цикле ([[concept-threejs-render-loop]]) — почти всегда включать: плавность читается как качество.
- Ограничивай `min/maxDistance` и полярный угол, иначе пользователь улетит внутрь модели или под пол.
- `controls.enabled = false` на время scroll-driven сцен ([[concept-scroll-driven-animation]]), чтобы контролы не воевали со скроллом.
- Для reduced-motion убирай автоповорот (`autoRotate`) — [[concept-reduced-motion]].
- Контролы конфликтуют с [[concept-threejs-raycasting]] по событиям указателя — следи за порядком обработчиков.

⚠ Путь импорта менялся: `three/examples/jsm/controls/…` → `three/addons/controls/…`. Сверяйся с версией ([[concept-training-cutoff]]).

В React — `<OrbitControls />` и `<CameraControls />` из [[lib-drei]].

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-scene-graph]] — управляем камерой сцены
- [[concept-threejs-render-loop]] — `controls.update()` каждый кадр
- [[concept-threejs-raycasting]] — общий поток событий указателя
- [[lib-drei]] — готовые React-обёртки контролов
- [[concept-scroll-driven-animation]] — отключать контролы в scroll-сценах
- [[concept-reduced-motion]] — без автоповорота
- [[concept-training-cutoff]] — пути импорта менялись

## Источник

- Публичный API three.js (addons/controls) — стабильная часть библиотеки
- raw/drei-readme.md — pmndrs/drei (React-обёртки контролов)
