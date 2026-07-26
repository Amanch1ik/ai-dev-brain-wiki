# Motion (Framer Motion)

Библиотека анимации для React/JS/Vue. Компонентный слой движения для [[method-premium-creative-web]]. С начала 2025 — независимый проект **Motion**: пакет `framer-motion` → `motion`, импорт из `motion/react`.

## Содержание

### Миграция framer-motion → motion
- Пакет переименован: `npm install motion` (не `framer-motion`).
- Импорт: `import { motion } from "motion/react"` (не `framer-motion`).
- Гайд миграции: motion.dev/docs/react-upgrade-guide. ⚠ Старые туториалы всё ещё пишут `framer-motion` — сверяйся с motion.dev.

### Ядро
- **motion-компоненты** — `<motion.div>` с пропами `initial` / `animate` / `exit` / `transition`.
- **variants** — именованные состояния + оркестрация: `staggerChildren`, `delayChildren` (родитель дирижирует детьми).
- **gestures** — `whileHover`, `whileTap`, `drag` (+ `dragConstraints`, `dragElastic`).
- **transition** — `spring` (`stiffness` / `damping` / `mass`) vs `tween` (`duration` / `ease`). Дефолты калибруй — см. [[concept-anti-ai-tells-motion-defaults]].

### Layout-анимации (киллер-фича)
- Проп **`layout`** — при изменении лейаута Motion анимирует переход **через `transform`, а не `width`/`height`** (дёшево, 60fps, без layout-thrash).
- **`layoutId`** — shared-element transition: одинаковый id на двух компонентах → магический переход между ними.
- **`LayoutGroup`** — координирует несколько независимых `layout`-компонентов.
- **`layoutScroll`** — на скролл-контейнерах (учесть скролл при расчёте).
- **`layoutRoot`** — на `position: fixed` элементах.

### AnimatePresence
Держит компонент в DOM **до конца `exit`-анимации** (React иначе размонтирует сразу).
- `mode="wait"` — старое уходит до прихода нового (порядок).
- `mode="popLayout"` — уходящий вынимается из потока (остальные сразу перестраиваются).

### Хуки
- `useScroll` — `scrollYProgress` 0→1 (по странице или `target`-секции).
- `useTransform` — маппинг motion value в другой диапазон/единицы.
- `useMotionValue` — значение вне React-рендера (меняется без ре-рендера → перформанс).
- `useSpring` — сглаживание motion value физикой пружины.

### Accessibility
- `<MotionConfig reducedMotion="user">` — при системном reduce **отключает transform/layout-движение, но оставляет opacity/color** (осмысленный фолбэк, не «всё замерло»). См. [[concept-reduced-motion]].
- `useReducedMotion()` — хук для ручных веток.

### Motion+ / motion-plus
Платный набор дополнений. Например компонент **`<Ticker items axis velocity>`** — готовый marquee с reduced-motion из коробки (+~2.1kb). ⚠ проверить — часть платного motion-plus, не в базовом пакете.

### Bundle size
- Полный импорт `motion` — ~34–46KB gz.
- **`LazyMotion` + `m`** (вместо `motion`) ужимает initial до **~4.6KB**, фичи догружаются лениво.

**Где в стеке:** Motion — для переходов React-компонентов, layout/exit/gesture. [[lib-gsap]] — для таймлайнов и scroll-скраба. Решающее дерево — [[concept-motion-vs-gsap]]. Готовые паттерны — [[method-motion-production-recipes]].

## Связано с

- [[method-premium-creative-web]] — слой движения в React
- [[lib-gsap]] — разделение ролей: Motion (компоненты) vs GSAP (таймлайны/scroll)
- [[concept-motion-vs-gsap]] — решающее дерево выбора
- [[method-motion-production-recipes]] — готовые сниппеты на этом API
- [[concept-anti-ai-tells-motion-defaults]] — калибровка дефолтов spring/ease/stagger
- [[method-motion-design-craft]] — принципы, которые Motion реализует
- [[concept-scroll-driven-animation]] — `useScroll`/`useTransform` в React
- [[concept-reduced-motion]] — `MotionConfig reducedMotion` и `useReducedMotion()`
- [[method-webgl-mobile-rn-porting]] — Reanimated как нативный аналог на RN
- [[method-webgl-performance-degradation]] — CSS/Motion вместо WebGL для дешёвых интеракций

## Источник

- raw/framer-motion-readme.md — motiondivision/motion (README)
- motion.dev/docs · motion.dev/changelog · motion.dev/docs/react-upgrade-guide
