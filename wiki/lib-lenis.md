# Lenis

Лёгкая, производительная библиотека **smooth scroll** от darkroom.engineering («lenis» = «плавный» на латыни). Ключевой слой scroll-опыта в [[method-premium-creative-web]].

## Содержание

Особенности (из доки):
- **Lightweight & dependency-free** — пара КБ, ноль рантайм-зависимостей.
- **Runs on native scroll** — оборачивает нативный скролл браузера, поэтому `position: sticky`, anchor-ссылки и доступность продолжают работать (важное отличие от «фейкового» transform-скролла).
- **Any axis** — вертикальный/горизонтальный/вложенный из одного инстанса.
- **Built for sync** — гоняет WebGL-scroll-сцены ([[lib-three-js]]), [[lib-gsap]] ScrollTrigger и parallax из одного loop → плавная синхронизация 3D и скролла.
- **Framework adapters** — пакеты для React, Vue, Framer.
- **Scroll snapping** — плагин snap выравнивает секции, не воюя со smooth scroll.

Типичная связка: Lenis (плавный скролл) → синхронизирует [[concept-scroll-driven-animation]] и WebGL-сцену на одном RAF-цикле. ⚠ Есть considerations/limitations (см. README) — например, взаимодействие с нативными scroll-снапами и position:fixed.

## Связано с

- [[concept-scroll-driven-animation]] — Lenis как основа плавного scroll-опыта
- [[lib-gsap]] — ScrollTrigger синхронизируется с Lenis
- [[lib-three-js]] — WebGL-сцены, привязанные к скроллу
- [[concept-webgl-performance]] — единый RAF-цикл для 60fps
- [[method-premium-creative-web]] — слой scroll-опыта стека

- [[concept-threejs-render-loop]] — общий RAF со сценой

## Источник

- raw/lenis-readme.md — darkroomengineering/lenis (README)
