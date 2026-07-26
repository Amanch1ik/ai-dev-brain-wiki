# HyperFrames

Инструмент от HeyGen (Apache-2.0, TypeScript): HTML-композиция → **детерминированный MP4**. Agent-first — заточен, чтобы LLM генерил анимированные слайды/видео из разметки. Альтернатива [[lib-gsap]]-в-Remotion для видео-вывода.

## Содержание

### Что делает
Парсит HTML-композицию с анимациями, headless Chrome «сикает» по кадрам, FFmpeg кодирует в MP4 + аудио. Принцип **«same input → same frames → same output»** — воспроизводимо, годится для CI/регрессии. Вывод — **видео (MP4)**, не интерактивные HTML-слайды.

### Стек и требования
TypeScript (Puppeteer + FFmpeg). Анимации: **GSAP, CSS, Lottie, Three.js ([[lib-three-js]]), Anime.js, WAAPI**. Нужны **Node 22+** и **FFmpeg**.

### Использование
```bash
npx hyperframes init my-video
npx hyperframes preview    # браузер + live-reload
npx hyperframes render     # → MP4
```

### Claude Code skills
Ставится как набор скиллов (симлинк в Claude Code):
```bash
npx skills add heygen-com/hyperframes --full-depth
```
Ставит `~/.claude/.agents/skills/`: hyperframes-core/cli/animation/keyframes/creative, а также `slideshow`, `motion-graphics`, `product-launch-video`, `faceless-explainer`, `changelog-video`, `motion-doctrine` и др. ⚠ Скиллы исполняют код с полными правами агента — источник (HeyGen) доверенный, но review перед боевым прогоном обязателен.

### Рабочий флоу (по практике энтузиаста)
1. Сформулировать тему.
2. Разложить смыслы по слайдам маркетинг-скиллом: от большой идеи → к деталям.
3. Упаковать через HyperFrames → анимированная презентация в MP4 (подставляется в видео / отчёты).
Хорошо работает в паре с генеративными моделями (Fable, GPT).

### HyperFrames vs Remotion
- **Remotion** — React→видео, компонентная модель.
- **HyperFrames** — HTML + любая аним-либа → видео, детерминизм, **agent-first** (проще для LLM-генерации). Есть скилл `remotion-to-hyperframes` для миграции.

## Связано с

- [[method-premium-creative-web]] — премиум-моушн, из которого собираются слайды
- [[lib-gsap]] — основная аним-либа для композиций HyperFrames
- [[lib-three-js]] — 3D-контент в кадре
- [[concept-scroll-driven-animation]] — родственная моушн-техника (веб vs видео-рендер)
- [[method-motion-design-craft]] — motion-приёмы для композиций (easing, stagger, переходы)
- [[method-presentation-design]] — структура и сторителлинг слайдов, собираемых в MP4
- [[method-voiceover-audio-mix]] — озвучка и аудио-дорожка промо-видео

## Источник

- https://github.com/heygen-com/hyperframes — HeyGen, Apache-2.0
