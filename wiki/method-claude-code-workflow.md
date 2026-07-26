# Рабочий процесс Claude Code

Каноничные best practices Anthropic по эффективной работе с Claude Code. Всё выводится из [[concept-context-window]] — контекст заполняется, производительность падает.

## Содержание

### Дай Claude способ проверить свою работу
Claude останавливается, когда работа «выглядит готовой». Без проверки «выглядит готово» — единственный сигнал, и **ты становишься циклом верификации**. Дай что-то, возвращающее pass/fail (тесты, exit-код сборки, линтер, скрипт-diff против фикстуры, скриншот против дизайна) — и цикл замыкается сам. Уровни жёсткости: в одном промпте → `/goal`-условие → Stop-hook (детерминированный гейт) → второй агент-ревьюер ([[pattern-writer-reviewer]]). Требуй **доказательства** (вывод тестов, команду и её результат, скриншот), а не утверждения об успехе. Это операционализация [[concept-ai-risk-assessment]] (ось Detectability).

### Explore → Plan → Code
Разделяй исследование/планирование и реализацию, иначе решишь не ту задачу. Plan mode полезен, но добавляет overhead: для мелкой ясной правки (опечатка, лог-строка, переименование) — делай сразу. План нужен, когда подход неясен, правка затрагивает много файлов или код незнаком. «Если diff описывается одним предложением — пропусти план».

### Давай конкретный контекст
Точность инструкций снижает число правок. Указывай файлы (`@`), ограничения, примеры паттернов. «add tests for foo.py» → «write a test for foo.py covering the edge case where the user is logged out. avoid mocks». Богатый ввод: `@`-файлы, вставка скриншотов, URL-доки, `cat error.log | claude`.

### Дай Claude взять у тебя интервью
Для крупной фичи: «Interview me in detail using the AskUserQuestion tool… then write a complete spec to SPEC.md», затем свежая сессия исполняет спеку. Хорошая спека самодостаточна (файлы, интерфейсы, что вне scope, end-to-end проверка). Связь с [[method-spec-driven-development]].

### Управляй сессией
`Esc` — стоп с сохранением контекста; `Esc+Esc`/`/rewind` — откат состояния; «Undo that»; **`/clear`** между несвязанными задачами; субагенты для исследования ([[concept-context-window]]).

### Типовые провалы
- **Kitchen sink** (мешанина задач) → `/clear`.
- **Correcting over and over** → после 2 неудач `/clear` + лучший промпт.
- **Over-specified CLAUDE.md** → безжалостно чистить ([[concept-claude-md]]).
- **Trust-then-verify gap** → всегда давай верификацию; «не можешь проверить — не шипи».
- **Infinite exploration** → узко ограничивай или используй субагентов.

### Развивай интуицию
Паттерны — стартовые точки, не догмы: иногда контекст стоит копить, иногда пропустить план, иногда расплывчатый промпт — то, что нужно.

## Связано с

- [[concept-context-window]] — фундамент всех этих практик
- [[concept-claude-md]] · [[method-context-engineering]] — настройка окружения
- [[pattern-writer-reviewer]] — верификация свежим контекстом / субагентом
- [[method-ai-assisted-programming]] — «тестируй то, что написано», take-over
- [[concept-ai-risk-assessment]] — верификация как ось Detectability
- [[method-spec-driven-development]] — интервью → SPEC.md
- [[tool-claude-code]] — инструмент, к которому относятся практики
- [[method-agent-orchestration]] — когда задачу стоит раздать нескольким агентам
- [[method-parallel-claude-terminals]] — параллельные сессии и worktrees
- [[method-token-economy]] — сессионная гигиена с точки зрения расходов

## Источник

- raw/claude-code-best-practices-anthropic.md — Anthropic, «Claude Code best practices» (текстовая версия доклада Cal Rueb, Code w/ Claude, 2025-05-22)
