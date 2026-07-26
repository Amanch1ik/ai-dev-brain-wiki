# Partner with the AI, throw away the code

Кейс [[martin-fowler]]: сложную алгоритмическую задачу (медленный API-эндпоинт на Go/MySQL) решили в паре с AI (Cursor + Claude Sonnet 4), причём часть промежуточного кода — расходный материал. Практические takeaways.

## Содержание

Задача: перевести медленный эндпоинт с Transaction Script на Domain Model (PoEAA), не понимая деталей legacy-функции. Ход работы и уроки:

- **Проси AI объяснить код.** Первый шаг — реверс-инжиниринг бизнес-правил: «Read `<function>` in `@<file>` and write good documentation… You may use `mysql -u… -h127.0.0.1…` to inspect the schema». Дав AI доступ к MySQL, он исследует схему и пробует запросы. Результат неидеален (не хватило точности для acceptance criteria), но это старт. → **Takeaway: ask the AI to explain the code.**
- **Генерируй незнакомые утилиты.** Попросил Cursor сгенерить бенчмарк на нативном Go benchmarking (табличный тест). Дефолтно Cursor выдал старый стиль (до Go 1.24) — пришлось попросить перевести на новый `testing.B.Loop`. → **Takeaway: use AI to generate unfamiliar (to me) utilities.** Дало baseline: 21с / 11с / 7+ мин на патологическом кейсе.
- **Test coverage как страховка.** Сделал стаб реимплементации и скопировал существующие тесты, чтобы сверять новую версию со старой; попутно улучшал нечитаемые тестовые данные (дерево оргструктуры).

Это иллюстрация [[concept-ai-risk-assessment]] в действии (калиброванная оценка Probability/Impact/Detectability на legacy-миграции с feature parity) и части [[method-context-management]] — «дамп кода → вопросы к модели».

## Связано с

- [[concept-ai-risk-assessment]] — та же legacy-миграция как пример калибровки
- [[method-ai-assisted-programming]] — «проси объяснить код» как приём
- [[method-context-management]] — вопросы к коду через модель
- [[tool-cursor]] — инструмент кейса (Cursor + Sonnet 4)
- [[martin-fowler]] · [[source-exploring-gen-ai]] — источник

## Источник

- raw/throw-away-code-fowler.md — Martin Fowler, «Partner with the AI, throw away the code»
