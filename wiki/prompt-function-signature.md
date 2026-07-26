# Промпт-приём: сигнатура функции

Для продакшн-кода задавай LLM **точную сигнатуру функции** и описание поведения — ты выступаешь дизайнером функции, модель пишет тело по спецификации. Приём [[simon-willison]].

## Содержание

После фазы исследования Уиллисон резко меняет режим: обращается с LLM «авторитарно», как с цифровым интерном, которому диктуют детальные инструкции. Пример из статьи:

```
Write a Python function that uses asyncio httpx with this signature:

async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path

Given a URL, this downloads the database to a temp directory and returns a path to it.
BUT it checks the content length header ... raises an error if over the limit.
When done: sqlite3.connect(...) then PRAGMA quick_check to confirm SQLite is valid ...
```

Такую функцию он написал бы сам минут за 15; Claude выдал за 15 секунд. Почему это работает:
- LLM «отлично заполняют пробелы»: ловят исключения, добавляют докстринги и типы — они «менее ленивы», чем человек.
- Английский допускает сокращения и неточности («use that popular HTTP library»), а код должен быть точным — диктовать спецификацию словами быстрее, чем печатать код.
- Естественное продолжение: «Now write me the tests using pytest» — технологию тестов тоже диктуешь ты.

Это частный случай [[method-context-management]] (примеры/спека как контекст) и часть [[method-ai-assisted-programming]] — но результат **обязательно** тестируется.

## Связано с

- [[method-ai-assisted-programming]] — приём внутри ответственного процесса (с ревью и тестами)
- [[method-context-management]] — спецификация как способ задать контекст
- [[tool-claude-code]] — «авторитарный» промптинг в реальном примере сессии
- [[simon-willison]] — автор приёма

## Источник

- raw/using-llms-for-code-simonwillison.md — Simon Willison, 2025-03-11
