# Claude Code

Агентный инструмент разработки от Anthropic, работающий в терминале (и как расширение IDE / веб). Реализует паттерн [[concept-llm-agent]] — «инструменты в цикле» — и поддерживает sub-agent режимы.

## Содержание

[[simon-willison]] приводит Claude Code как пример того, почему в определение агента не стоит зашивать «цель, поставленную пользователем»: здесь встречается **sub-agent паттерн**, когда одна LLM ставит цель другой LLM (аналогично Claude Research / multi-agent research).

Практически Claude Code — это харнесс, дающий модели инструменты (чтение/запись файлов, запуск команд, поиск, вызовы MCP-серверов) и прогоняющий их в ограниченном цикле до достижения цели. Это делает его пригодным как для [[method-ai-assisted-programming]] (с ревью правок), так и, при желании, для [[concept-vibe-coding]]. Он стоит в ряду инструментов, реально запускающих код, рядом с [[tool-aider]] и Agent-режимом [[tool-cursor]].

### Реальный пример (Simon Willison)

Уиллисон построил страницу-colophon для своего репозитория tools, используя Claude Code на локальной машине по «авторитарному» процессу ([[prompt-function-signature]], точные инструкции):
- Первый заход: скрипт `gather_links.py` → правки через диалог → `build_colophon.py`. Итог по команде `/cost`: **$0.61, ~17 минут**. Часть кода он не читал вовсе — «pure vibe-coding».
- Второй заход (новая сессия, [[method-context-management]] «clean slate»): кастомный GitHub Pages build через Actions — **$0.18, 45 сек** API. Здесь он **следил** за действиями внимательно, т.к. не знал area и опасался галлюцинаций.
- Грабли: пришлось «бросить LLM и почитать доку» (двойной деплой Jekyll) — иллюстрация принципа «будь готов, что человек возьмёт управление».

⚠ Набор возможностей, модели и команды регулярно обновляются — сверяйся с официальной документацией.

## Связано с

- [[concept-llm-agent]] — Claude Code как реализация «tools in a loop» и sub-agent паттерна
- [[simon-willison]] — привёл его как пример и подробно описал сессию
- [[method-ai-assisted-programming]] — типичный режим использования с ревью
- [[prompt-function-signature]] — «авторитарный» стиль промптинга в примере
- [[method-context-management]] — приём «clean slate» между сессиями
- [[tool-aider]] · [[tool-cursor]] — соседние инструменты того же класса
- [[concept-agent-memory]] — инструментальная модель памяти Claude
- [[method-context-engineering]] — лидер по фичам контекста (CLAUDE.md, skills, subagents, hooks)
- [[concept-mcp]] — вызывает MCP-серверы как context interface
- [[person-armin-ronacher]] — детальный полевой workflow на Claude Code
- [[concept-tools-for-agents]] — гоняет инструменты, спроектированные по этим правилам
- [[concept-agentic-engineering]] — движется к оркестрации субагентов
- [[method-claude-code-workflow]] — каноничные best practices работы с ним
- [[concept-claude-md]] — читает CLAUDE.md каждую сессию
- [[concept-context-window]] — главное ограничение (/clear, /compact, субагенты)
- [[pattern-writer-reviewer]] — worktrees, agent teams, `/code-review`
- [[concept-workflows-vs-agents]] — Claude Code как coding-агент
- [[tool-playwright-mcp]] · [[tool-container-use]] — MCP-серверы, которые он подключает
- [[tool-claude-subagents]] — воркеры внутри сессии: конфигурация и лимиты
- [[tool-claude-headless]] — `claude -p` для CI, cron и своих ферм
- [[method-parallel-claude-terminals]] — worktrees, фоновые сессии, agent teams
- [[method-token-economy]] — сколько это стоит и как удешевить
- [[tool-modelscope]] — внешняя площадка бесплатных модельных API как дополнение к Claude

## Источник

- raw/what-is-an-agent-simonwillison.md — Simon Willison, 2025-09-18
- https://docs.claude.com/en/docs/claude-code — официальная документация
