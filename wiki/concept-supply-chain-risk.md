# Риск цепочки поставок с агентами

Агентные ассистенты (Cursor, Windsurf, Cline, Copilot) расширяют attack surface: они не просто подсказывают код, а действуют в среде разработчика через tool-use / ReAct-циклы — и могут быть скомпрометированы сами. Разбор из серии [[martin-fowler]].

## Содержание

Среда разработчика давно слабое звено supply chain (повышенные привилегии, прямая интеграция с продакшном). Агенты добавляют новые измерения.

### Attack surface цикла агента
- **Context poisoning** — вредоносные ответы внешних инструментов/API запускают нежелательное поведение, усиливаясь через петли обратной связи.
- **Escalation of privilege** — скомпрометированный (особенно слабо контролируемый) ассистент выполняет опасные команды прямо через свой execution flow: произвольные команды, правка конфигов/исходников, внедрение заражённых зависимостей. С учётом локальных привилегий разработчика — пивот в продакшн.

### Новые слабые места
- **MCP-серверы** ([[concept-mcp]]): по умолчанию **нет** встроенной аутентификации, шифрования контекста и проверки целостности инструментов — «S in MCP stands for Security».
- **Rules-файлы** (например cursor rules): ещё один слой, куда можно инъектировать вредоносные промпты. Часть [[method-context-engineering]] — и одновременно вектор атаки.

### Защита (во многом традиционные практики)
- **Sandboxing + least privilege** — ограничивай привилегии агента, sandbox уменьшает blast radius (ср. [[tool-claude-artifacts]]).
- **Supply chain scrutiny** — вычитывай MCP-серверы и rules-файлы как критические зависимости.
- **Monitoring/observability** — логируй изменения ФС, сетевые вызовы к MCP, модификации зависимостей.
- **Threat modeling** — явно включай workflow ассистента.
- **Human in the loop** — auto-accept резко расширяет простор для вреда. Быстрая генерация → approval fatigue → неявное доверие; overconfidence и [[concept-vibe-coding]] повышают риск уязвимостей.

## Связано с

- [[concept-mcp]] — MCP как слабое место без встроенной безопасности
- [[method-context-engineering]] — rules-файлы как вектор инъекций
- [[concept-vibe-coding]] — auto-accept и approval fatigue повышают риск
- [[concept-llm-agent]] — attack surface цикла «tools in a loop»
- [[concept-harness-engineering]] — линтеры/тесты/мониторинг как контроль
- [[tool-claude-artifacts]] — sandboxing как снижение blast radius
- [[concept-agentic-engineering]] — рост автоматизации умножает точки риска
- [[tool-container-use]] — контейнерная изоляция как least-privilege
- [[reference-mcp-servers]] — reference ≠ production, оценивай безопасность
- [[martin-fowler]] · [[source-exploring-gen-ai]] — источник

## Источник

- raw/supply-chain-risk-fowler.md — «Coding Assistants Threaten the Software Supply Chain», серия Exploring Gen AI
