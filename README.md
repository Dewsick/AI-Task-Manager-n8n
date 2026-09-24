# AI Task Manager

AI-powered Telegram task manager built with n8n, OpenRouter and JavaScript.

## Что это

AI Task Manager позволяет создавать и управлять задачами через Telegram с помощью обычных сообщений.

Пользователь может написать, например:

> Завтра в 15:00 нужно позвонить в банк и узнать про перевод.

AI определяет:

* задачу;
* приоритет;
* дату;
* время;
* категорию.

После обработки задача сохраняется в Data Table.

## Возможности

* создание задач через естественный язык;
* автоматическое определение даты и времени;
* определение приоритета;
* автоматическая категоризация задач;
* поддержка относительного времени, например «через 10 минут»;
* команды `/tasks` и `/today`;
* выполнение задач через `/done N`;
* удаление выполненных задач через `/delete`;
* удаление конкретной задачи по ID;
* автоматические напоминания;
* случайное вечернее время напоминания, если время задачи не указано;
* защита от повторной отправки напоминания;
* обработка неизвестных команд и некорректных аргументов;
* отдельная обработка ошибок workflow.

## Команды

| Команда      | Назначение                 |
| ------------ | -------------------------- |
| `/start`     | Запуск бота                |
| `/tasks`     | Список активных задач      |
| `/today`     | Задачи на сегодня          |
| `/done N`    | Выполнить задачу №N        |
| `/delete`    | Удалить выполненные задачи |
| `/delete ID` | Удалить задачу по ID       |
| `/help`      | Список команд              |

## Архитектура

### Основной workflow

Telegram Trigger принимает сообщение и передаёт его в Switch.

В зависимости от команды выполняется соответствующая ветка:

```text
Telegram Trigger
       ↓
     Switch
       ├── /start
       ├── /tasks
       ├── /today
       ├── /done N
       ├── /delete [ID]
       ├── /help
       └── AI Task Processing
```

### AI обработка

```text
Message
   ↓
Edit Fields
   ↓
AI Agent
   ↓
JavaScript JSON Parser
   ↓
Data Table
```

AI работает с московским часовым поясом Europe/Moscow.

### Напоминания

```text
Schedule Trigger
       ↓
Data Table
       ↓
JavaScript
       ↓
Check reminder time
       ↓
Telegram
       ↓
Update task
```

Если время выполнения не указано, система один раз создаёт случайное время для вечернего напоминания и сохраняет его.

## Технологии

* n8n
* Telegram Bot API
* OpenRouter API
* JavaScript
* Data Tables
* Docker
* Docker Compose
* Tailscale Funnel

## Скриншоты

### Основной workflow

![Main workflow](screenshots/main-workflow.png)

### AI Agent

![AI Agent](screenshots/ai-agent.png)

### Система напоминаний

![Reminders](screenshots/reminders.png)

### Telegram

![Telegram](screenshots/telegram.png)

## Запуск

Проект предназначен для запуска в self-hosted n8n.

Workflow можно импортировать в n8n из JSON-файлов из директории `workflows`.

Перед запуском необходимо самостоятельно настроить:

* Telegram credentials;
* OpenRouter credentials;
* Data Tables;
* переменные окружения;
* публичный webhook URL при необходимости.

## Структура проекта

```text
AI-Task-Manager-n8n/
│
├── README.md
│
├── workflows/
│   ├── ai-task-manager.json
│   └── reminders.json
│
└── screenshots/
    ├── main-workflow.png
    ├── ai-agent.png
    ├── reminders.png
    └── telegram.png
```
