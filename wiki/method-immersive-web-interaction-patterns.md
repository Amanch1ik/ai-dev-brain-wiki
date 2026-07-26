# Immersive-web: анатомия и приёмы интеракции

Из чего собирается «immersive»-опыт уровня Active Theory / Lusion и как воспроизводить его приёмы. Прикладной интерактивный слой [[method-premium-creative-web]].

## Содержание

### Анатомия immersive
Ощущение «дорогой живой сцены» = сумма пяти слоёв:
1. **WebGL/3D** как холст ([[lib-three-js]] / [[lib-react-three-fiber]]).
2. **Курсор как сигнал** — не просто указатель, а участник сцены (искажает, притягивает, реагирует).
3. **Физика сглаживания** — `lerp`/`spring` вместо резких скачков (инерция, плавное догоняние).
4. **Shader-искажение** — bulge/displacement/noise во vertex/fragment ([[concept-glsl-shaders]]).
5. **Scroll как таймлайн** — прогресс скролла управляет камерой/uniform'ами ([[concept-scroll-driven-animation]]).

Стек: [[lib-react-three-fiber]] + [[lib-drei]] + [[lib-gsap]]/ScrollTrigger + GLSL.

### Приёмы эталонов
- **Active Theory** — mask-video reveal; bulge vertex-shader вокруг курсора; **glow через увеличенную геометрию** (дубликат меша чуть больше + эмиссия) вместо дорогого bloom-pass; флаги/ткань анимируются во vertex shader.
- **Lusion** — cloth-симуляция на ~4096 вершинах; хранение состояния в 16-bit внутри шейдера; click-hold переключает cinematic-режим.
- **Bonhomme** — типографика живёт **внутри** 3D-сцены; курсор — участник композиции.
- **Igloo** — particle simulation перестраивается по hover: `InstancedMesh` + custom attribute на каждую частицу.

### Codrops-тьюториалы (воспроизводимые рецепты)
- **WebGL Distortion Hover** — 2 картинки + displacement-текстура, микс по прогрессу hover.
- **Bulge Distortion** — выпуклость под курсором (часто на OGL).
- **Grid Displacement + RGB Shift** — GPGPU-сетка смещения + хроматическая аберрация.
- **Animating Shaders with GSAP** — GSAP твинит `uniform`-значения → ripple/reveal по клику.

### Практика реализации (R3F)
- **Hover** — `onPointerOver`/`onPointerOut` на меше + `useCursor()` из drei (меняет CSS-курсор). R3F делает raycasting под капотом ([[concept-threejs-raycasting]]).
- **Pointer-lerp** — в `useFrame` догонять целевую позицию: `current = MathUtils.lerp(current, target, 0.1)` ([[concept-threejs-render-loop]]).
- **Scroll-driven uniform** — ScrollTrigger `scrub` пишет `progress` → в `useFrame` читаешь и гонишь uniform/камеру.

### Дешёвые drei-хелперы (без ручных шейдеров)
Когда «дорогой» вид нужен быстро и без GLSL:
- `MeshDistortMaterial` — `distort` + `speed` (органическая деформация меша).
- `MeshWobbleMaterial` — `factor` + `speed` (мягкое покачивание).
- `Float` — `floatIntensity` / `rotationIntensity` / `speed` (левитация объекта).

Это стартовая точка; кастомный shader даёт уникальность, хелперы — скорость. Держи бюджет кадра — см. [[method-webgl-performance-degradation]].

## Связано с

- [[method-premium-creative-web]] — интерактивный слой премиум-веба
- [[lib-react-three-fiber]] — декларативная сцена и события
- [[lib-drei]] — `useCursor`, `MeshDistortMaterial`, `Float`
- [[concept-glsl-shaders]] — bulge/displacement/distortion
- [[lib-gsap]] — твининг uniform'ов, ScrollTrigger scrub
- [[concept-motion-vs-gsap]] — GSAP на сцене vs Motion на React-UI в одном проекте
- [[concept-scroll-driven-animation]] — scroll как таймлайн сцены
- [[concept-threejs-raycasting]] — hover/click через onPointerOver
- [[concept-threejs-render-loop]] — pointer-lerp в useFrame
- [[method-webgl-performance-degradation]] — бюджет кадра immersive-сцены
- [[lib-three-js]] · [[concept-webgl-performance]] — фундамент и перформанс

## Источник

- medium.com/active-theory — разборы приёмов Active Theory ⚠ вторичный
- awwwards.com — кейсы Lusion / Bonhomme / Igloo ⚠ вторичный (косвенно)
- tympanus.net/codrops — тьюториалы (distortion hover, bulge, grid displacement, shaders+GSAP)
- blog.maximeheckel.com — R3F/shader-паттерны
