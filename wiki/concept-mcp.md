# Model Context Protocol (MCP)

Стандартный интерфейс, через который LLM взаимодействуют с внешними приложениями: приложение выставляет **tools**, **resources** (контент для контекста) и параметризованные **prompts**. Инициатива Anthropic (ноябрь 2024).

## Содержание

По разбору [[simon-willison]]: MCP даёт единый способ подключать к модели инструменты и данные. Первая рабочая версия — в Claude Desktop (macOS/Windows): приложение запускает дополнительные «серверы» (процессы) и общается с ними по **JSON-RPC через stdin/stdout**. Сервер публикует список tools/resources/prompts; модель может вызвать инструмент или запросить данные.

- SDK: TypeScript и Python (`mcp` на PyPI); репозиторий `modelcontextprotocol/servers` с примерами (любимый у Уиллисона — SQLite-сервер: чтение/запись/создание таблиц в локальной БД).
- Ранняя версия была «неуклюжей» (ручная правка JSON-конфига); спецификация предусматривала следующий шаг — HTTP SSE-транспорт для внешних серверов. Ранние партнёры: Cody (Sourcegraph), Zed.

В таксономии context engineering ([[method-context-engineering]]) MCP-сервер — это **context interface**: программа, дающая агенту доступ к API/данным/действиям; решение о вызове принимает LLM, но сам tool-call обычно детерминирован. Тренд: часть функций MCP-серверов вытесняется **skills**, описывающими, как использовать скрипты и CLI. MCP-серверы вызывает и [[tool-claude-code]].

⚠ Транспорты и возможности MCP быстро эволюционируют — сверяйся с modelcontextprotocol.io.

## Связано с

- [[concept-llm-agent]] — MCP как способ дать агенту инструменты для цикла
- [[method-context-engineering]] — MCP-сервер как «context interface»
- [[tool-claude-code]] — вызывает MCP-серверы
- [[method-spec-driven-development]] — Tessl CLI работает и как MCP-сервер
- [[concept-supply-chain-risk]] — MCP как слабое место без встроенной безопасности
- [[person-armin-ronacher]] — прагматичный «minimal MCP» подход (playwright-mcp)
- [[concept-tools-for-agents]] — MCP-сервер как один из видов инструмента
- [[tool-playwright-mcp]] — конкретный MCP-сервер (браузер)
- [[tool-container-use]] — MCP-сервер контейнерных окружений
- [[reference-mcp-servers]] — официальные референс-серверы
- [[concept-augmented-llm]] — MCP как способ подключить augmentations
- [[concept-agent-computer-interface]] — MCP как документированный интерфейс инструментов
- [[simon-willison]] — автор разбора релиза
- [[tool-modelscope]] — альтернативный способ подключать модели: прямой OpenAI-совместимый API

## Источник

- raw/mcp-simonwillison.md — Simon Willison, «Introducing the Model Context Protocol», 2024-11-25
- https://modelcontextprotocol.io — спецификация и документация
