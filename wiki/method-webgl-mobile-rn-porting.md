# Перенос web-моушна/WebGL на React Native

Что из web-анимации переносится в нативный React Native (Reanimated + Skia), а что нет. Прикладная заметка под мобильный проект на Expo/RN/TS.

## Содержание

### Что переносится
- **Spring/gesture-моушн** → **Reanimated 3**: worklets исполняются на UI-thread (60fps без прыжков через JS-мост). Магнитные элементы, drag, inertia, shared-value анимации — прямой аналог `useMotionValue`/`useSpring` из [[lib-framer-motion]].
- **Шейдеры** → **React Native Skia**: SkSL-шейдеры (диалект, близкий к GLSL) под жест — runtime-эффекты, дисторшн, градиенты. Ближайший нативный аналог [[concept-glsl-shaders]].
- **Экспериментально** — **Skia Graphite на WebGPU**: цель — приблизить копипаст web/three.js-примеров в нативный рантайм (общий графический бэкенд).

### Что НЕ переносится 1-в-1
- **GLSL под WebGL2 API** — Skia использует **SkSL**, не WebGL: синтаксис близок, но API байндинга uniform'ов/текстур другой. Переписывать, не копировать.
- **ScrollTrigger** ([[lib-gsap]]) → нет; scroll-driven собирается через `useAnimatedScrollHandler` (Reanimated) + `interpolate` по `scrollY`. См. [[concept-scroll-driven-animation]].
- **R3F raycaster** ([[lib-react-three-fiber]]) → нет; hit-testing через `react-native-gesture-handler` + Skia touch-координаты, не через `onPointerOver`/[[concept-threejs-raycasting]].

### Реалистично для мобильного приложения
Не тянуть полноценную R3F-сцену в приложение бронирования автобусов. Достаточно:
- **Reanimated spring** для магнитных/тактильных элементов (кнопки, карточки, переходы).
- **Один Skia-момент** для wow-splash (анимированный логотип/шейдер), про который в памяти проекта.
- Деградация и бюджет кадра — те же принципы, что в [[method-webgl-performance-degradation]] (мобайл — первичная цель, не десктоп).

## Связано с

- [[method-webgl-performance-degradation]] — бюджет кадра на мобиле
- [[concept-glsl-shaders]] → SkSL как нативный аналог
- [[lib-framer-motion]] — Reanimated как нативный аналог spring/motion-value
- [[lib-gsap]] — ScrollTrigger → useAnimatedScrollHandler
- [[concept-scroll-driven-animation]] — scroll-driven на Reanimated
- [[lib-react-three-fiber]] — R3F raycaster → gesture-handler + Skia touch
- [[concept-threejs-raycasting]] — почему hit-testing переписывается
- [[method-premium-creative-web]] — тот же стандарт крафта на нативе

## Источник

- shopify.engineering — WebGPU / Skia web graphics (Graphite) ⚠ вторичный
- docs.swmansion.com/react-native-reanimated · shopify.github.io/react-native-skia
