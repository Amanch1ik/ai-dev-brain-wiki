# Context engineering для агентов

«Курирование того, что видит модель, ради лучшего результата» (определение Bharani Subramaniam, Thoughtworks). Дисциплина настройки контекста кодинг-агента. Обзор [[martin-fowler]].

## Содержание

Отличие от [[method-context-management]] (разговорный приём «context is king» у Уиллисона): здесь речь о **конфигурационных фичах инструментов** и стратегии их использования.

### Категории контекста

**Reusable prompts** (markdown-файлы):
- **Instructions** — «сделай то-то» («напиши E2E-тест так: …»).
- **Guidance** (rules, guardrails) — общие конвенции («всегда пиши независимые тесты»).

**Context interfaces** — описания того, как модель может добрать контекст сама:
- **Tools** — встроенные (bash, поиск файлов).
- **MCP-серверы** — доступ к данным/действиям через [[concept-mcp]].
- **Skills** — ресурсы/инструкции/скрипты, которые LLM подгружает по требованию, если сочтёт релевантным.
- Файлы воркспейса — базовый и мощнейший интерфейс; важна «AI-friendly» структура кода.

### Кто решает загрузить контекст
- **LLM** (пример — skills): предпосылка для неконтролируемых агентов, но есть неопределённость, подгрузит ли.
- **Человек** (slash-команды): контроль ценой автоматизации.
- **Софт агента** (Claude Code hooks): детерминированные точки.

### Сколько — как можно меньше
Большое окно ≠ надо всё туда вываливать: эффективность падает и растёт стоимость. Рекомендация — наращивать rules-файлы **постепенно**. Критична прозрачность (`/context` в Claude Code показывает, что сколько занимает). Часть оптимизации — на стороне инструмента (компактизация истории, Tool Search Tool).

### Фичи Claude Code (пример, янв 2026)
`CLAUDE.md` (guidance, всегда в начале) · `Rules` (модульная guidance, скоуп по путям) · `Slash commands` (instructions, человек; вытесняются skills) · `Skills` (lazy-load, LLM/человек) · `Subagents` (свой контекст, другая модель, параллелизм) · `MCP servers` · `Hooks` (скрипты на события) · `Plugins` (дистрибуция всего этого). Есть попытка стандартизировать rules-файл как `AGENTS.md`.

> **Illusion of control:** несмотря на название, это не совсем «инженерия» — исполнение зависит от того, как LLM интерпретирует инструкции. «Ensure» и «prevent hallucinations» невозможны; думай в вероятностях и выбирай уровень надзора.

## Связано с

- [[method-context-management]] — разговорный приём «context is king»
- [[concept-mcp]] — MCP-серверы как context interface
- [[tool-claude-code]] — лидер по фичам context engineering (CLAUDE.md, skills, subagents, hooks)
- [[method-spec-driven-development]] — специализированный вид контекста (спеки, memory bank)
- [[concept-harness-engineering]] — context engineering как компонент harness
- [[concept-supply-chain-risk]] — rules-файлы как вектор инъекций
- [[tool-github-copilot]] — среди ассистентов, перенимающих rules/skills
- [[concept-tools-for-agents]] — инженерное качество инструментов как context interface
- [[concept-claude-md]] — CLAUDE.md как always-loaded guidance
- [[concept-context-window]] — цель: держать контекст маленьким
- [[method-claude-code-workflow]] — настройка окружения на практике
- [[concept-ai-swe]] — формализация правил как memory bank
- [[martin-fowler]] · [[source-exploring-gen-ai]] — источник обзора
- [[method-token-economy]] — конфигурация контекста с точки зрения стоимости

## Источник

- raw/context-engineering-fowler.md — Martin Fowler, «Context Engineering for Coding Agents»
