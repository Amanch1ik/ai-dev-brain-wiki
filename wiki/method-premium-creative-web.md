# Премиальный креативный веб

Как делать сайты уровня Awwwards / студий (Active Theory, Lusion, Bonhomme), а не шаблоны: арт-дирекшн, 3D, движение, глубина. Хаб-страница кластера; лежит в основе скилла `premium-creative-frontend` в `~/.claude/skills/`.

## Содержание

**Отказ от шаблонного вида.** Никаких generic hero + 3 карточки + дефолтный шрифт и корпоративный синий. Каждому проекту — своя арт-дирекция: типографическая система, продуманная палитра, фирменный мотив. Референс — Awwwards Site of the Day.

### Техники (брать под задачу, не всё сразу)
- **3D/WebGL** — [[lib-three-js]], в React — [[lib-react-three-fiber]]: hero-сцены, product viewer, частицы, displaced-меши.
- **Кастомные шейдеры (GLSL)** — [[concept-glsl-shaders]]: градиенты, noise, fluid, iridescence — «дорогой» вид часто живёт здесь.
- **Scroll-driven** — [[concept-scroll-driven-animation]]: [[lib-gsap]] ScrollTrigger + [[lib-lenis]] (smooth scroll), pin, scrub, parallax с реальной глубиной.
- **Движение** — GSAP для таймлайнов, Framer Motion для React-переходов; spring вместо линейных ease.
- **Микроинтеракции** — кастомный курсор, магнитные кнопки, hover-дисторшн, text split/scramble, page transitions.
- **Тип и лейаут** — сильный display-шрифт, `clamp()` fluid scale, много воздуха, асимметричные/сломанные сетки.

### Non-negotiables (это отделяет премиум от «дёшево»)
- **Перформанс** — 60fps, lazy-load 3D, сжатые текстуры (KTX2/Basis), cap `dpr` в R3F. Лагающий 3D = дёшево. См. [[concept-webgl-performance]].
- **Доступность** — уважать `prefers-reduced-motion` (спокойный фолбэк), клавиатура, семантика, контраст.
- **Адаптив** — деградировать в упрощённую, но всё ещё крафтовую мобильную версию, не в сломанную десктопную сцену.
- **Детали** — easing, тайминги, loading/empty/focus состояния. Последние 10% — и есть «дорого».

### Дефолтный стек
[[lib-vite]] + React + [[lib-react-three-fiber]] + drei + [[lib-gsap]] (ScrollTrigger) + [[lib-lenis]] + Tailwind.

## Связано с

- [[lib-three-js]] · [[lib-react-three-fiber]] — 3D/WebGL
- [[lib-gsap]] · [[lib-lenis]] — движение и scroll
- [[concept-glsl-shaders]] · [[concept-scroll-driven-animation]] · [[concept-webgl-performance]] — техники
- [[lib-vite]] — build-инструмент стека
- [[lib-tailwind]] — utility-first стилизация
- [[lib-framer-motion]] — движение React-компонентов
- [[lib-drei]] — готовые абстракции для R3F
- [[concept-oklch-color]] — палитра арт-дирекции
- [[concept-usability-heuristics]] — крутизна без потери юзабилити
- [[concept-reduced-motion]] — доступность движения
- [[concept-ai-friendly-codebase]] — выбор стека и рефакторинг под агента

- [[concept-threejs-postprocessing]] — кинематографичный «дорогой» вид
- [[concept-threejs-raycasting]] — микроинтеракции в 3D
- [[tool-modelscope]] — бесплатный Qwen-Image для генерации ассетов лендинга
- [[tool-hyperframes]] — упаковка премиум-моушна в видео (MP4) для презентаций/отчётов
- [[method-motion-design-craft]] — прикладной слой движения: easing, тайминги, overshoot, переходы
- [[method-presentation-design]] — визуальная простота и арт-дирекция в слайдах/питчах
- [[method-voiceover-audio-mix]] — озвучка и аудио-микс промо по тому же стандарту крафта
- [[concept-motion-vs-gsap]] — выбор движка движения под задачу
- [[method-immersive-web-interaction-patterns]] — анатомия и приёмы immersive-опыта
- [[method-webgl-performance-degradation]] — адаптивная деградация и бюджет кадра
- [[method-motion-production-recipes]] — готовые Motion-паттерны премиум-UI
- [[method-webgl-mobile-rn-porting]] — тот же крафт на нативном RN

## Источник

- скилл `~/.claude/skills/premium-creative-frontend/SKILL.md`
- raw/threejs-home.md, raw/r3f-readme.md, raw/gsap-readme.md, raw/lenis-readme.md, raw/book-of-shaders-intro.md
