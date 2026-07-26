# Паттерн Writer/Reviewer

Одна сессия/агент пишет код, другая — **в свежем контексте** — ревьюит его. Свежий контекст улучшает ревью: агент не смещён в пользу кода, который сам только что написал. Практика Anthropic.

## Содержание

**Параллельные сессии** (worktrees, desktop-сессии, web, agent teams) не только ускоряют, но и включают quality-workflow:

| Session A (Writer) | Session B (Reviewer) |
|---|---|
| `Implement a rate limiter for our API endpoints` | |
| | `Review the rate limiter in @src/…/rateLimiter.ts. Look for edge cases, race conditions, consistency with existing middleware.` |
| `Here's the review feedback: […]. Address these issues.` | |

Аналогично с тестами: один Claude пишет тесты, другой — код, чтобы их пройти.

### Adversarial review субагентом
Перед тем как счесть задачу готовой — субагент в свежем контексте видит **только diff и критерии**, не рассуждения, что породили изменение. Встроенный `/code-review` ищет баги; для проверки против плана — свой промпт: «review the diff against PLAN.md… Report gaps, not style preferences». Субагент возвращает пробелы прямо в сессию.

⚠ Важная оговорка: ревьюер, которого попросили искать пробелы, **почти всегда что-то найдёт**, даже если код хорош — это ведёт к over-engineering. Проси флагать только пробелы, влияющие на корректность или заявленные требования; остальное — опционально.

Это частный случай [[concept-agentic-engineering]] (роли: writer / reviewer) и реализация субагентов как «tools in a loop» ([[concept-llm-agent]]).

## Связано с

- [[method-claude-code-workflow]] — верификация как способ закрыть цикл
- [[concept-context-window]] — субагент/свежая сессия как чистый контекст
- [[concept-agentic-engineering]] — мультиагентные роли (writer/reviewer/security)
- [[concept-llm-agent]] — субагент как агент в отдельном окне
- [[method-ai-assisted-programming]] — независимое ревью как обязательный шаг
- [[tool-claude-code]] — worktrees, agent teams, `/code-review`
- [[pattern-agent-workflows]] — voting/evaluator-optimizer как обобщение
- [[tool-container-use]] — изолированные окружения для параллельных агентов
- [[method-agent-orchestration]] — верификация как часть протокола дирижёра
- [[concept-multi-agent-failure-modes]] — Code-Review-Loop как подтверждённо работающий паттерн
- [[tool-claude-subagents]] — чем именно исполняется критик со свежим контекстом

## Источник

- raw/claude-code-best-practices-anthropic.md — Anthropic, «Claude Code best practices»
