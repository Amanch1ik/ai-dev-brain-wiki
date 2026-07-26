# Augmented LLM

Базовый строительный блок агентных систем: LLM, дополненная **retrieval, инструментами и памятью**. Современные модели активно ими пользуются — сами генерируют поисковые запросы, выбирают инструменты, решают, что запомнить. Формулировка Anthropic.

## Содержание

Все workflow- и agent-паттерны ([[pattern-agent-workflows]], [[concept-workflows-vs-agents]]) строятся поверх этого блока — предполагается, что каждый LLM-вызов имеет доступ к augmented-возможностям.

Два ключевых аспекта реализации:
1. **Заточить возможности** под конкретный use case.
2. Дать LLM **простой, хорошо документированный интерфейс** (см. [[concept-agent-computer-interface]]).

Один из способов подключить augmentations — [[concept-mcp]] (Model Context Protocol): интеграция с растущей экосистемой сторонних инструментов через простой клиент. Память как augmentation перекликается с [[concept-agent-memory]].

## Связано с

- [[concept-workflows-vs-agents]] — блок, поверх которого строятся оба типа систем
- [[pattern-agent-workflows]] — паттерны, использующие augmented LLM
- [[concept-mcp]] — способ подключить инструменты/данные
- [[concept-llm-agent]] — агент = augmented LLM, гоняющий инструменты в цикле
- [[concept-agent-memory]] — память как одна из augmentations

## Источник

- raw/building-effective-agents-anthropic.md — Anthropic, «Building Effective Agents»
