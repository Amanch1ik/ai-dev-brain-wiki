# Параллельный Claude Code: сессии, worktrees, терминалы

Как гонять несколько инстансов Claude Code одновременно — от нативных фоновых сессий до фермы окон tmux. Решение «кому что поручить» — в [[method-agent-orchestration]].

## Содержание

### Что важно знать до чтения старых гайдов

Начиная с ветки v2.1.x у Claude Code появились **нативные** worktrees (`claude -w`), **фоновые сессии** (`claude --bg` + `claude agents`), **agent teams** со split-панелями tmux/iTerm2 и **dynamic workflows**. Большинство статей 2025 года и половина OSS-оркестраторов писались до этого и теперь частично избыточны.

### Выбор механизма

| Нужно | Механизм | Вход |
|---|---|---|
| Сам сижу в каждой сессии | worktrees + tmux/iTerm2 | `claude -w feat-a` в N терминалах |
| Раздал задачи, вернусь позже | **agent view** | `claude --bg "..."` → `claude agents` |
| Claude сам делит работу, воркеры переписываются | **agent teams** (экспериментально) | `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` |
| Одна механическая правка → 5–30 PR | `/batch` | `/batch migrate src/ from Solid to React` |
| Сотни агентов по скрипту с кросс-проверкой | **dynamic workflows** | `ultracode: …` |
| Полная изоляция ОС и зависимостей | container-use (Docker) | MCP-сервер `container-use stdio` |

### Git worktrees

**Нативный путь:**

```bash
claude --worktree feature-auth     # или -w
claude -w                          # имя сгенерится: bright-running-fox
claude --worktree "#1234"          # из PR; кавычки обязательны — # это комментарий в shell
```

Директория — `.claude/worktrees/<name>/`, ветка — `worktree-<name>`, база — **дефолтная ветка remote** (`origin/HEAD`), а не твой HEAD.

```bash
echo ".claude/worktrees/" >> .gitignore   # иначе засорит git status основного чекаута
```

Ветвиться от текущей работы вместо `origin/HEAD`:
```json
{ "worktree": { "baseRef": "head" } }
```
Имя конкретной ветки `baseRef` не принимает — для этого только `git worktree add` руками.

**Ручной путь:**
```bash
git worktree add ../project-feature-a -b feature-a
git worktree list
git worktree remove ../project-feature-a
git worktree prune
```

**Грабли, на которых спотыкаются все:**

- **`.env` и прочие gitignored-файлы не переносятся** — worktree это чистый чекаут. Лечится файлом `.worktreeinclude` в корне (синтаксис `.gitignore`); копируются только файлы, которые матчатся паттерном **и** gitignored. При своём хуке `WorktreeCreate` `.worktreeinclude` **не обрабатывается** — копируй внутри хука.
- **`node_modules` не разделяются.** `pnpm` с глобальным content-addressable store лечит частично; симлинк работает, но ломается на нативных биндингах и постинсталл-скриптах.
- **Общие ресурсы worktree не изолирует**: база, Redis, порты dev-сервера, кэши по абсолютным путям. Порты — из диапазона через `$PORT` в команде.
- **Одну ветку нельзя чекаутить в двух worktree** одновременно.
- **Stash общий на весь репозиторий** — виден из другого worktree. При worktree-workflow стэш и не нужен, в этом смысл.
- `claude -p --worktree` **за собой не убирает** — чистить руками.
- Удаление фоновой сессии из agent view **удаляет её worktree** — коммить до удаления.

**Что worktree делит с основным чекаутом (и это хорошо):** `.git` (коммиты идут в общий object store), project-scope плагины, permission-одобрения «Yes, don't ask again» — они сохраняются в `.claude/settings.local.json` **основного** чекаута и переживают удаление worktree.

**Изоляция субагента:** `isolation: worktree` во frontmatter — параллелизм внутри параллелизма. ⚠ **Agent teams в worktrees не изолируются** — воркеры работают в одной директории, партиционировать файлы твоя задача.

### Фоновые сессии (agent view)

Половина того, ради чего раньше ставили сторонние оркестраторы.

```bash
claude --bg "investigate the flaky test"
claude --bg --name auth-refactor "refactor auth module"
claude --agent code-reviewer --bg "review PR #1234"

claude agents                # дашборд
claude agents --json --all   # машиночитаемо — база для своего монитора
claude attach <id> ; claude logs <id> ; claude stop <id> ; claude rm <id>
```

В дашборде: `Space` — peek без открытия, `Enter` — attach, `Ctrl+T` — pin, `Ctrl+X` — stop. Изнутри сессии: `/bg` уводит текущую в фон, `/fork` копирует в новую фоновую.

Фоновые сессии **автоматически** уезжают в `.claude/worktrees/` перед первой правкой файла; отключается `{"worktree": {"bgIsolation": "none"}}`. Живут в per-user supervisor daemon, переживают sleep машины, но не shutdown. Rate limits расходуются пропорционально числу сессий.

### Agent teams

Включение: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` в env или `settings.json`.

| | Субагенты | Agent teams |
|---|---|---|
| Контекст | Свой, результат вызвавшему | Свой, полностью независимый |
| Коммуникация | Только назад главному | Воркеры пишут друг другу |
| Координация | Главный агент рулит всем | Shared task list + self-claim |
| Цена | Ниже | **≈7× токенов** обычной сессии |

Lead — главная сессия, лидерство передать нельзя. Воркеры — полноценные сессии, им можно писать напрямую. Хранилище: `~/.claude/teams/{team}/config.json`, mailbox `inboxes/{agent}.json`, задачи `~/.claude/tasks/{team}/` (переживают сессию). Захват задач — через **file locking**, гонки исключены на уровне рантайма; зависимости разрешаются автоматически. Конфиг руками не править — рантайм перезапишет.

Отображение — `teammateMode` в settings: `in-process` (по умолчанию, работает везде), `auto`, `tmux`, `iterm2`. Режим iTerm2 требует CLI **`it2`** и включённого Python API в настройках iTerm2. Split-панели **не работают** в терминале VS Code, Windows Terminal и Ghostty. Рекомендованный вход в tmux на macOS — `tmux -CC` внутри iTerm2.

Готовые определения субагентов переиспользуются как роли воркеров: наследуются `tools` и `model`, тело **дописывается** к system prompt. `skills` и `mcpServers` из frontmatter на этом пути **не применяются**.

Хуки как quality gate: `TeammateIdle` (exit code 2 → воркер продолжает работать), `TaskCreated`, `TaskCompleted` (exit 2 не даёт закрыть задачу и возвращает фидбек).

Предупреждение из доков: *«Letting a team run unattended for too long increases the risk of wasted effort»*.

**Хак:** общий task list между независимыми сессиями без agent teams —
```bash
CLAUDE_CODE_TASK_LIST_ID=my-project claude -w feat-a
CLAUDE_CODE_TASK_LIST_ID=my-project claude -w feat-b
```
Обе пишут в `~/.claude/tasks/my-project/`. Самый дешёвый способ дать N терминалам общую доску.

### tmux вручную

Окно на воркер, **не панель**: при 6+ агентах сплиты превращаются в «panel hell». Панели — только под manager-дашборд.

```bash
#!/usr/bin/env bash
set -euo pipefail
SESSION=orchestra
REPO="$(git rev-parse --show-toplevel)"
TASKS=(auth-refactor flaky-tests perf-db docs-sweep)

tmux new-session -d -s "$SESSION" -n manager -c "$REPO"
for t in "${TASKS[@]}"; do
  WT="$REPO/.claude/worktrees/$t"
  [ -d "$WT" ] || git -C "$REPO" worktree add "$WT" -b "worktree-$t" origin/HEAD
  tmux new-window -t "$SESSION" -n "$t" -c "$WT"
  tmux send-keys -t "$SESSION:$t" "claude" C-m
done
tmux attach -t "$SESSION"
```

**Отправка промпта программно.** TUI не успевает обработать текст и Enter в одном `send-keys` — нужна пауза, а `-l` обязателен, иначе `Enter`/`C-c` внутри текста истолкуются как клавиши:

```bash
tmux send-keys -t "$TARGET" -l "$*"
sleep 0.5
tmux send-keys -t "$TARGET" Enter
```

Состояние панели читается через `tmux capture-pane -p -t <target>` — на этом построены все сторонние мониторы. Детект по строкам TUI **хрупкий**: ломается с каждым изменением интерфейса. Честнее — хуки или `claude agents --json`.

⚠ **`Ctrl+B` в Claude Code = увести Bash-команду в фон, и это же префикс tmux.** Под tmux жать **дважды** либо переназначить префикс (`set -g prefix C-a`). Полезно также `setw -g automatic-rename off`, иначе имена окон затрутся именами процессов.

**iTerm2 напрямую:**
```bash
osascript -e 'tell application "iTerm2" to create window with default profile'
```
Модель: application → window → tab → session (session = панель). `write text` отправляет строку **и** жмёт Enter — отдельный Enter не нужен, в отличие от tmux.

### Координация: как не словить merge hell

1. **Партиционируй по файлам, а не по фичам**: «агент A владеет `src/api/**`, агент B — `src/ui/**`». Официально: *«Two teammates editing the same file leads to overwrites»*.
2. Общие файлы (`package.json`, схема БД, i18n-словари, barrel-экспорты) — **зона только владельца репозитория**.
3. `git fetch && git rebase origin/main` в каждом worktree **перед** PR, не после.
4. Мержить по одному, прогоняя тесты между мержами.
5. Широкие механические правки — через `/batch` (PR на юнит), а не пятью ручными агентами.
6. Задачи с зависимостями — **последовательно**.
7. Обязательный **явный шаг сведения**: без него теряется половина пользы от параллели.
8. Коммит каждые ~30 минут в каждом worktree — чтобы сорвавшийся агент не унёс три часа работы.

**Lock-протокол** для сессий без общего task list (проверен на 20+ агентах, реализуется целиком промптом в `CLAUDE.md`): каталог `coordination/` с `active_work_registry.json`, `agent_locks/`, `completed_work_log.json`. Агент до правки читает реестр, создаёт lock-файл со списком путей, которые захватывает, и не трогает незахваченное. Локи старше **2 часов** считаются брошенными. Атомарный захват на shell-уровне — через `set -o noclobber`, а не `[ -f ] && touch`.

### Экосистема сторонних оркестраторов (на 2026-07)

**Живые:** `smtg-ai/claude-squad` (⭐8159, tmux+worktree TUI, ⚠ AGPL-3.0 и auto-accept в обход permissions) · `kbwo/ccmanager` (⭐1199, самый свежий; **не требует tmux**, показывает реальный статус Waiting/Busy/Idle, умеет переносить контекст сессии между worktrees) · `dagger/container-use` (⭐3917, контейнер + ветка на агента — единственный, кто изолирует **окружение**) · `h0x91b/dev-3.0` (⭐228, Kanban-кокпит).

**Умершие — не начинать на них:** `BloopAI/vibe-kanban` (⭐27482, **sunsetting**, о чём написано в README) · `stravu/crystal` (⭐3102, **deprecated**) · `devflowinc/uzi` (⭐579, год без коммитов, но лучший CLI-дизайн фермы: `prompt --agents claude:3`, `ls -w`, `broadcast`, `checkpoint`) · `Jedward23/Tmux-Orchestrator` (⭐1801, источник паттерна send-keys+sleep+Enter; ⚠ в `schedule_with_note.sh` **захардкожен путь автора**).

⚠ `parruda/claude-swarm` **не существует** (404); под этим именем лежит ≥8 несвязанных проектов разных авторов. `ruvnet/ruflo` (бывший `claude-flow`, ⭐65537) — заявления README не верифицированы, относиться скептически.

**Что уже не надо строить руками:** мониторинг статуса (`claude agents`), изоляция файлов (`-w`, `bgIsolation`), общий task list с file-locking (agent teams), fan-out по файлам с PR (`/batch`), периодический опрос (`/loop`, `Monitor`). Сторонние инструменты сейчас добавляют в основном **UI** поверх этого.

### Опасное

- `--dangerously-skip-permissions` (нужен для agent farm) — только в контейнере или на выделенной машине.
- Auto-accept в сторонних TUI обходит защиту Claude Code.
- Токены масштабируются **линейно** по числу агентов; agent teams ≈7×. Предупреждение `Large workflow` возникает при >25 агентов или прогнозе >1,5M токенов.
- Не оставлять ферму без присмотра надолго.

### Чек перед запуском

```bash
echo ".claude/worktrees/" >> .gitignore
printf '.env\n.env.local\n' > .worktreeinclude
claude --version    # agent view требует ≥ 2.1.139
which tmux
```
Плюс: партиционирование файлов записано в `CLAUDE.md` · частые Bash-команды предодобрены в `permissions.allow` (иначе промпты воркеров всплывают в сессии lead'а и всё встаёт) · порты из диапазона.

## Связано с

- [[method-agent-orchestration]] — что кому поручать и как писать бриф
- [[tool-claude-subagents]] — уровень ниже: воркеры внутри одной сессии
- [[tool-claude-headless]] — `claude -p` как строительный блок собственного fan-out
- [[tool-claude-code]] — сам инструмент
- [[method-token-economy]] — линейный рост стоимости с числом агентов
- [[concept-multi-agent-failure-modes]] — почему параллельная запись ломается
- [[method-claude-code-workflow]] — сессионная гигиена
- [[concept-agentic-engineering]] — общая рамка: агенты как компоненты процесса

## Источник

- https://code.claude.com/docs/en/worktrees — `--worktree`, `.worktreeinclude`, `baseRef`, sweep, хуки
- https://code.claude.com/docs/en/agent-view — фоновые сессии, daemon, `bgIsolation`
- https://code.claude.com/docs/en/agent-teams — task list, mailbox, `teammateMode`, хуки, лимиты
- https://code.claude.com/docs/en/agents — таксономия механизмов параллелизма
- https://habr.com/ru/articles/1001478/ — обзор шести инструментов, тезис про «review bottleneck»
- https://habr.com/ru/companies/alpinadigital/articles/1032134/ — правила параллели, обязательный шаг сведения
- https://tproger.ru/articles/tmux-v-2026-godu-perezhivaet-vtoroe-rozhdenie-kak-razrabotchiki-de — потолок 4–8 агентов, panel hell
- https://vc.ru/ai/2765507-git-worktrees-i-claude-code-effektivnaya-parallelnaya-razrabotka — грабли worktrees
- https://github.com/Dicklesworthstone/claude_code_agent_farm — lock-протокол координации
