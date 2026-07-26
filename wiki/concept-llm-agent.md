# LLM-агент

Рабочее определение [[simon-willison]] (сентябрь 2025): **«An LLM agent runs tools in a loop to achieve a goal»** — LLM-агент выполняет инструменты в цикле для достижения цели.

## Содержание

Уиллисон годами избегал термина «agent» как buzzword: он собрал 211 определений с Twitter и сгруппировал их в 13 категорий. Проблема не нова — ещё в 1994 Michael Wooldridge цитировал Карла Хьюитта: вопрос «что такое агент?» так же неловок для agent-based computing, как «что такое интеллект?» для мейнстрим-AI.

Разбор определения:
- **«Tools in a loop»** — паттерн, на котором сошёлся, в частности, Anthropic; он же зашит в tool/function calling многих LLM API: модель запрашивает действия, харнесс их выполняет, результат возвращается в модель для дальнейшего рассуждения. На Anthropic dev-конференции Hannah Moran сформулировала это прямо: «Agents are models using tools in a loop».
- **«To achieve a goal»** — это не бесконечный цикл: есть условие остановки.
- Уиллисон сознательно **не** добавил «цель, поставленную пользователем»: существуют sub-agent паттерны, где цель ставит другая LLM (пример — [[tool-claude-code]] и Claude Research).
- **Память**: короткая память встроена — предыдущие шаги живут в диалоге с моделью; долговременную удобнее реализовать через отдельный набор инструментов (см. [[concept-agent-memory]]).

### Определения, которые он отвергает

- **Агент = замена сотрудника** (customer support / sales / accounting agents) — «пока научная фантастика». У людей есть **accountability** (ответственность) и **agency** (способность формировать собственные цели), чего у AI-агентов нет. Легендарный слайд IBM 1979: «A computer can never be held accountable, therefore a computer must never make a management decision».
- **Путаница от OpenAI**: Сэм Альтман называет агентов «системами, что делают работу за тебя независимо»; при этом «ChatGPT agent» — это браузерная автоматизация, а Agents SDK (`openai-agents` / `@openai/agents`) — как раз ближе к «tools in a loop».

## Связано с

- [[simon-willison]] — автор принятого определения
- [[tool-claude-code]] — пример sub-agent паттерна (LLM ставит цель другой LLM)
- [[tool-aider]] — open-source реализация «tools in a loop»
- [[concept-agent-memory]] — память как ещё один набор инструментов
- [[concept-mcp]] — стандарт для подключения инструментов к агенту
- [[concept-agentic-steering]] — режим supervised agent и роль человека
- [[concept-supply-chain-risk]] — attack surface цикла «tools in a loop»
- [[concept-agentic-engineering]] — системы, где агенты — компоненты процесса
- [[pattern-writer-reviewer]] — субагент как агент в отдельном окне
- [[concept-workflows-vs-agents]] — различие workflows и agents
- [[pattern-agent-workflows]] — паттерны построения агентных систем
- [[concept-augmented-llm]] — базовый блок агента
- [[concept-agent-computer-interface]] — качество инструментов критично для цикла
- [[method-ai-assisted-programming]] — смежная область применения LLM в разработке
- [[source-exploring-gen-ai]] — memo «humans and agents in loops»
- [[tool-modelscope]] — площадка с бесплатным API к моделям (Qwen/DeepSeek), OpenAI-совместимо

## Источник

- raw/what-is-an-agent-simonwillison.md — Simon Willison, «I think “agent” may finally have a widely enough agreed upon definition», 2025-09-18
- raw/tools-in-a-loop-simonwillison.md — Simon Willison, цитата Hannah Moran (Anthropic), 2025-05-22
- https://simonwillison.net/2025/May/22/tools-in-a-loop/ — про «tools in a loop»
