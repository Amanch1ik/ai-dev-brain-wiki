# Саймон Уиллисон

Simon Willison — разработчик, соавтор Django, автор Datasette и блога simonwillison.net, один из самых цитируемых практиков AI-assisted programming. В этой базе — ключевой источник по вайбкодингу и определению агентов.

## Содержание

Через его тексты в базу пришли два опорных определения:
- **[[concept-vibe-coding]]** — «building software with an LLM without reviewing the code it writes»; провёл границу между вайбкодингом и ответственной разработкой ([[method-ai-assisted-programming]]), сформулировал «золотое правило» о коммите только понятного кода.
- **[[concept-llm-agent]]** — «An LLM agent runs tools in a loop to achieve a goal»; отверг определения «агент = замена сотрудника» и раскритиковал разнобой в терминологии OpenAI.

Активно вайбкодит сам: опубликовал 80+ экспериментов (`simonw/tools`), построенных этим способом, как инструмент наращивания интуиции о возможностях LLM. Его развёрнутый разбор процесса — источник принципов «context is king», диктовки сигнатур, учёта training cutoff и памяти агента.

## Связано с

- [[andrej-karpathy]] — прокомментировал и уточнил введённый им термин
- [[concept-vibe-coding]] — уточнил определение
- [[method-ai-assisted-programming]] — автор «золотого правила» и разбора процесса
- [[concept-llm-agent]] — автор принятого определения агента
- [[method-context-management]] — принцип «context is king»
- [[prompt-function-signature]] — приём диктовки сигнатуры
- [[concept-training-cutoff]] — учёт даты обучения при выборе библиотек
- [[concept-agent-memory]] — разбор философии памяти Claude vs ChatGPT
- [[concept-mcp]] — разбор релиза Model Context Protocol
- [[tool-claude-code]] — привёл как пример и описал сессию
- [[tool-aider]] · [[tool-claude-artifacts]] · [[tool-cursor]] — разобрал по критерию безопасности

## Источник

- raw/vibe-coding-simonwillison.md — 2025-03-19
- raw/what-is-an-agent-simonwillison.md — 2025-09-18
- raw/using-llms-for-code-simonwillison.md — 2025-03-11
- raw/claude-memory-simonwillison.md — 2025-09-12
- raw/tools-in-a-loop-simonwillison.md — 2025-05-22
- https://simonwillison.net
