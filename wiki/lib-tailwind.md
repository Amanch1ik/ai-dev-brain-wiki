# Tailwind CSS

Utility-first CSS-фреймворк: стилизуешь прямо в разметке классами-утилитами (`flex`, `pt-4`, `text-xl`), без написания CSS-файлов. Часть дефолтного стека [[method-premium-creative-web]]; хвалит [[person-armin-ronacher]].

## Содержание

Вместо семантических классов и отдельного CSS — композиция мелких утилит в `class`. Даёт скорость и консистентность (единая шкала отступов/типографики из конфига). v4 — движок на Rust (Oxide), OKLCH-палитра, CSS-first конфиг.

**Почему в премиум-стеке и почему AI-friendly:**
- Быстрый фидбек-цикл (правишь класс — видишь результат), fluid-типографика через `clamp()`/arbitrary values, произвольные значения для «сломанных» сеток и точной арт-дирекции.
- Стабильная популярная библиотека → агенты уверенно генерируют ([[concept-training-cutoff]]). Все стили на виду в разметке — легко ревьюить ([[concept-ai-friendly-codebase]], «важные вещи локально»).
- Грабли (по Ронахеру про фронт-стек в целом): «класс-каша» из 50 файлов → пора извлекать компоненты ([[concept-ai-friendly-codebase]], рефактори вовремя).

⚠ v3 → v4 breaking changes — сверяйся с tailwindcss.com.

## Связано с

- [[method-premium-creative-web]] — часть дефолтного стека
- [[lib-vite]] — типичная связка сборки
- [[concept-ai-friendly-codebase]] — стили на виду, когда извлекать компоненты
- [[concept-training-cutoff]] — стабильная популярная библиотека
- [[concept-oklch-color]] — v4 перешёл на OKLCH-палитру

## Источник

- raw/tailwind-readme.md — tailwindlabs/tailwindcss (README)
