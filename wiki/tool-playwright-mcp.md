# Playwright MCP

MCP-сервер от Microsoft, дающий агентам браузерную автоматизацию через Playwright. Ключевая идея: LLM взаимодействует со страницей по **structured accessibility tree**, а не по скриншотам — без vision-моделей.

## Содержание

- **Быстрый и лёгкий:** использует accessibility-дерево Playwright, а не пиксельный ввод.
- **LLM-friendly:** работает на структурированных данных, детерминированное применение инструментов (меньше двусмысленности, чем при скриншотах).
- Требует Node.js 18+; клиент — VS Code, Cursor, Claude Desktop, Goose и любой MCP-клиент. Стандартный конфиг: `npx @playwright/mcp@latest` в `mcpServers`.

### MCP vs CLI+SKILLS (важный нюанс)
Сама доко признаёт: современные **coding-агенты всё чаще предпочитают CLI+SKILLS вместо MCP** — CLI-вызовы токен-эффективнее (не грузят большие tool-схемы и многословные accessibility-деревья в контекст). Это перекликается с [[concept-context-window]] и прагматизмом [[person-armin-ronacher]] («minimal MCP»). MCP остаётся уместен для специализированных агентных циклов с persistent state и итеративным reasoning по структуре страницы (self-healing тесты, долгие автономные workflow). Пример конкретного [[concept-mcp]]-сервера как [[concept-tools-for-agents]].

## Связано с

- [[concept-mcp]] — playwright-mcp как конкретный MCP-сервер
- [[concept-tools-for-agents]] — браузер как инструмент агента
- [[concept-context-window]] — почему CLI+SKILLS часто токен-эффективнее MCP
- [[person-armin-ronacher]] — использует playwright-mcp для браузера
- [[tool-claude-code]] — один из MCP-клиентов
- [[reference-mcp-servers]] — соседние официальные MCP-серверы

## Источник

- raw/playwright-mcp-readme.md — Microsoft, Playwright MCP (README)
