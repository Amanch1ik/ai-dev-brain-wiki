# Claude Code в headless-режиме

`claude -p` — неинтерактивный запуск: строительный блок для CI, cron и собственных ферм агентов. База для fan-out из [[method-parallel-claude-terminals]].

## Содержание

### Базовое

```bash
claude -p "Find and fix the bug in auth.py" --allowedTools "Read,Edit,Bash"
cat build-error.txt | claude -p 'concisely explain the root cause' > out.txt
```
stdin через пайп капнут на **10 MB** — превышение даёт ошибку и ненулевой код возврата.

### `--bare` — режим для CI и скриптов

Пропускает автодискавери: hooks, skills, plugins, MCP-серверы, auto memory, `CLAUDE.md`. Даёт **минимальный контекст и воспроизводимость**. Anthropic пишет, что `--bare` **станет дефолтом для `-p`**.

⚠ Пропускает также OAuth и keychain → нужен `ANTHROPIC_API_KEY` или `apiKeyHelper`. Нужное догружается явно: `--append-system-prompt`, `--settings`, `--mcp-config`, `--agents <json>`.

### Форматы вывода

```bash
claude -p "Summarize this project" --output-format json | jq -r '.result'
claude -p "..." --output-format json | jq -r '.total_cost_usd'
```

- `json` — payload содержит `result`, `session_id`, **`total_cost_usd`** и разбивку по моделям. Основа для подсчёта бюджета прогона.
- `--json-schema '{...}'` — валидированный структурированный ответ в `structured_output`, парсится без регэкспов. Невалидная схема теперь даёт явную ошибку, а не молчание.
- `stream-json` — NDJSON, последняя строка `result`. Для живого дашборда: `--output-format stream-json --verbose --include-partial-messages`.

Стриминг текста наружу:
```bash
claude -p "Write a poem" --output-format stream-json --verbose --include-partial-messages | \
  jq -rj 'select(.type=="stream_event" and .event.delta.type?=="text_delta") | .event.delta.text'
```

**Субагенты в стриме** приходят как `assistant`/`user` с `parent_tool_use_id`, равным id соответствующего вызова `Agent` (у главного разговора там `null`). По умолчанию эмитятся только их `tool_use`/`tool_result`; текст и thinking — по флагу `--forward-subagent-text`.

### Ключевые флаги

**Сессия:** `-c/--continue` · `-r/--resume <id>` · `--fork-session` · `--session-id` · `--bg` · `-n/--name`
**Промпт:** `--system-prompt` (полная замена) · `--append-system-prompt` · **`--append-subagent-system-prompt`** (дописывается каждому субагенту, включая вложенных)
**Права:** `--allowedTools` · `--disallowedTools` · `--permission-mode` · `--permission-prompt-tool`
**Модель:** `--model` · `--effort low|medium|high|xhigh|max` · `--fallback-model sonnet,haiku`
**Агенты:** `--agent <name>` (агент как главная сессия) · `--agents '<json>'` (эфемерные определения)
**Лимиты:** `--max-turns N` · `--max-budget-usd 5.00`
**Прочее:** `-w/--worktree` · `--bare` · `--add-dir` · `--strict-mcp-config` · `--debug`

Синтаксис `--allowedTools` — это permission rules, и пробел перед `*` критичен:
```bash
claude -p "commit my staged changes" \
  --allowedTools "Bash(git diff *)" "Bash(git status *)" "Bash(git commit *)"
```
`Bash(git diff*)` без пробела заматчит ещё и `git diff-index`.

Эфемерные субагенты прямо из командной строки:
```bash
claude --agents '{"reviewer":{"description":"Code reviewer","prompt":"You are a senior reviewer.","tools":["Read","Grep"],"model":"sonnet"}}'
```

Слэш-команды и skills в `-p` работают — просто вставь `/skill-name` в строку промпта. Терминальные (`/login`) — нет.

### Fan-out своими руками

```bash
#!/usr/bin/env bash
set -euo pipefail
MAX=6
migrate() {
  local f="$1" name="mig-$(basename "$f" | tr -cd 'a-zA-Z0-9')"
  claude -p --worktree "$name" \
    "Migrate $f from styled-components to Tailwind. Run tests. Commit." \
    --allowedTools "Read,Edit,Bash(npm test *),Bash(git *)" \
    --output-format json > "logs/$name.json"
}
export -f migrate
mkdir -p logs
find src/components -name '*.tsx' -print0 | xargs -0 -P "$MAX" -I{} bash -c 'migrate "$@"' _ {}
jq -s 'map({sid:.session_id, cost:.total_cost_usd, res:.result[0:120]})' logs/*.json
```

Продолжение конкретного разговора:
```bash
sid=$(claude -p "Start a review" --output-format json | jq -r '.session_id')
claude -p "Continue that review" --resume "$sid"
```
⚠ Обе команды — **из одной директории**: поиск session id скоупится по project directory и её worktrees.

### Cron

```bash
0 3 * * * cd ~/proj && /usr/local/bin/claude --bare -p \
  "run the nightly dependency audit and open an issue if anything is critical" \
  --allowedTools "Bash,Read,Edit,WebFetch" >> ~/logs/claude-nightly.log 2>&1
```

### Коды возврата и жизненный цикл

- `0` — успех; `143` — SIGTERM (прерывает ход, убивает дерево процессов запущенных команд, гоняет `SessionEnd`-хуки); `1` — не удалось войти в worktree.
  ⚠ Полной таблицы кодов в документации нет. В SDK эквивалент — `SDKResultMessage.subtype`: `success`, `error_max_turns`, `error_during_execution`, `error_interrupted`, `error_no_valid_model`.
- **Фоновый shell** убивается через ~5 секунд после финального результата. **Фоновые субагенты и workflows** ждутся, но не дольше **10 минут** (`CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS`, `0` — без лимита).
- `claude -p --worktree` **за собой не убирает** — чистка ручная.
- В `-p` режим `--permission-mode auto` **прерывается**, если классификатор многократно блокирует: подтвердить некому.

### Agent SDK как альтернатива

Для TypeScript и Python есть `@anthropic-ai/claude-agent-sdk` / `claude-agent-sdk`: **бандлят нативный бинарь** Claude Code, отдельная установка не нужна. Субагенты объявляются программно в поле `agents` опций `query()`, и **программные определения перебивают** файловые с тем же именем. Для остальных языков канонический путь — `claude -p --output-format json`.

Разграничение: **Client SDK** — ты сам крутишь tool loop; **Agent SDK** — loop крутит Claude; **Managed Agents** — hosted REST API с песочницей на стороне Anthropic.

## Связано с

- [[tool-claude-code]] — интерактивный режим того же инструмента
- [[method-parallel-claude-terminals]] — где headless используется как строительный блок
- [[tool-claude-subagents]] — `--agents` и `--append-subagent-system-prompt`
- [[method-token-economy]] — `total_cost_usd` в json-выводе как способ мерить прогон
- [[method-agent-orchestration]] — что поручать таким процессам

## Источник

- https://code.claude.com/docs/en/headless — `-p`, `--bare`, форматы вывода, стрим, фоновые задачи на выходе
- https://code.claude.com/docs/en/cli-reference — полная таблица флагов
- https://code.claude.com/docs/en/agent-sdk/overview — установка и место SDK относительно CLI
