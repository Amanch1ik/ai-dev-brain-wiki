# Motion-design craft (что делает движение «дорогим»)

Профессиональный motion-design для промо, лендингов и kinetic-видео: правильный easing, тайминги, stagger, overshoot и переходы, которые отличают студийную анимацию от «ИИ-дефолта». Прикладной слой движения для [[method-premium-creative-web]] и рендер-видео ([[tool-hyperframes]]).

## Содержание

### Easing по типу действия (Emil Kowalski)
Кривая выбирается **по смыслу движения**, а не «на глаз»:
- **Enter / exit** (появление, исчезновение) → `ease-out`.
- **Move / morph** (перемещение, изменение формы) → `ease-in-out`.
- **Hover** → `ease`.
- **Constant / infinite** (спиннеры, marquee, ambient loop) → `linear`.
- **Никогда** чистый `ease-in` для UI — вход, ускоряющийся к концу, ощущается «засасыванием», это первый AI-tell.

Опорные кривые:
- strong ease-out — `cubic-bezier(0.23, 1, 0.32, 1)`
- strong ease-in-out — `cubic-bezier(0.77, 0, 0.175, 1)`
- iOS-подобная — `cubic-bezier(0.32, 0.72, 0, 1)`

Кастомный bezier > встроенных `ease`/`ease-out`: даёт уникальность почерка и лёгкий overshoot. Встроенные кривые узнаваемы как «дефолт браузера».

### Duration
- Рабочий диапазон **200–500ms** (Val Head).
- UI-микроанимации — **<300ms** (Kowalski): дольше ощущается медленным.
- Для промо / kinetic-сцен допустимо **400–600ms** на сцену — там движение само по себе контент.
- **Exit ≈ 60–70% от enter**: уход быстрее прихода (пользователь уже принял решение).
- Опорные точки восприятия: **100ms** = «мгновенно», **~230ms** = типичное время восприятия перехода, **~1s** = предел «потока мысли» (дольше — внимание рвётся).

### Stagger (каскад)
- UI-группы (карточки, списки) — **30–80ms** между элементами.
- Kinetic type: **40–80ms на символ**, **50–150ms на слово**.
- В GSAP — `stagger: { each, from, ease }`: распределяет тайминги стартов (`from: "center" | "edges" | index), а не длительности. См. [[lib-gsap]].

### Anticipation → Overshoot → Settle
Движение с «замахом» и лёгким перелётом убирает механику:
- GSAP `back.out(1.2–1.7)` для settle с overshoot.
- **Не выше 1.7** для премиума — иначе мультяшно/bouncy.
- Пара overshoot + settle (перелёт и возврат) — то, что читается как «живое», а не «интерполяция от A к B».

### Disney 12 principles в motion graphics
- **Anticipation** — микрозамах перед движением.
- **Slow-in / slow-out** = easing; `linear` там, где нужен ease — первый AI-tell.
- **Staging** — направляй взгляд, один фокус за раз.
- **Secondary action** — вторичные движения, поддерживающие главное.
- **Squash & stretch** — как micro-scale-overshoot (1.0 → 1.03 → 1.0), не буквальная деформация.

### Ambient continuous motion (Smashing Magazine)
Держит сцену/видео живым в паузах: slow, seamless loop, наслоение слабых слоёв движения, organic easing (не линейное дыхание). Обязателен `prefers-reduced-motion`-фолбэк ([[concept-reduced-motion]]). Тонкий ambient-слой — разница между «замерло» и «дышит».

### Parallax — глубина слоями
Разные скорости = ощущение объёма:
- foreground ≈ **1.0**
- midground ≈ **0.5–0.8**
- background ≈ **0.1–0.3**

В вебе это движок scroll-driven-подачи ([[concept-scroll-driven-animation]]).

### Transitions между сценами
- **Fade / dissolve** — нейтральный, всегда работает.
- **Wipe / mask** — брендовый переход (маска логотипом/формой).
- **Whip-pan** — энергия; режь на пике смаза (cut mid-blur).
- **Match-cut** — форма/движение продолжается в следующей сцене.
- **Restraint:** если сам замечаешь переход — его слишком много. Точный timing и matching элементов = признак pro; спецэффект ради эффекта = любительство.

### Anti-AI-tells (чек-лист «дёшево»)
- `linear` как дефолтный easing.
- Всё анимируется разом — без stagger и без неподвижных якорей.
- Нет overshoot / settle — чистая интерполяция.
- `ease-in` на входе.
- Анимация layout-свойств (`width`, `top`, `margin`) вместо `transform`/`opacity` — дёргается и выдаёт неопытность.
- `scale` from 0 вместо `0.95 + opacity` — «выпрыгивает из точки».
- Одинаковый enter и exit.
- Игнор `prefers-reduced-motion`.

## Связано с

- [[method-premium-creative-web]] — движение как слой премиум-опыта в вебе
- [[lib-gsap]] — движок таймлайнов, `stagger`, `back.out`, кастомные eases
- [[tool-hyperframes]] — упаковка этих техник в детерминированный MP4
- [[concept-scroll-driven-animation]] — parallax и scrub как motion-подача по скроллу
- [[concept-reduced-motion]] — обязательный фолбэк ambient/parallax-движения
- [[method-presentation-design]] — motion-consistency в кинетических презентациях
- [[method-voiceover-audio-mix]] — синхронизация движения с озвучкой и битом
- [[concept-anti-ai-tells-motion-defaults]] — те же принципы на уровне дефолтов кода (spring/ease/stagger)
- [[lib-framer-motion]] — Motion реализует эти принципы в React

## Источник

- gsap.com/docs/v3/Eases · gsap.com/resources/getting-started/Staggers
- github.com/emilkowalski/skills — emil-design-eng SKILL.md (easing по типу действия)
- valhead.com/2016/05/05/how-fast-should-your-ui-animations-be — Val Head, duration
- rauno.me/craft/interaction-design — Rauno Freiberg, craft
- schoolofmotion.com/blog/understanding-the-principles-of-anticipation ⚠ вторичный
- m3.material.io/styles/motion — Material 3 motion
- smashingmagazine.com/2025/09/ambient-animations-web-design-principles-implementation
- studiobinder.com/blog/types-of-editing-transitions-in-film ⚠ вторичный
