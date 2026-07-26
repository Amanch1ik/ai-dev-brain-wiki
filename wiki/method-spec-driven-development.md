# Spec-driven development (SDD)

Подход, при котором перед кодом с AI пишут «спеку» («documentation first»), и она становится источником истины для человека и агента. Разбор [[martin-fowler]] по трём инструментам.

## Содержание

Определение в движении. Фаулер выделяет **три уровня** зрелости:
1. **Spec-first** — продуманную спеку пишут первой и используют в задаче.
2. **Spec-anchored** — спеку сохраняют и после задачи, для эволюции фичи.
3. **Spec-as-source** — спека становится главным файлом; человек правит только её, код не трогает (`// GENERATED FROM SPEC - DO NOT EDIT`).

Все найденные подходы — spec-first; не все стремятся к anchored/as-source.

**Спека vs memory bank.** Спека — структурированный behavior-oriented артефакт на естественном языке для конкретной функциональности. От него отличают более общий контекст кодовой базы (rules-файлы, описание продукта/архитектуры) — «memory bank», релевантный всем сессиям. Это частный вид [[method-context-engineering]].

### Три инструмента
- **Kiro** (VS Code) — самый лёгкий, spec-first. Workflow: **Requirements → Design → Tasks** (по markdown-файлу на шаг; requirements как «User Story» + acceptance «GIVEN/WHEN/THEN»). Memory bank = «steering» (product/tech/structure.md).
- **Spec-kit** (GitHub, CLI) — самый кастомизируемый (артефакты в воркспейсе, слэш-команды). Workflow: **Constitution → 𝄆 Specify → Plan → Tasks 𝄇**. Memory bank = «constitution» (immutable-принципы, мощный rules-файл). Активно использует чек-листы как «definition of done» (интерпретируются AI — без гарантий). Создаёт ветку на каждую спеку.
- **Tessl** (beta, CLI + MCP-сервер) — единственный, кто целится в spec-anchored и экспериментирует со spec-as-source (1:1 спека↔файл, `tessl build`).

### Скепсис Фаулера
- **Один workflow на все размеры?** Для мелкого бага Kiro раздул до 4 «user stories» и 16 acceptance criteria — «кувалдой по ореху». Spec-kit генерит гору повторяющихся markdown-файлов на ревью.
- **Ревьюить markdown вместо кода** — часто хуже, чем ревьюить код.
- **Ложное чувство контроля** — агент всё равно игнорирует инструкции или, наоборот, чрезмерно рьяно следует (дубли, overengineering). Контроль лучше даёт малые итеративные шаги — что противоречит идее большого up-front дизайна.
- **Параллель с MDD** (model-driven development): LLM убирают часть overhead, но добавляют недетерминизм; риск получить «недостатки обоих» — негибкость И недетерминизм.
- Термин уже «семантически размыт» — «спекой» иногда называют просто «детальный промпт».

Вывод: принцип **spec-first** ценен (Фаулер сам часто пишет спеку агенту), но инструменты рискуют буквально переносить старые workflow и усиливать review overload («Verschlimmbesserung»).

## Связано с

- [[method-context-engineering]] — спека и memory bank как виды контекста
- [[method-ai-assisted-programming]] — spec-first как часть ответственного процесса
- [[concept-mcp]] — Tessl CLI работает и как MCP-сервер
- [[method-claude-code-workflow]] — «интервью → SPEC.md» как лёгкий spec-first
- [[concept-ai-swe]] — memory bank vs спеки в системном подходе
- [[martin-fowler]] · [[source-exploring-gen-ai]] — источник разбора

## Источник

- raw/spec-driven-development-fowler.md — Martin Fowler, «Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl»
