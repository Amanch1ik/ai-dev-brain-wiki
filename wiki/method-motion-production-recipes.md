# Motion: продакшн-рецепты (сниппеты)

Готовые паттерны на [[lib-framer-motion]] (`motion/react`) для типовых задач премиум-UI. Значения откалиброваны против [[concept-anti-ai-tells-motion-defaults]].

## Содержание

### Page transition
`AnimatePresence` + `key` по маршруту:
```jsx
<AnimatePresence mode="wait">
  <motion.div key={pathname}
    initial={{ opacity: 0, y: 12 }}
    animate={{ opacity: 1, y: 0 }}
    exit={{ opacity: 0, y: -12 }}
    transition={{ duration: 0.5, ease: [0.22, 1, 0.36, 1] }} />
</AnimatePresence>
```

### Magnetic button
`useMotionValue` + `useSpring`, притяжение в пределах радиуса:
```jsx
const x = useMotionValue(0), y = useMotionValue(0);
const sx = useSpring(x, { stiffness: 200, damping: 20 });
const sy = useSpring(y, { stiffness: 200, damping: 20 });
function onMove(e) {
  const r = ref.current.getBoundingClientRect();
  const dx = e.clientX - (r.left + r.width / 2);
  const dy = e.clientY - (r.top + r.height / 2);
  const MAX = 40; // clamp по MAX_DISTANCE
  x.set(Math.max(-MAX, Math.min(MAX, dx)));
  y.set(Math.max(-MAX, Math.min(MAX, dy)));
}
// onMouseLeave → x.set(0); y.set(0)
// <motion.button style={{ x: sx, y: sy }} />
```

### Scroll-reveal stagger
Контейнер задаёт каскад, дети наследуют:
```jsx
const container = { hidden: {}, show: { transition: { staggerChildren: 0.08, delayChildren: 0.1 } } };
const item = { hidden: { opacity: 0, y: 16 }, show: { opacity: 1, y: 0 } };
// <motion.ul variants={container} initial="hidden" whileInView="show" viewport={{ once: true }}>
//   {items.map(i => <motion.li key={i} variants={item} />)}
```

### Shared-element transition
Одинаковый `layoutId` на превью и детали — Motion сам интерполирует между ними:
```jsx
// в списке:   <motion.img layoutId={`cover-${id}`} />
// в детали:   <motion.img layoutId={`cover-${id}`} />
```

### Drag-to-reorder
`layout` + `drag="y"` внутри `LayoutGroup`:
```jsx
<motion.li layout drag="y" dragConstraints={containerRef} dragElastic={0.15} />
```

### Parallax
`useScroll` целится в секцию, `useTransform` мапит на слои:
```jsx
const { scrollYProgress } = useScroll({ target: ref, offset: ["start end", "end start"] });
const yBg = useTransform(scrollYProgress, [0, 1], ["0%", "40%"]);
// <motion.div style={{ y: yBg }} />
```

### Count-up
`useMotionValue` + `animate` + `onUpdate`:
```jsx
const mv = useMotionValue(0);
useEffect(() => {
  const c = animate(mv, 1234, { duration: 1.2, ease: "easeOut",
    onUpdate: v => el.current.textContent = Math.round(v).toLocaleString() });
  return () => c.stop();
}, []);
```

### Marquee
`animate` x по кругу линейно (или `<Ticker>` из motion-plus):
```jsx
<motion.div animate={{ x: ["0%", "-50%"] }}
  transition={{ repeat: Infinity, ease: "linear", duration: 20 }} />
```

### Scroll-linked spring smoothing
Сгладить рывки `scrollYProgress`:
```jsx
const { scrollYProgress } = useScroll();
const smooth = useSpring(scrollYProgress, { stiffness: 100, damping: 30, restDelta: 0.001 });
```

Для всех: уважать [[concept-reduced-motion]] через `<MotionConfig reducedMotion="user">` или `useReducedMotion()`.

## Связано с

- [[lib-framer-motion]] — API за всеми рецептами
- [[concept-anti-ai-tells-motion-defaults]] — почему именно такие значения
- [[concept-motion-vs-gsap]] — когда рецепт лучше отдать GSAP
- [[concept-scroll-driven-animation]] — parallax / scroll-linked smoothing
- [[concept-reduced-motion]] — обязательный фолбэк
- [[method-premium-creative-web]] — микроинтеракции премиум-UI

## Источник

- motion.dev/docs — useScroll, useTransform, useSpring, AnimatePresence, layout
- motion.dev/docs/react-motion-value — motion values
