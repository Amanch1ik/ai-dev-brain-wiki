# Three.js: постпроцессинг

Полноэкранная обработка отрендеренного кадра: **bloom**, depth of field, вигнетка, хроматическая аберрация, шум, SSAO. Именно постобработка чаще всего создаёт «дорогой» кинематографичный вид в [[method-premium-creative-web]].

## Содержание

Модель: **passes** (проходы) и **effects** (эффекты) поверх обычного рендера. Библиотека `postprocessing` (pmndrs) — более быстрая и богатая альтернатива встроенному `EffectComposer` из three.js: объединяет несколько эффектов в один проход.

```js
import { BloomEffect, EffectComposer, EffectPass, RenderPass } from "postprocessing";

const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));      // первый проход: рисует сцену
composer.addPass(new EffectPass(camera, new BloomEffect()));

// в цикле рендерим композер, а не renderer:
composer.render();
```

### Настройка рендерера под постпроцессинг
```js
const renderer = new WebGLRenderer({
  powerPreference: "high-performance",
  antialias: false,   // AA делает постпроцессинг (SMAA), а не браузер
  stencil: false,
  depth: false
});
```
`RenderPass` первым — он чистит буферы и рендерит сцену для дальнейшей обработки. Несколько эффектов складывай в **один** `EffectPass` — так они мержатся в один шейдер и это заметно дешевле, чем цепочка отдельных проходов.

### Цена и практика
Постобработка работает **на каждый пиксель** — это прямая нагрузка на GPU ([[concept-webgl-performance]] и [[concept-glsl-shaders]]). Практика:
- Bloom и DOF — самые «дорогие»; на мобильных снижай разрешение эффекта или отключай.
- Не включай эффекты «на всякий случай»: 2–3 точных эффекта выглядят лучше и дешевле, чем стопка.
- Свои эффекты пишутся как фрагментные шейдеры — вход в кастомный визуал.

В React: `@react-three/postprocessing` (обёртка той же библиотеки) — `<EffectComposer><Bloom/></EffectComposer>`.

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-webgl-performance]] — постобработка как главный потребитель GPU
- [[concept-glsl-shaders]] — свои эффекты = фрагментные шейдеры
- [[lib-react-three-fiber]] — обёртка для React
- [[method-premium-creative-web]] — кинематографичный «дорогой» вид
- [[concept-threejs-render-loop]] — рендерим `composer`, а не `renderer`

## Источник

- raw/postprocessing-readme.md — pmndrs/postprocessing (README)
