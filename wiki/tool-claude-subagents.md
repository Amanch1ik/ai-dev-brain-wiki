# Субагенты Claude Code

Воркеры внутри одной сессии: свой context window, свой system prompt, свой набор инструментов; наверх возвращают **только summary**. Основной инструмент экономии контекста в [[tool-claude-code]].

## Содержание

### Где живут и как разрешаются имена

| Расположение | Scope | Приоритет |
|---|---|---|
| Managed settings `.claude/agents/` | Организация | 1 (высший) |
| CLI-флаг `--agents` (JSON) | Текущая сессия | 2 |
| `.claude/agents/` | Проект | 3 |
| `~/.claude/agents/` | Пользователь, все проекты | 4 |
| `agents/` внутри плагина | Где включён плагин | 5 (низший) |

Обе директории сканируются **рекурсивно**; подпапки на идентичность не влияют — её задаёт только поле `name`. Project-агенты ищутся вверх по дереву от cwd до корня репозитория; при конфликте имён побеждает определение, ближайшее к cwd. Директории под **hot-reload** — правки подхватываются за секунды без рестарта; рестарт нужен, только если папки `agents/` не существовало на старте сессии.

### Полная схема frontmatter

Обязательны только `name` и `description`.

```markdown
---
name: code-reviewer                 # lowercase + дефисы; hooks видят это как agent_type
description: Expert code review specialist. Use proactively after code changes.
tools: Read, Grep, Glob, Bash       # allowlist; опущено — наследует все
disallowedTools: Write, Edit        # denylist; применяется ПЕРВЫМ, затем резолвится tools
model: sonnet                       # sonnet|opus|haiku|fable|inherit (default inherit)
permissionMode: default             # default|acceptEdits|auto|dontAsk|plan|manual|...
maxTurns: 20
skills:                             # ПОЛНЫЙ контент скиллов инжектится в контекст на старте
  - api-conventions
mcpServers:                         # inline-определение ИЛИ ссылка по имени
  - github
hooks:                              # lifecycle-хуки, живут пока агент активен
  PreToolUse:
    - matcher: "Bash"
      hooks: [{ type: command, command: "./scripts/validate.sh" }]
memory: project                     # user|project|local — персистентная память между сессиями
background: true                    # всегда фоном
effort: high                        # low|medium|high|xhigh|max
isolation: worktree                 # временный git worktree = изолированная копия репо
color: cyan
initialPrompt: "..."                # авто-первый turn, только когда агент = main session (--agent)
---

Системный промпт агента — в теле markdown.
```

Семантика, о которую спотыкаются:
- `disallowedTools` применяется **до** `tools`. Инструмент в обоих списках — удаляется. Оба принимают MCP-паттерны `mcp__<server>__*`, denylist ещё и `mcp__*`.
- Если **ни один** элемент `tools` не резолвится в реальный инструмент — субагент не запускается с ошибкой-перечислением.
- Субагентам **недоступны** даже при перечислении: `AskUserQuestion`, `EndConversation`, `EnterPlanMode`, `ExitPlanMode`, `ScheduleWakeup`.
- `Agent` **наследуется** → субагент может спавнить вложенных.
- Резолв модели по порядку: `CLAUDE_CODE_SUBAGENT_MODEL` → per-invocation `model` → frontmatter → модель главной сессии.
- Plugin-субагенты **игнорируют** `hooks`, `mcpServers`, `permissionMode` (безопасность). Родительский `bypassPermissions`/`acceptEdits` **перебивает** `permissionMode` субагента.
- `memory` живёт в `~/.claude/agent-memory/<name>/`; в system prompt инжектится первые 200 строк или 25 KB `MEMORY.md`.

### Что именно попадает внутрь субагента

**Получает:** свой system prompt + environment details; task message, который пишет главный Claude; **всю иерархию CLAUDE.md**; git status snapshot; полный контент скиллов из `skills:`.

**НЕ получает:** историю разговора родителя, его tool results, его system prompt, auto memory. Context window сайзится по **своей** модели.

`cwd` = cwd главной сессии; `cd` внутри субагента **не персистит** между Bash-вызовами. Транскрипты — в `~/.claude/projects/{project}/{sessionId}/subagents/agent-{agentId}.jsonl`, переживают компакцию главного разговора.

### Built-in субагенты

| Агент | Модель | Инструменты | Особенность |
|---|---|---|---|
| **Explore** | inherit | read-only, Write/Edit запрещены | Пропускает CLAUDE.md и git status. Принимает thoroughness `quick`/`medium`/`very thorough`. One-shot, не резюмится |
| **Plan** | inherit | read-only | Используется в plan mode, тоже one-shot |
| **general-purpose** | inherit | все | Сложные многошаговые задачи с правками |
| statusline-setup / claude-code-guide | Sonnet / Haiku | — | служебные |

Свой `~/.claude/agents/Explore.md` **переопределяет** встроенный — например, чтобы вернуть дешёвую модель. Отключение: `"permissions": {"deny": ["Agent(Explore)"]}` или `CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS=1`. Deny самого `Agent` запрещает любое делегирование.

### Как триггерится делегирование

1. **Автоматически** — Claude матчит задачу против поля `description`. Формулировки вида *«Use proactively after…»* усиливают срабатывание. Поэтому `description` пишется для **машины-роутера**, а не для человека.
2. **Естественным языком:** `Use the test-runner subagent to fix failing tests` — обычно срабатывает, но не гарантированно.
3. **@-mention гарантирует** конкретного агента: `@agent-code-reviewer`. Контролирует только *кого*, не *что* — промпт всё равно пишет главный Claude.
4. **Вся сессия как агент:** `claude --agent code-reviewer` — system prompt субагента **полностью заменяет** дефолтный.

### Инструмент `Agent` (бывший `Task`)

Переименован в v2.1.63; `Task(...)` работает как алиас. В SDK `tool_use` эмитит `"Agent"`, но `system:init` и `permission_denials[].tool_name` всё ещё содержат `"Task"` — **проверяй оба значения**.

Параметры: `subagent_type` (имя агента), `prompt` (**единственный канал передачи контекста от родителя**), `description`, `run_in_background`, `model`, `isolation: "worktree"`, `name`.

Родитель **не видит** промежуточных tool calls воркера — только финальный текст. Сам вызов `Agent` не требует permission; проверяются вызовы инструментов внутри.

**Fan-out** делается несколькими `Agent`-блоками в одном assistant-сообщении. Триггер-формулировка: *«Research the authentication, database, and API modules in parallel using separate subagents»*.

### Лимиты (актуальные)

- **Глубина вложенности ≤ 5** уровней; на пятом `Agent` не выдаётся. Не конфигурируется.
- **200 субагентов на сессию** (`CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION` поднимает). `/clear` сбрасывает счётчик.
- **200 вызовов WebSearch на сессию**, считая всех субагентов — существенно для research fan-out.
- **Жёсткого лимита одновременных субагентов в сессии официально нет.** Единственный задокументированный concurrency-cap — в dynamic workflows: **16 concurrent, 1000 на run**.
  ⚠ Ходовая цифра «до 10 параллельных, остальные в очередь» **докой не подтверждена** — это community lore; issues с просьбой добавить `maxParallelAgents` косвенно подтверждают, что настройки нет.

### Foreground vs background

С v2.1.198 субагенты **по умолчанию фоновые**. Background-субагент пробрасывает permission prompt в главную сессию с указанием, кто просит; `Esc` = deny одного вызова без убийства воркера. `Ctrl+B` отправляет задачу в фон. `CLAUDE_ASYNC_AGENT_STALL_TIMEOUT_MS` (default 600000) сбрасывается на каждом progress-событии; нет прогресса → abort с частичным результатом.

### Fork-субагент (`/subtask`)

Fork наследует **весь разговор** вместо чистого старта; его tool calls остаются вне твоего контекста.

| | Fork | Именованный субагент |
|---|---|---|
| Контекст | Полная история | Чистый + переданный prompt |
| System prompt / tools | Как у main | Из файла определения |
| **Prompt cache** | **Общий с main (дешевле)** | Отдельный |

Команда: `/subtask draft unit tests for the parser changes so far`. Отдельно `/fork` копирует сессию в новую background-сессию.

### Защита от prompt injection в выводе субагента (v2.1.210+)

Перед тем как родитель прочтёт отчёт воркера, харнесс сканирует его: вставляет backslash в имитации control-тегов и строк `Human:`/`Assistant:`, и вешает префикс `[harness: subagent output matched instruction-shaped pattern(s): ...]`. Скан **ничего не удаляет** и не заменяет ограничение прав. Практика: директивный текст из отчёта воркера — это **находка для отчёта пользователю, а не инструкция**.

### Когда НЕ брать субагента

Частый back-and-forth; фазы, делящие много контекста; быстрая точечная правка (воркер стартует с нуля и должен набрать контекст); критична латентность. Для переиспользуемого промпта в главном контексте нужен **Skill**, а не субагент.

Предупреждение из доков: *«Running many subagents that each return detailed results can consume significant context»* — дайджест на возврате обязателен.

## Связано с

- [[tool-claude-code]] — харнесс, внутри которого живут субагенты
- [[method-agent-orchestration]] — когда делегировать и как писать бриф воркеру
- [[concept-multi-agent-failure-modes]] — почему сабагенты Claude Code названы «правильным» дизайном
- [[method-parallel-claude-terminals]] — уровни выше: teams, agent view, worktrees
- [[tool-claude-headless]] — те же агенты из скриптов и CI
- [[concept-context-window]] — изоляция контекста как главная мотивация
- [[method-token-economy]] — цена fan-out и общий кэш у fork
- [[pattern-writer-reviewer]] — критик со свежим контекстом как канонический субагент

## Источник

- https://code.claude.com/docs/en/sub-agents — схема frontmatter, built-in агенты, лимиты, fork, output scanning
- https://code.claude.com/docs/en/tools-reference — поведение инструмента `Agent`, переименование Task→Agent
- https://code.claude.com/docs/en/agents — официальная таксономия механизмов параллелизма
- https://code.claude.com/docs/en/agent-sdk/subagents — программные определения агентов
