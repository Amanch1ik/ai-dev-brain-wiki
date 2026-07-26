# TanStack Query

Библиотека управления **асинхронным (server) состоянием**: упрощает fetching, кеширование, синхронизацию и обновление серверных данных. Часть фронтенд-стека, который эндорсит [[person-armin-ronacher]].

## Содержание

Протокол-агностична (REST, GraphQL, промисы). Ключевое: разделяет **server state** (данные с бэкенда) и client state, снимая с разработчика ручное кеширование/инвалидацию. Возможности: кеширование, refetch, пагинация и infinite scroll, мутации, зависимые запросы, background updates, prefetching, отмена, поддержка React Suspense.

**Почему в агентном стеке.** Ронахер выбрал TanStack Query + Router + Vite + Tailwind для фронтенда. Стабильная, популярная библиотека → хорошо представлена в обучающих данных ([[concept-training-cutoff]]), агенты уверенно её генерируют. Замечание Ронахера: файловый роутер TanStack с `$` в именах путей путает агента (shell-интерполяция) — это про [[concept-ai-friendly-codebase]] (мелкие трения инструментов).

⚠ API версий меняется — сверяйся с tanstack.com/query.

## Связано с

- [[concept-ai-friendly-codebase]] — часть AI-friendly фронтенд-стека (и его трения)
- [[lib-vite]] — используется вместе в том же стеке
- [[concept-training-cutoff]] — стабильная популярная библиотека = уверенная генерация
- [[person-armin-ronacher]] — эндорсит эту связку

## Источник

- raw/tanstack-query-readme.md — TanStack Query (README)
