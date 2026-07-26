# GSAP (GreenSock Animation Platform)

Framework-agnostic JS-библиотека анимации: высокопроизводительный «property manipulator», обновляющий значения во времени с высокой точностью. Ядро движения в [[method-premium-creative-web]].

## Содержание

Анимирует всё, к чему может прикоснуться JS: CSS, SVG, canvas, WebGL, цвета, строки, motion paths, generic-объекты. Работает во всех браузерах, обходя их несовместимости — «animations just work». Zero dependencies, до 20× быстрее jQuery.

Ключевое для премиум-веба:
- **ScrollTrigger** — scroll-based анимации минимальным кодом: pin, scrub, reveal, parallax. Основа [[concept-scroll-driven-animation]] (часто в паре с [[lib-lenis]]).
- **Продвинутое sequencing** — таймлайны с точным контролем порядка/перекрытия.
- **`gsap.matchMedia()`** — адаптивные и accessibility-friendly анимации (в т.ч. `prefers-reduced-motion`).
- Плагины: **SplitText** (text split/scramble), MorphSVG, MotionPath, **Flip**, Observer (нормализация событий), Draggable.

Установка: `npm install gsap` (или CDN). ⚠ Версии/плагины меняются — сверяйся с gsap.com/docs.

## Связано с

- [[concept-scroll-driven-animation]] — ScrollTrigger как её движок
- [[lib-lenis]] — smooth scroll, синхронизируемый со ScrollTrigger
- [[lib-framer-motion]] — разделение ролей: GSAP (таймлайны/scroll) vs Motion (компоненты)
- [[concept-reduced-motion]] — `gsap.matchMedia()` для reduced-motion
- [[method-premium-creative-web]] — движение как слой премиум-опыта

- [[concept-threejs-render-loop]] — синхронизация в одном RAF

- [[concept-threejs-animation]] — предпочтительно для анимации three.js из кода
- [[tool-hyperframes]] — рендер GSAP-композиций в детерминированный MP4
- [[method-motion-design-craft]] — `stagger`, `back.out`, кастомные eases как приёмы «дорогого» движения
- [[concept-motion-vs-gsap]] — когда GSAP, когда Motion; как совмещать без конфликтов
- [[method-immersive-web-interaction-patterns]] — твининг shader-uniform'ов, ScrollTrigger scrub
- [[concept-anti-ai-tells-motion-defaults]] — eases и stagger, выдающие «дефолт»
- [[method-webgl-mobile-rn-porting]] — ScrollTrigger → useAnimatedScrollHandler на RN

## Источник

- raw/gsap-readme.md — greensock/GSAP (README)
