# Index — AI Dev Brain

Живое оглавление инженерной базы. Обновляется после каждой обработки `raw/`. Правила — в [AGENTS.md](AGENTS.md).

Формат: `[[имя-файла]] — одна строка описания`.

## Темы

### AI / агенты и вайбкодинг

- [[concept-vibe-coding]] — построение софта с LLM без ревью кода; когда это уместно; способ учиться
- [[method-ai-assisted-programming]] — ответственное использование LLM: ментальная модель, тесты, take-over
- [[concept-llm-agent]] — «LLM-агент выполняет инструменты в цикле для достижения цели»
- [[concept-agent-memory]] — память агента: инструментальная (Claude) vs автоматическая (ChatGPT)
- [[tool-modelscope]] — площадка Alibaba: бесплатный API к Qwen/DeepSeek/Qwen-Image (лимиты + real-name verification)
- [[concept-mcp]] — Model Context Protocol: стандарт подключения инструментов к LLM
- [[concept-agentic-steering]] — роль навыков разработчика; три радиуса ошибок агента
- [[concept-ai-risk-assessment]] — Probability × Impact × Detectability; шкала усилий на ревью
- [[concept-harness-engineering]] — «упряжь» для агентов: guides и sensors
- [[concept-supply-chain-risk]] — attack surface агентов: context poisoning, MCP, rules-файлы
- [[concept-agentic-engineering]] — агенты как компоненты процесса; ассистент vs агент; оркестрация
- [[concept-workflows-vs-agents]] — workflows (заданные пути) vs agents (динамическое управление)
- [[concept-augmented-llm]] — базовый блок: LLM + retrieval + tools + memory
- [[concept-agent-computer-interface]] — ACI: промпт-инжиниринг определений инструментов
- [[concept-ai-swe]] — системная методология AI SWE и memory bank
- [[concept-tools-for-agents]] — «anything is a tool»; правила дизайна инструментов
- [[concept-ai-friendly-codebase]] — простой стабильный код под агентов; выбор языка; рефакторинг

### Мультиагентность и оркестрация

- [[method-agent-orchestration]] — решающее правило «read vs write», бриф воркера, верификация, масштаб усилия
- [[concept-multi-agent-failure-modes]] — Cognition vs Anthropic, таксономия MAST, 14 режимов отказа с частотами
- [[tool-claude-subagents]] — субагенты Claude Code: frontmatter, изоляция контекста, лимиты, fork
- [[method-parallel-claude-terminals]] — worktrees, фоновые сессии, agent teams, tmux-ферма, OSS-оркестраторы
- [[tool-claude-headless]] — `claude -p` для CI, cron и собственного fan-out
- [[method-token-economy]] — кэш префикса, роутинг моделей, цена веера (4×/15×), измерение расходов

### Методики и промпт-приёмы

- [[method-claude-code-workflow]] — best practices Anthropic: verify-work, explore→plan→code, сессии
- [[concept-context-window]] — главное ограничение агента; /clear, /compact, субагенты
- [[concept-claude-md]] — как писать эффективный CLAUDE.md (что включать/исключать)
- [[pattern-writer-reviewer]] — writer/reviewer и adversarial-ревью свежим контекстом
- [[pattern-agent-workflows]] — 5 паттернов: chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer
- [[method-context-management]] — «context is king»: clean slate, seeding, примеры как топливо
- [[method-context-engineering]] — конфигурация контекста инструментами (rules, skills, MCP, hooks)
- [[method-spec-driven-development]] — «documentation first»: spec-first/anchored/as-source, Kiro/spec-kit/Tessl
- [[method-tdd]] — TDD с AI-ассистентами: обратная связь и divide-and-conquer
- [[prompt-function-signature]] — диктовать точную сигнатуру функции для продакшн-кода
- [[concept-training-cutoff]] — дата обучения модели и выбор «boring» библиотек
- [[method-partner-throw-away-code]] — парная работа с AI на legacy: проси объяснить код

## Премиальный креативный веб (3D / интерактив)

- [[method-premium-creative-web]] — как делать awards-уровень, а не шаблоны (хаб)
- [[lib-three-js]] — 3D/WebGL в браузере **(хаб изучения three.js ↓)**
  - [[concept-threejs-scene-graph]] — сцена, Object3D, камера, трансформации
  - [[concept-threejs-geometry]] — BufferGeometry, примитивы, атрибуты, инстансинг
  - [[concept-threejs-materials]] — типы материалов и PBR (metalness/roughness)
  - [[concept-threejs-lights]] — источники света, тени, HDRI-окружение
  - [[concept-threejs-textures]] — карты, UV, цветовые пространства, KTX2
  - [[concept-threejs-render-loop]] — цикл рендера, delta, ресайз
  - [[concept-threejs-gltf]] — загрузка моделей glTF/GLB, Draco
  - [[concept-threejs-animation]] — AnimationMixer, скелет, морфы
  - [[concept-threejs-postprocessing]] — bloom, DOF, эффекты
  - [[concept-threejs-raycasting]] — клики и hover по 3D
  - [[concept-threejs-controls]] — OrbitControls и управление камерой
- [[lib-react-three-fiber]] — three.js декларативно в React
- [[lib-gsap]] — анимация + ScrollTrigger
- [[tool-hyperframes]] — HTML→MP4, детерминированный рендер анимированных презентаций (HeyGen)
- [[lib-lenis]] — smooth scroll, синхронизация с WebGL
- [[concept-glsl-shaders]] — GLSL-шейдеры, «дорогой» визуал
- [[concept-scroll-driven-animation]] — scroll-driven опыт
- [[concept-webgl-performance]] — 60fps, чтобы «дорого», а не «лагает»
- [[lib-drei]] — готовые абстракции для R3F
- [[lib-framer-motion]] — движение React-компонентов (Motion, `motion/react`)
- [[concept-motion-vs-gsap]] — решающее дерево: Motion vs GSAP и как совмещать
- [[method-motion-production-recipes]] — продакшн-сниппеты на Motion (page transition, magnetic, parallax…)
- [[concept-anti-ai-tells-motion-defaults]] — дефолты spring/ease/stagger, выдающие «генерацию»
- [[method-immersive-web-interaction-patterns]] — анатомия immersive-опыта и приёмы эталонов
- [[method-webgl-performance-degradation]] — on-demand рендер, динамика качества, mobile, WebGL vs CSS
- [[method-webgl-mobile-rn-porting]] — перенос web-моушна/WebGL на React Native (Reanimated + Skia)
- [[lib-tailwind]] — utility-first CSS
- [[concept-oklch-color]] — OKLCH-палитра для дизайн-систем
- [[concept-usability-heuristics]] — 10 эвристик Нильсена
- [[concept-reduced-motion]] — доступность движения (prefers-reduced-motion)

## Промо, motion и видео-контент

- [[method-motion-design-craft]] — что делает движение «дорогим»: easing, тайминги, stagger, overshoot, переходы, anti-AI-tells
- [[method-presentation-design]] — одна идея на слайд, assertion-заголовки, сторителлинг Duarte, дорогие переходы
- [[method-voiceover-audio-mix]] — озвучка (SSML/voice-settings), LUFS-нормализация, ducking, EQ, анти-роботность

## Библиотеки и инструменты стека

- [[reference-mcp-servers]] — официальные MCP-серверы (Fetch, Filesystem, Git, Memory, …)
- [[tool-container-use]] — контейнерные окружения для параллельных агентов (MCP/Dagger)
- [[lib-tanstack-query]] — управление server-state во фронтенде
- [[lib-vite]] — быстрый build-инструмент (dev-сервер + HMR)
- [[lib-zod]] — TypeScript-first валидация схем

## Сущности

- [[simon-willison]] — практик AI-assisted programming, источник большинства определений
- [[andrej-karpathy]] — автор термина vibe coding
- [[martin-fowler]] — куратор серии Thoughtworks об GenAI в разработке
- [[person-armin-ronacher]] — создатель Flask; полевые практики агентного кодинга
- [[tool-cursor]] — AI-редактор кода (Composer)
- [[tool-claude-code]] — агентный dev-инструмент Anthropic (tools in a loop, subagents, MCP)
- [[tool-aider]] — ведущий open-source агент кодинга (dogfooding)
- [[tool-github-copilot]] — ассистент кодинга GitHub (практика TDD)
- [[tool-claude-artifacts]] — песочница Claude как модель безопасного вайбкодинга
- [[tool-playwright-mcp]] — MCP-сервер браузерной автоматизации (accessibility tree)

## Источники

- raw/vibe-coding-simonwillison.md — Simon Willison, «Not all AI-assisted programming is vibe coding», 2025-03-19
- raw/using-llms-for-code-simonwillison.md — Simon Willison, «Here's how I use LLMs to help me write code», 2025-03-11
- raw/what-is-an-agent-simonwillison.md — Simon Willison, определение агента, 2025-09-18
- raw/tools-in-a-loop-simonwillison.md — Simon Willison, цитата Hannah Moran (Anthropic), 2025-05-22
- raw/claude-memory-simonwillison.md — Simon Willison, философия памяти Claude, 2025-09-12
- raw/mcp-simonwillison.md — Simon Willison, «Introducing the Model Context Protocol», 2024-11-25
- [[source-exploring-gen-ai]] — Martin Fowler / Thoughtworks, серия memo (SDD, context engineering, TDD, dev skills + очередь)
- raw/agentic-coding-ronacher.md — Armin Ronacher, «Agentic Coding Recommendations», 2025-06-12
- raw/vibecoding-to-agentic-habr.md · raw/ai-agents-cursor-claude-habr.md — Habr, агентный инжиниринг (RU), 2026
- raw/claude-code-best-practices-anthropic.md — Anthropic, «Claude Code best practices» (текст доклада Code w/ Claude, Cal Rueb)
- raw/building-effective-agents-anthropic.md — Anthropic (Erik S., Barry Zhang), «Building Effective Agents»
- raw/ai-swe-systematic-habr.md — Habr, «AI Software Engineering: от хаоса Vibe Coding к системной разработке»
- raw/playwright-mcp-readme.md — Microsoft, Playwright MCP (README)
- raw/mcp-servers-readme.md · raw/container-use-readme.md — MCP-серверы (GitHub)
- raw/tanstack-query-readme.md · raw/vite-readme.md · raw/zod-docs.md — библиотеки стека
