# VibeCoder System

AI-агент для поиска и выигрыша заказов на Profi.ru — специализация: логистика, планирование, снабжение, аналитика.

## Что делает система

- Мониторит Profi.ru и другие площадки
- Фильтрует задачи по нише и сложности (оставляет только подходящие)
- Анализирует клиента в 5 слоёв: лексика, структура, эмоции, тип личности (DISC), истинная потребность
- Генерирует персонализированный ответ под тип клиента (4 шаблона)
- Создаёт репозиторий с прототипом в GitHub
- Отправляет уведомление в Telegram с досье на клиента и готовым откликом

## Стек

- **Парсинг** — Playwright (Python) — нужен для JS-рендеринга Profi.ru
- **Оркестратор** — n8n
- **AI** — OpenAI API
- **Хранилище** — Supabase (PostgreSQL)
- **Репозитории** — GitHub API
- **Уведомления** — Telegram Bot API
- **Дашборд** — HTML + Alpine.js → GitHub Pages

## Структура проекта

```
vibecoder-system/
├── .env                        # API ключи — НЕ пушить в git!
├── .gitignore
├── config.yaml                 # Настройки: задержки, ключевые слова
├── requirements.txt
├── src/
│   ├── parser/                 # Парсинг Profi.ru через Playwright
│   ├── analyzer/               # AI-анализ клиента и фильтрация
│   ├── templates/              # Генерация ответов
│   ├── integrations/           # GitHub API, Telegram Bot
│   └── dashboard/              # Фронтенд (index.html → GitHub Pages)
├── tests/
└── n8n/workflow.json           # Экспорт воркфлоу n8n
```

## Быстрый старт

### 1. Клонировать репо и установить зависимости

```bash
git clone https://github.com/твойник/vibecoder-system.git
cd vibecoder-system
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
playwright install chromium
```

### 2. Настроить переменные окружения

```bash
cp .env.example .env
# Открыть .env и вставить свои ключи
```

### 3. Запустить скрипт разведки (первым делом!)

```bash
python src/parser/explore.py
```

Скрипт сохранит HTML-структуру Profi.ru — нужно для написания парсера без ошибок.

### 4. Запустить дашборд локально

Открыть `src/dashboard/index.html` в браузере. Для деплоя — см. раздел ниже.

## Деплой дашборда на GitHub Pages

1. В репозитории: **Settings → Pages**
2. Branch: `main`, папка: `/ (root)` или `/docs`
3. Save — через минуту сайт появится на `https://твойник.github.io/vibecoder-system`

Если кладёшь дашборд в корень — переименуй в `index.html`.
Если в папку `/docs` — в настройках Pages выбери `/docs`.

## Настройка

Все параметры в `config.yaml`:

```yaml
filters:
  complexity_max: 4       # максимальная сложность задачи (1-10)
  budget_min: 5000        # минимальный бюджет в рублях

scraper:
  delay_min: 5            # задержка между запросами (секунды)
  delay_max: 15
  rotate_useragent: true
```

## Важно

- Файл `.env` никогда не должен попасть в git — он в `.gitignore`
- Перед написанием парсера обязательно запустить `explore.py` и изучить HTML
- Уважай задержки между запросами — Profi.ru может заблокировать при агрессивном парсинге
- Проверь `robots.txt` и Terms of Service перед запуском

## Лицензия

MIT — см. `LICENSE`
