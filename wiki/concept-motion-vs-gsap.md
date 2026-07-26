# Motion vs GSAP — что выбрать

Решающее дерево: когда брать [[lib-framer-motion]] (Motion), когда [[lib-gsap]], и как совмещать без конфликтов. Обе — движки [[method-premium-creative-web]], но с разной философией.

## Содержание

Это не «кто быстрее», а **разная модель мышления**. Ошибка — вешать обе на `transform` одного элемента: подерутся за одно и то же свойство.

### Бери GSAP, когда
- **Timeline-thinking** — сложные секвенции с точным контролем порядка/перекрытия (`.to().to("<0.2")`).
- **ScrollTrigger scrub** — привязка таймлайна к прогрессу скролла, pin-секции ([[concept-scroll-driven-animation]]).
- **SVG morph** (MorphSVG), **motion path**, **text-split** (SplitText).
- Award-сайты, кинематографичные intro, оркестрованные последовательности вне React-стейта.
- Анимация **не-DOM**: shader uniforms, `three.js`-камера, generic-объекты (вне React-цикла).

### Бери Motion, когда
- Работаешь в **React lifecycle** — движение завязано на стейт компонента.
- Нужна «магия» из коробки: **layout-анимации** (`layout`, `layoutId` shared-element), **AnimatePresence** (exit до размонтирования), **gesture-driven** UI (`whileHover`/`whileTap`/`drag`).
- Микроинтеракции и переходы, где хочется декларативности, а не императивного таймлайна.

### Как совмещать (правильно)
Разделяй по слою владения свойством:
- **GSAP** — на DOM/объекты **вне React-стейта**: scroll-таймлайн секции, shader uniforms, canvas, глобальный intro.
- **Motion** — на **React-компонентах**, где нужны `layout`/`exit`/gesture.
- ⚠ **Никогда оба на `transform` одного узла.** Если GSAP двигает `x`, Motion не должен трогать тот же элемент через `animate`/`layout`.

Практика: hero-скролл-сцена и параллакс — GSAP+ScrollTrigger; карточки/модалки/списки/навигация — Motion. Оба живут в одном проекте без проблем, пока не пересекаются на одном свойстве.

## Связано с

- [[lib-gsap]] — timeline / ScrollTrigger / SVG-morph / text-split
- [[lib-framer-motion]] — layout / AnimatePresence / gesture в React
- [[concept-scroll-driven-animation]] — где GSAP выигрывает (scrub, pin)
- [[method-immersive-web-interaction-patterns]] — оба движка в immersive-сцене
- [[method-motion-production-recipes]] — рецепты, где рецепт отдают тому или иному движку
- [[method-premium-creative-web]] — движение как слой премиум-опыта

## Источник

- semaphore.io — сравнение GSAP vs Framer Motion ⚠ вторичный
- lab.good-fella.com — 2026-сравнение анимационных подходов ⚠ вторичный
- motion.dev/docs · gsap.com/docs
