# Container Use

Open-source MCP-сервер (на Dagger), дающий кодовым агентам **изолированные контейнерные окружения**: каждый агент получает свежий контейнер в собственной git-ветке. Упоминается [[person-armin-ronacher]] как инструмент к наблюдению для параллелизации.

## Содержание

Позволяет уйти от «babysitting одного агента» к нескольким агентам, работающим безопасно и независимо:
- **Isolated Environments** — свежий контейнер + отдельная git-ветка на агента: нет конфликтов, эксперименты безопасны, провал выбрасывается мгновенно. Прямо решает проблему shared-state из [[concept-ai-friendly-codebase]] (параллелизация).
- **Real-time Visibility** — полная история команд и логов: что агенты **реально** делали, а не что заявляют.
- **Direct Intervention** — можно зайти в терминал любого агента и перехватить управление ([[concept-agentic-steering]]).
- **Environment Control** — обычный git: `git checkout <branch>` для ревью работы агента.
- Совместим с любым MCP-агентом (Claude Code, Cursor). Установка: `brew install dagger/tap/container-use`; подключение: `claude mcp add container-use -- container-use stdio`; опционально `rules/agent.md >> CLAUDE.md`. Команда-шорткат — `cu`.

Инфраструктурная опора для [[pattern-writer-reviewer]] и параллельных сессий: изоляция снижает blast radius ([[concept-supply-chain-risk]]).

⚠ Проект experimental, активно меняется — сверяйся с container-use.com.

## Связано с

- [[concept-mcp]] — Container Use как MCP-сервер
- [[person-armin-ronacher]] — назвал его инструментом к наблюдению
- [[concept-ai-friendly-codebase]] — решает shared-state для параллельных агентов
- [[pattern-writer-reviewer]] — изолированные окружения для параллельной работы
- [[concept-supply-chain-risk]] — контейнерная изоляция как least-privilege
- [[reference-mcp-servers]] — соседние MCP-серверы
- [[concept-agentic-steering]] — прямое вмешательство в работу агента
- [[tool-claude-code]] — MCP-клиент, к которому подключается

## Источник

- raw/container-use-readme.md — Dagger, Container Use (README)
