# Reference MCP-серверы

Официальные **референс-реализации** MCP от steering group (репозиторий `modelcontextprotocol/servers`) — образцы фич и SDK, а не production-решения. Каталог конкретных [[concept-mcp]]-серверов.

## Содержание

> ⚠ Это образовательные примеры; для production оценивай безопасность под свой threat model (см. [[concept-supply-chain-risk]]). Полный список опубликованных серверов — в MCP Registry.

### Reference-серверы
- **Everything** — тестовый сервер с prompts, resources, tools.
- **Fetch** — загрузка и конвертация веб-контента для эффективного потребления LLM (аналог `defuddle` в этой базе).
- **Filesystem** — безопасные файловые операции с настраиваемым доступом.
- **Git** — чтение, поиск, манипуляции в git-репозиториях.
- **Memory** — персистентная память на основе knowledge graph (перекликается с [[concept-agent-memory]] и идеей memory bank из [[concept-ai-swe]]).
- **Sequential Thinking** — рефлексивное решение задач через последовательности мыслей.
- **Time** — время и конвертация таймзон.

Запуск: TypeScript-серверы через `npx -y @modelcontextprotocol/server-memory`; Python — через `uvx`. SDK есть под C#, Go, Java, Kotlin, PHP, Python, Ruby, Rust, Swift, TypeScript.

Архивированы (перенесены): GitHub, GitLab, Google Drive/Maps, PostgreSQL, Puppeteer, Redis, Sentry, Slack, SQLite, Brave Search — часть заменена официальными сторонними серверами.

Прагматика: Ронахер ([[person-armin-ronacher]]) замечает, что многие такие функции агент делает и без MCP (обычный `psql`, `git`), а тренд — вытеснять MCP-серверы skills/CLI ([[concept-context-window]] — токен-эффективность).

## Связано с

- [[concept-mcp]] — протокол, который эти серверы реализуют
- [[tool-playwright-mcp]] · [[tool-container-use]] — конкретные соседние серверы
- [[concept-tools-for-agents]] — серверы как инструменты агента
- [[concept-agent-memory]] — Memory-сервер как persistent memory
- [[concept-supply-chain-risk]] — reference ≠ production, оценивай безопасность

## Источник

- raw/mcp-servers-readme.md — modelcontextprotocol/servers (README)
