# GitHub Copilot

AI-ассистент кодинга от GitHub: автодополнение в IDE и чат. В этой базе — инструмент, на котором Thoughtworks отрабатывали практику [[method-tdd]].

## Содержание

Ключевое свойство для практики: **Copilot использует открытые файлы как контекст**. Отсюда тактики из [[method-tdd]] — держать тест и реализацию рядом, класть стартовый контекст (user story, acceptance criteria, «assume no GUI») в начало файла; описательные имена тестов в стиле Given-When-Then резко улучшают автодополнение.

Наблюдаемые ограничения (по опыту Thoughtworks): плохо делает «baby steps» (прыгает к нетестированной функциональности → нужен backfill тестов); слаб в рефакторинге крупнее одного метода — там выигрывают IDE-рефакторинги. Для правок реализации эффективнее удалить код и дать перегенерить.

Также фигурирует в обзоре [[method-context-engineering]] как инструмент, перенимающий фичи в духе Claude Code (path-based rules, skills-подобные механизмы).

⚠ Возможности быстро меняются — сверяйся с github.com/features/copilot.

## Связано с

- [[method-tdd]] — практика TDD, отработанная на Copilot
- [[method-context-engineering]] — Copilot среди ассистентов с rules/skills
- [[method-ai-assisted-programming]] — типовой инструмент ответственного процесса

## Источник

- raw/tdd-with-copilot-fowler.md — Thoughtworks, «TDD with GitHub Copilot»
- raw/context-engineering-fowler.md — Martin Fowler, «Context Engineering for Coding Agents»
