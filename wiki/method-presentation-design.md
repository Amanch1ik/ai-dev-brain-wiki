# Дизайн презентаций и сторителлинг

Как делать профессиональные слайды и питчи: одна идея на слайд, assertion-заголовки, визуальная простота, нарративная арка и «дорогие» переходы. Слой контента/структуры для промо-презентаций и kinetic-видео ([[tool-hyperframes]]).

## Содержание

### Один слайд — одна идея
- **Одна мысль на слайд** (PLOS «Ten Simple Rules for Effective Presentation Slides»).
- Заголовок = **вывод-утверждение** (assertion-evidence), а не ярлык темы: не «Результаты», а «Конверсия выросла на 40% после редизайна».
- **≤6 элементов** на слайд.
- **Cognitive load:** нельзя одновременно читать текст и слушать спикера — работает один канал. Слайд поддерживает речь, а не дублирует её.

### Правила беглого восприятия
- **3-second rule** (Duarte, *slide:ology*): смысл слайда считывается за 3 секунды. «Distracted person test» — отвлёкшийся зритель должен вернуться и мгновенно понять, где он.
- **10/20/30** (Guy Kawasaki): 10 слайдов / 20 минут / шрифт ≥ **30pt**.
- **TED-стандарт:** шрифт **≥42pt**, без bullet-списков, консистентный визуальный стиль по всей деке.

### Визуальный язык (Presentation Zen, Reynolds + Tufte)
- **Simplicity** и **signal-to-noise** — убрать всё, что не несёт смысла.
- **Picture superiority** — образ запоминается лучше слов; **full-bleed** изображения.
- **Contrast** и **rule of thirds** — композиция, а не «текст по центру».
- Тёплый цвет = акцент, холодный = фон.
- **Tufte data-ink:** максимум смысла на каплю чернил — убрать 3D-тени, лишние gridlines, декоративные рамки графиков.

### Сторителлинг (Duarte)
- **Big Idea** = одно полное предложение с твоим POV (point of view) + stakes (что на кону). Не тема, а утверждение с позицией.
- **Sparkline** — структура колебания между «what is» (как есть) ↔ «what could be» (как могло бы быть); финал — «new bliss».
- **3-act:** setup → confrontation → resolution.
- **Визуальный язык должен меняться** между блоком «проблема» и блоком «видение» — тон, цвет, плотность.

### Pitch-deck (Sequoia)
Каждый слайд отвечает на один вопрос инвестора:
Purpose → Problem → Solution → Why Now → Market Size → Product → Team → Business Model → Competition → Financials.

### Переходы: дорого vs дёшево
- **Дорого:** Fade / Cut / Morph (целевой Morph 1–2с), длительность **≤0.5с**, **один тип перехода на всю деку**.
- **Дёшево:** Vortex, Origami, Reveal, Curtains, Glitter; смешение разных типов переходов.
- Консистентность перехода = тот же принцип restraint, что и в [[method-motion-design-craft]].

### Кинетические видео-презентации
Для рендер-видео (не live-доклад):
- **Sequential reveal** — элементы появляются по смыслу, а не разом.
- **Motion consistency** — единое направление и почерк движения по всей деке.
- **Sync** с озвучкой / битом ([[method-voiceover-audio-mix]]).
- Консервативный совет PLOS «избегай анимаций» относится к **live-докладам на чужом железе** (риск лагов, несовместимости), а не к детерминированному рендер-видео, где анимация — часть продукта.

## Связано с

- [[method-motion-design-craft]] — motion-consistency, sequential reveal, переходы
- [[method-voiceover-audio-mix]] — синхронизация слайдов с озвучкой и музыкой
- [[tool-hyperframes]] — сборка слайдов в MP4 (от большой идеи → к деталям по слайдам)
- [[method-premium-creative-web]] — визуальная простота и арт-дирекция как общий принцип
- [[concept-usability-heuristics]] — минимализм, recognition-over-recall в подаче слайдов

## Источник

- journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009554 — «Ten Simple Rules for Effective Presentation Slides»
- guykawasaki.com/the_102030_rule — Guy Kawasaki, 10/20/30
- garrreynolds.com/design-tips — Garr Reynolds, Presentation Zen
- duarte.com/resources/books/slideology — Nancy Duarte, slide:ology (3-second rule)
- duarte.com/blog/ultimate-guide-to-contrast · duarte.com/blog/how-to-develop-the-best-big-idea-for-your-presentation
- easyvc.ai/blog/sequoia-capital-pitch-deck-template — Sequoia pitch template ⚠ вторичный
- deckary.com/blog/powerpoint-transitions — переходы дорого/дёшево ⚠ вторичный
- linearity.io/blog/kinetic-typography — kinetic typography ⚠ вторичный
