# prefers-reduced-motion

CSS media-фича, отражающая системную настройку «уменьшить движение». Обязательный фолбэк для любого движения в [[method-premium-creative-web]] — иначе анимация вредит части пользователей (вестибулярные расстройства, укачивание).

## Содержание

Пользователь включает «Reduce Motion» в ОС (macOS: Accessibility → Display → Reduce Motion). Сайт обязан это уважать:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Правильный подход — **не убить всё, а дать спокойную версию**: заменить параллакс/большие перемещения и авто-проигрывание на fade/instant; сохранить смысловые микро-переходы. Это прямое проявление эвристики user control ([[concept-usability-heuristics]]).

Практика в стеке:
- **GSAP** — `gsap.matchMedia()` с веткой `(prefers-reduced-motion: reduce)` ([[lib-gsap]]).
- **Framer Motion** — хук `useReducedMotion()` ([[lib-framer-motion]]).
- **Scroll-driven** — отключай scrub/pin-скачки, оставляй мгновенный reveal ([[concept-scroll-driven-animation]]).
- **WebGL** — спокойный фолбэк: меньше движения частиц/камеры ([[concept-webgl-performance]]).

Это не «опция для галочки», а часть того, что отличает премиум от janky.

## Связано с

- [[method-premium-creative-web]] — non-negotiable доступности
- [[concept-usability-heuristics]] — эвристика user control
- [[concept-scroll-driven-animation]] — фолбэк для scroll-анимации
- [[concept-webgl-performance]] — спокойная версия 3D
- [[lib-gsap]] · [[lib-framer-motion]] — API для reduced-motion веток

- [[method-motion-design-craft]] — фолбэк для ambient/parallax-движения

- [[concept-threejs-render-loop]] — спокойный режим 3D-анимации

- [[concept-threejs-controls]] — без автоповорота камеры

- [[method-webgl-performance-degradation]] — спокойный режим R3F через matchMedia
- [[method-motion-production-recipes]] — `MotionConfig reducedMotion` в сниппетах

## Источник

- raw/reduced-motion-webdev.md — web.dev, «prefers-reduced-motion»
