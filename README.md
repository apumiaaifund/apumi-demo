# Apumi — Engineering Filter · Public Demo

> **Формулы не маскируются.**
>
> Ø25 f7, Ra 0.8, M8x1.25 доходят до LLM целыми. PII — вырезается. Инъекции — 403.

**Фильтр инженерных данных для LLM.** Обычный PII-фильтр видит инженерные обозначения как «цифры» и маскирует их — инженер получает бессмысленный ответ. Apumi — различает.

## Что доказывает демо

| Файл | Что показывает |
|---|---|
| request_engineering.json / response_engineering.json | Формулы сохранены, LLM отвечает осмысленно |
| request_injection.json / response_403.json | Инъекция заблокирована |
| request_sensitive.json / response_sensitive.json | Чертёж → CPU-модель (routing) |
| corpus_measurement.json | 573 строки, 5 стандартов, 100% |

## Замер (воспроизводимо)

| Стандарт | Источник | Строк | Сохранность |
|---|---|---|---|
| ISO 286 (посадки) | chenzefeng09/iso-286-fits (MIT) | 260 | 100% |
| GB/T 1800 (=ISO 286-1) | zhangsanbin/Tolerance (XML) | 253 | 100% |
| ISO 1302 (шероховатость) | makenexa/engineering-reference-data (CSV) | 12 | 100% |
| ISO 261 (резьба) | clay-good/anvilate (CC0) | 20 | 100% |
| ISO 2768 (общие допуски) | clay-good/anvilate (CC0) | 30 | 100% |
| **Итого** | | **573** | **100.0%** |

_synthetic: false. Все источники машиночитаемые.

## Как проверить

    docker-compose up -d
    curl http://127.0.0.1:8000/health
    curl -X POST http://127.0.0.1:8000/v1/chat/completions -H "Content-Type: application/json" -d "{\"messages\":[{\"role\":\"user\",\"content\":\"Ø25 f7\"}]}"

## Тесты

**29 passed**: 22 на формулы + 7 на API.

## Что не доказано (честно)

- Твёрдость HRC 45 / 220 HB — только юнит-тесты, machine-readable источника нет.
- NER (ФИО) — Stage 4.
- Кэш — in-memory O(n), не Redis/HNSW.

## Кому

| Сегмент | Боль |
|---|---|
| Precision engineering / полупроводники (SG) | CAD/PLM в облако — нельзя, ИБ блокирует |
| Промышленная автоматизация (UAE) | PDPL data residency |
| Инженерные консультанты | Нужен LLM, но формулы теряются в PII-фильтре |

## Регуляторный триггер

- Сингапур: MAS AI Risk Management Guidelines (7 Oct 2026, в силе 2027).
- ОАЭ: PDPL data residency + sovereign AI (Feb 2026).

## Лицензия

MIT. См. LICENSE.

## Private repo

Полный код: github.com/apumiaaifund/apumi (private).
