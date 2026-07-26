# Армин Ронахер

Armin Ronacher (mitsuhiko) — создатель Flask и Jinja, автор блога lucumr.pocoo.org. В этой базе — источник конкретных, «полевых» практик агентного кодинга.

## Содержание

Его сетап (лето 2025): преимущественно **Claude Code** на Max ($100/мес), исключительно модель **Sonnet** (предпочитает её вывод Opus), оптимизация под token-efficiency (избегает скриншотов и браузера). Workflow: выдать агенту задачу с полными правами и почти не вмешиваться; роль IDE снижена (вернулся к Vim). Запускает `claude --dangerously-skip-permissions` (алиас `claude-yolo`), риск снижает Docker'ом.

Из его статьи выросли страницы:
- [[concept-tools-for-agents]] — «anything can be a tool» и правила дизайна инструментов.
- [[concept-ai-friendly-codebase]] — «write simple code», стабильные экосистемы, Go для бэкенда.

Прагматичный взгляд на [[concept-mcp]]: почти не использует, т.к. Claude Code хорошо гоняет обычные инструменты (`psql`); MCP берёт только когда альтернатива слишком ненадёжна (пример — playwright-mcp для браузера). «MCP-серверы сами бывают ненадёжны — лишняя точка отказа».

Оговорка самого автора: пост «состарится очень быстро», поэтому он держится за концепции с «staying power»: **простота, стабильность, observability, разумная параллелизация**.

## Связано с

- [[concept-tools-for-agents]] — его правила дизайна инструментов для агента
- [[concept-ai-friendly-codebase]] — его принципы простого кода под агентов
- [[concept-agentic-engineering]] — его workflow «задача агенту → жди»
- [[tool-claude-code]] — его основной инструмент (Sonnet, yolo-режим)
- [[concept-mcp]] — его прагматичный «minimal MCP» подход
- [[tool-playwright-mcp]] — MCP-сервер, который он использует для браузера
- [[tool-container-use]] — назвал инструментом к наблюдению для параллелизации
- [[lib-tanstack-query]] · [[lib-vite]] · [[lib-tailwind]] — библиотеки его фронтенд-стека
- [[reference-mcp-servers]] — его прагматика «многое делается без MCP»

## Источник

- raw/agentic-coding-ronacher.md — Armin Ronacher, «Agentic Coding Recommendations», 2025-06-12
