# Scroll-driven анимация

Опыт, где скролл управляет анимацией: pin-секции, scrub-таймлайны, reveal-по-скроллу, parallax с реальной глубиной. Сигнатурный приём awards-сайтов в [[method-premium-creative-web]].

## Содержание

Технический стек приёма:
- **[[lib-gsap]] ScrollTrigger** — движок: привязывает таймлайны к прогрессу скролла (`scrub`), «пинит» секции (`pin`), триггерит reveal на входе в вьюпорт.
- **[[lib-lenis]]** — плавный скролл поверх нативного; синхронизирует GSAP и WebGL-сцену ([[lib-three-js]]) на одном RAF-цикле, чтобы 3D и DOM двигались в такт.
- Нативная альтернатива — CSS Scroll-driven Animations (`animation-timeline: scroll()/view()`), но поддержка и контроль пока ограниченнее GSAP.

Приёмы: horizontal scroll секции, sticky-повествование (текст закреплён, меняется визуал), scrub-скраб видео/3D-камеры по скроллу, staggered reveal, parallax слоями с разной скоростью.

**Обязательно:** `prefers-reduced-motion` фолбэк (спокойная версия без резких движений) и производительность — тяжёлые scroll-хендлеры и layout-thrash убивают 60fps (см. [[concept-webgl-performance]]). `gsap.matchMedia()` помогает делать адаптивно и доступно.

## Связано с

- [[lib-gsap]] — ScrollTrigger как движок
- [[lib-lenis]] — плавный scroll и синхронизация
- [[concept-webgl-performance]] — держать 60fps при scroll-сценах
- [[lib-framer-motion]] — scroll-linked эффекты в React
- [[lib-drei]] — `ScrollControls` для 3D по скроллу
- [[concept-reduced-motion]] — фолбэк без резких движений
- [[method-premium-creative-web]] — сигнатурный приём премиум-веба

- [[concept-threejs-controls]] — отключать контролы в scroll-сценах
- [[tool-hyperframes]] — родственный вывод: те же аним-либы, но в видео
- [[method-motion-design-craft]] — parallax-глубина и scrub как motion-подача по скроллу
- [[concept-motion-vs-gsap]] — scrub/pin как зона GSAP
- [[method-immersive-web-interaction-patterns]] — scroll как таймлайн 3D-сцены
- [[method-motion-production-recipes]] — parallax и scroll-linked spring на Motion
- [[method-webgl-mobile-rn-porting]] — scroll-driven на Reanimated (useAnimatedScrollHandler)

## Источник

- raw/gsap-readme.md — ScrollTrigger
- raw/lenis-readme.md — smooth scroll и sync
