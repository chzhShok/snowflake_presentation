# Snowflake: Elastic Data Warehouse

Доклад по статье **«The Snowflake Elastic Data Warehouse»** (SIGMOD 2016) — одной из самых цитируемых работ по архитектуре облачных аналитических систем.

## О чём доклад

Snowflake — пример проектирования системы с нуля под облако. В статье разбирается, как авторы решали нефункциональные требования: масштабируемость, эластичность, изоляция нагрузок и обновления без простоя.

Ключевая идея — отказ от классической **shared-nothing** архитектуры (жёсткая связка вычислений и хранилища) в пользу **трёхслойной SOA**: независимые слои хранения, вычислений и облачных сервисов, общающиеся через REST.

## Содержание репозитория

| Файл | Описание |
|------|----------|
| [`snowflake.pptx`](snowflake.pptx) | Презентация (слайды) |
| [`snowflake.pdf`](snowflake.pdf) | PDF-версия презентации |
| [`snowflake_architecture.md`](snowflake_architecture.md) | Полный текст доклада с таймингом по слайдам |
| [`architercture.png`](architercture.png) | Схема архитектуры Snowflake |

## Структура презентации

1. **Вводный** — контекст статьи и зачем она важна для архитектора ПО
2. **Проблема и контекст** — ограничения shared-nothing в облаке
3. **Трёхслойная SOA** — Database Storage (S3), Query Processing (EC2), Cloud Services
4. **Слой хранения** — PAX, иммутабельные файлы, MVCC, min-max pruning
5. **Виртуальные Warehouses** — изоляция, ephemeral processes, consistent hashing, file stealing
6. **Cloud Services** — Cascades-оптимизатор, online upgrade без даунтайма
7. **ELT и execution engine** — columnar, vectorized, push-based исполнение
8. **Выводы** — уроки для проектирования ПО и влияние на индустрию

## Основные архитектурные решения

- **Shared storage + independent compute** — данные в S3, вычисления в эфемерных кластерах EC2
- **Иммутабельность** — следствие ограничений S3; упрощает MVCC, отказоустойчивость и кэширование
- **Разделение жизненных циклов** — общий слой хранения и сервисов vs. краткоживущие warehouses под конкретную нагрузку
- **Осознанные trade-offs** — lazy consistent hashing, отказ от индексов, pruning вместо B+-tree

## Источник

Оригинальная статья: [The Snowflake Elastic Data Warehouse (CMU)](https://www.cs.cmu.edu/~15721-f24/papers/Snowflake.pdf)
