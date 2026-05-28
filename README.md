# 🤝 HR Bot — Recruitment Automation (Telegram + n8n Form)

An HR process automation system built on [n8n](https://n8n.io). Consists of two connected workflows: a Telegram bot for candidates and an AI agent that processes resumes.

## Screenshots

### Candidate conversation in Telegram
![Dialog 1](images/dialog1.png)
![Dialog 2](images/dialog2.png)

### Application form
![Form](images/form.png)

### Workflow in n8n
![Workflow](images/workflow.png)

### Candidates in Google Sheets
![Google Sheets](images/sheets.png)

## How the System Works

```
Candidate messages the Telegram bot
        │
        ▼
[Telegram HR Bot] — validates the request and sends a form link
        │
        ▼
Candidate fills in the form (name, resume, details)
        │
        ▼
[HR Bot] — processes the application:
        ├─ AI agent analyzes the resume (Gemini / Ollama)
        ├─ Resume is uploaded to Google Drive
        ├─ Candidate data is saved to Google Sheets
        ├─ Email notification sent to HR manager via Gmail
        └─ Telegram notification sent to HR team
```

## Workflows

### 1. `workflow_telegram_bot.json` — Telegram HR Bot
Entry point for candidates. Receives a Telegram message, validates the request type, and sends a personalized application form link.

| Node | Purpose |
|------|---------|
| Telegram Trigger | Receives messages from candidates |
| If / If1 | Checks request type |
| Build Form URL | Generates form link with parameters |
| Send a text message | Sends reply / form link to candidate |

### 2. `workflow_hr_form.json` — HR Form Processor
Core application processing workflow. Triggered when a candidate submits the form.

| Node | Purpose |
|------|---------|
| On form submission | Receives form data |
| AI Agent | Analyzes candidate's resume |
| Google Gemini / Ollama | LLM models for analysis |
| Structured Output Parser | Structures AI output |
| Upload file (Google Drive) | Saves resume to Drive |
| Append row in sheet | Logs candidate to Google Sheets |
| Send a message (Gmail) | Sends email to HR manager |
| Send a text message (Telegram) | Notifies HR team in Telegram |

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Automation | n8n |
| Messaging | Telegram Bot API |
| AI model | Google Gemini / Ollama |
| File storage | Google Drive |
| Candidate database | Google Sheets |
| Email notifications | Gmail |

## Setup

1. **Import both workflows** — start with `workflow_hr_form.json`, then `workflow_telegram_bot.json`
2. **Configure credentials:**
   - Telegram Bot token
   - Google Gemini API key (or local Ollama)
   - Google Drive, Google Sheets, Gmail — via OAuth2
3. **Copy the form URL** from `On form submission` node and paste it into `Build Form URL` in the Telegram bot
4. **Update parameters:**
   - Google Sheets ID in `Append row` nodes
   - Google Drive folder ID in `Upload file` node
   - HR manager email in `Send a message` node
   - HR team chat ID in Telegram node
5. **Activate both workflows**

## Author

[Talgat Rashit](https://github.com/rasittalgat-alt)

---

# 🤝 HR-бот — Автоматизация найма (Telegram + n8n Form)

Система автоматизации HR-процессов на базе [n8n](https://n8n.io). Состоит из двух связанных воркфлоу: Telegram-бот для кандидатов и AI-агент для обработки резюме.

## Скриншоты

### Диалог с кандидатом в Telegram
![Диалог 1](images/dialog1.png)
![Диалог 2](images/dialog2.png)

### Форма подачи заявки
![Форма](images/form.png)

### Воркфлоу в n8n
![Воркфлоу](images/workflow.png)

### Кандидаты в Google Sheets
![Google Sheets](images/sheets.png)

## Как устроена система

```
Кандидат пишет в Telegram
        │
        ▼
[Telegram HR Bot] — проверяет запрос и отправляет ссылку на форму
        │
        ▼
Кандидат заполняет форму (имя, резюме, данные)
        │
        ▼
[HR Bot (RU)] — обрабатывает заявку:
        ├─ AI-агент анализирует резюме (Gemini / Ollama)
        ├─ Резюме загружается в Google Drive
        ├─ Данные кандидата сохраняются в Google Sheets
        ├─ HR-менеджеру отправляется письмо на Gmail
        └─ Уведомление в Telegram HR-команде
```

## Воркфлоу

### 1. `workflow_telegram_bot.json` — Telegram HR Bot
Точка входа для кандидатов. Получает сообщение в Telegram, проверяет тип запроса и отправляет персонализированную ссылку на форму подачи заявки.

| Узел | Назначение |
|------|------------|
| Telegram Trigger | Получает сообщения от кандидатов |
| If / If1 | Проверяет тип запроса |
| Build Form URL | Формирует ссылку на форму с параметрами |
| Send a text message | Отправляет ответ / ссылку кандидату |

### 2. `workflow_hr_form.json` — HR Bot (RU)
Основной воркфлоу обработки заявок. Запускается при отправке формы кандидатом.

| Узел | Назначение |
|------|------------|
| On form submission | Получает данные из формы |
| AI Agent | Анализирует резюме кандидата |
| Google Gemini / Ollama | LLM-модели для анализа |
| Structured Output Parser | Структурирует вывод AI |
| Upload file (Google Drive) | Сохраняет резюме в Drive |
| Append row in sheet | Логирует кандидата в Google Sheets |
| Send a message (Gmail) | Отправляет письмо HR-менеджеру |
| Send a text message (Telegram) | Уведомляет HR-команду в Telegram |

## Стек

| Компонент | Технология |
|-----------|-----------|
| Автоматизация | n8n |
| Мессенджер | Telegram Bot API |
| AI-модель | Google Gemini / Ollama |
| Хранилище файлов | Google Drive |
| База кандидатов | Google Sheets |
| Email-уведомления | Gmail |

## Установка и настройка

1. **Импортируй оба воркфлоу** в n8n — сначала `workflow_hr_form.json`, затем `workflow_telegram_bot.json`
2. **Настрой учётные данные:**
   - Telegram Bot token
   - Google Gemini API-ключ (или локальный Ollama)
   - Google Drive, Google Sheets, Gmail — через OAuth2
3. **Скопируй URL формы** из узла `On form submission` и вставь в `Build Form URL` в Telegram-боте
4. **Обнови параметры:**
   - ID Google Sheets таблицы в узлах `Append row`
   - ID папки Google Drive в узле `Upload file`
   - Email HR-менеджера в узле `Send a message`
   - Chat ID HR-команды в узле Telegram
5. **Активируй оба воркфлоу**

## Автор

[Talgat Rashit](https://github.com/rasittalgat-alt)
