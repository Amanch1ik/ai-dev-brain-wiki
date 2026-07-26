# Anti-AI-tells: дефолты моушна в коде

Конкретные значения по умолчанию в [[lib-framer-motion]]/[[lib-gsap]], которые выдают «сгенерённую» анимацию, и чем их заменить. Кодовый срез к [[method-motion-design-craft]] (там — принципы, здесь — цифры дефолтов).

## Содержание

### Easing: не `easeInOut` на всё
- Дефолтный `ease: "easeInOut"` на каждой анимации = шаблон. Выдаёт, что кривую не выбирали.
- Бери **кастомный cubic-bezier** под смысл движения (см. easing-по-типу-действия в [[method-motion-design-craft]]) — например `[0.22, 1, 0.36, 1]` (ease-out-expo) — или **честный spring**.

### Spring: калибруй damping
- Дефолт `{ stiffness: 100, damping: 10 }` = «bouncy AI»: заметный отскок, читается как демо.
- Премиум: **`damping` 18–30 при `stiffness` 150–300** — упругость без мультяшности. Отскок только там, где он осмыслен (playful UI), не везде.

### Иерархия длительностей (одинаковая 0.3s = tell)
Разные типы движения — разные тайминги:
- **micro** (hover, tap, toggle) — 150–250ms.
- **content reveals** — 400–600ms.
- **page transitions** — 600–900ms.
Одинаковая `duration: 0.3` на всё — первый признак генерации.

### Stagger: не 0.1s+ на большом списке
- `staggerChildren: 0.1` на длинном списке **тянется как демо** (последний элемент появляется поздно).
- Премиум: **0.03–0.06s** + `delayChildren` для отступа старта каскада. Ощущается как один слаженный жест, а не перекличка.

### AnimatePresence: не забывай `mode`
- Без `mode="wait"` (или `"popLayout"`) exit старого и enter нового **накладываются** — сырой overlap, дёрганый переход.
- `mode="wait"` держит порядок: старое ушло → новое пришло.

### Чек-лист «дёшево» (код-уровень)
- `easeInOut`/дефолтный ease на всём.
- `spring` с `damping: 10` (звонкий отскок везде).
- `duration: 0.3` на каждой анимации.
- `staggerChildren: 0.1+` на списке.
- `AnimatePresence` без `mode`.
- `scale: 0 → 1` вместо `0.95 → 1 + opacity`.

## Связано с

- [[method-motion-design-craft]] — принципы easing/тайминга/overshoot (родительский слой)
- [[lib-framer-motion]] — где живут эти дефолты (spring, transition, stagger)
- [[lib-gsap]] — eases и stagger на стороне GSAP
- [[method-motion-production-recipes]] — готовые сниппеты с правильными значениями

## Источник

- motion.dev/docs — дефолты spring/tween/transition
- [[method-motion-design-craft]] — источники по easing и таймингам
