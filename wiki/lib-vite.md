# Vite

Next-generation frontend build tool: мгновенный старт dev-сервера и молниеносный HMR поверх нативных ES-модулей, оптимизированная production-сборка. Часть стека, который хвалит [[person-armin-ronacher]] («Tailwind и vite — great, no complaints»).

## Содержание

Два компонента:
- **Dev-сервер** — обогащает нативные ES-модули (в частности, крайне быстрый Hot Module Replacement).
- **Build-команда** — бандлит код (Rolldown), выдаёт оптимизированные статические ассеты.

Расширяем через Plugin API и JavaScript API с полной типизацией. Node-совместим, полностью типизированные API.

**Почему в агентном стеке.** Скорость критична для агентного цикла: быстрый HMR/пересборка = быстрый фидбек, что прямо усиливает [[concept-tools-for-agents]] («tools need to be fast») и [[concept-ai-friendly-codebase]] (медленный boot тормозит агента). Стабильный популярный инструмент → уверенная генерация ([[concept-training-cutoff]]).

⚠ Версии/дефолты меняются — сверяйся с vite.dev.

## Связано с

- [[concept-ai-friendly-codebase]] — быстрый фидбек-цикл для агента
- [[concept-tools-for-agents]] — «инструмент должен быть быстрым»
- [[lib-tanstack-query]] — используется вместе в стеке
- [[concept-training-cutoff]] — стабильная популярная библиотека
- [[person-armin-ronacher]] — эндорсит Vite
- [[method-premium-creative-web]] — build-инструмент премиум-стека
- [[lib-react-three-fiber]] — R3F-проекты собираются на Vite
- [[lib-tailwind]] — типичная связка сборки

## Источник

- raw/vite-readme.md — Vite (README)
