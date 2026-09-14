# BEZNIGATIVA — подробное техническое задание
## Creator / Artist / Production Operating System

**Версия:** 1.0
**Дата:** 13 сентября 2026
**Язык интерфейса:** русский
**Основные каналы:** Telegram Bot + Telegram Mini App + Web App
**Основная рабочая среда:** браузер
**Концепция визуального слоя:** Black Liquid Glass + интерактивный пиксельный 3D/2.5D офис
**Статус документа:** базовая спецификация для реализации агентом-разработчиком

---

## 0. КРИТИЧЕСКОЕ ПРАВИЛО ДЛЯ АГЕНТА

Этот документ является не предложением, а **контрактом реализации**.

Агент-разработчик обязан:

1. Не менять продуктовую логику по собственной инициативе.
2. Не заменять заявленную архитектуру на упрощённый аналог без явной причины.
3. Не делать «демо», если требуется рабочая функциональность.
4. Не использовать моковые данные вместо реального backend после появления соответствующего модуля.
5. Не смешивать Telegram-логику, UI и бизнес-логику в одном файле/слое.
6. Не доверять данным из браузера без серверной проверки.
7. Не делать глобальную realtime-подписку на все таблицы. Подписки должны быть адресными.
8. Не отправлять Telegram-сообщения напрямую из UI. Все исходящие сообщения проходят серверный Notification/Telegram слой.
9. Не хранить Telegram Bot Token, AI API keys, database secret keys и другие секреты в клиентском коде.
10. Не строить критические операции только на optimistic UI. После операции должна быть серверная фиксация состояния.
11. Все тексты пользовательского интерфейса должны быть на русском языке, кроме технических идентификаторов в коде.
12. Все загрузочные состояния, пустые состояния, ошибки, подтверждения и success-состояния должны быть продуманы отдельно.
13. Нельзя добавлять визуальные эффекты, которые ухудшают читаемость, производительность или мобильную работу.
14. Нельзя превращать интерфейс в перегруженный «CRM из 2015 года».
15. Нельзя заменять интерактивный офис одной статичной картинкой. Офис должен быть компонентом интерфейса и отражать состояние пользователя.
16. Нельзя блокировать основное приложение ожиданием тяжёлой 3D-сцены, AI-анализа, больших файлов или второстепенных API.
17. Любая спорная реализационная деталь должна оформляться как ADR/решение с объяснением, а не молча изменяться.
18. При отсутствии данных использовать честное пустое состояние, а не придуманную статистику.
19. Все критические действия должны иметь audit log.
20. Код должен быть typed, валидируемым, тестируемым и пригодным для дальнейшего расширения.

---

# 1. ПРОДУКТОВАЯ КОНЦЕПЦИЯ

BEZNIGATIVA — это единая рабочая платформа для набора, развития и управления блогерами, музыкантами, артистами и внутренним персоналом продакшна.

Продукт состоит из двух больших контуров:

### 1.1. Внешний контур

Для людей, которые только знакомятся с проектом:

- Telegram Bot;
- открытый Telegram Mini App;
- регистрация;
- анкета;
- база знаний / Academy;
- AI-помощник для развития;
- подача заявки в команду;
- уведомления по заявке.

### 1.2. Внутренний контур

Для одобренных участников и команды:

- личный кабинет;
- рабочее пространство;
- интерактивный офис;
- статус присутствия;
- проекты;
- задачи;
- календарь;
- контент;
- документы;
- возможности / кастинги / реклама / коллаборации;
- личные и проектные чаты;
- AI Copilot;
- аналитика;
- уведомления;
- Telegram-синхронизация.

---

# 2. КЛЮЧЕВОЙ USER FLOW

Основной путь нового человека:

```text
Telegram
  ↓
/start
  ↓
Приветствие
  ↓
Заполнить профиль
  ↓
Открытый кабинет / Mini App
  ↓
Изучить материалы / AI
  ↓
«Хочу работать с BEZNIGATIVA»
  ↓
Анкета
  ↓
Отправка заявки
  ↓
Заявка попадает в Admin Inbox
  ↓
Менеджер обрабатывает заявку
  ↓
Статус изменяется
  ↓
Система отправляет сообщение в Telegram
  ↓
При одобрении → onboarding
  ↓
Доступ к Team Workspace
```

Путь внутреннего сотрудника/участника:

```text
Telegram / Web
  ↓
Авторизация
  ↓
Рабочий кабинет
  ↓
Интерактивный офис
  ↓
«Я на рабочем месте»
  ↓
Presence = ONLINE / WORKING
  ↓
Получение задач / проектов
  ↓
Работа
  ↓
Сдача
  ↓
Проверка менеджером
  ↓
APPROVED или REVISION
  ↓
Результат автоматически синхронизируется с Telegram
```

---

# 3. ОСНОВНАЯ АРХИТЕКТУРА

Рекомендуемая архитектура:

```text
                       TELEGRAM
                  ┌───────────────┐
                  │ Bot + MiniApp │
                  └───────┬───────┘
                          │
                          │ HTTPS / Telegram API
                          ▼
┌──────────────────────────────────────────────────────┐
│                NEXT.JS APPLICATION                   │
│                                                      │
│  Web UI │ Mini App UI │ Server Actions / API        │
│  Auth   │ RBAC        │ Domain Services              │
└───────────────────────┬──────────────────────────────┘
                        │
            ┌───────────┼────────────┐
            ▼           ▼            ▼
      PostgreSQL     Storage     Realtime
            │           │            │
            └───────────┼────────────┘
                        ▼
                 Event / Domain Layer
                        │
         ┌──────────────┼───────────────┐
         ▼              ▼               ▼
   Telegram Service   AI Service    Notifications
         │              │               │
         └──────────────┼───────────────┘
                        ▼
                     Audit Log
```

---

# 4. ТЕХНОЛОГИЧЕСКИЙ СТЕК

## 4.1. Frontend / Full-stack framework

### Next.js 16.3+

Использовать **App Router**.

Next.js 16.3 на текущий момент является актуальной веткой, а релиз включает улучшения навигации, streaming, caching и более быстрые сценарии приложения. Использовать именно современную архитектуру App Router, Server Components и клиентские компоненты только там, где требуется интерактивность.

Официальные материалы:
- https://nextjs.org/docs
- https://nextjs.org/blog

### TypeScript

Strict mode обязателен.

Запрещено:

- `any` без обоснования;
- невалидированные `unknown`;
- неявные nullable значения;
- бизнес-логика без типов.

---

## 4.2. UI

Рекомендуемый стек:

- React 19.x-compatible stack;
- Tailwind CSS;
- shadcn/ui как базовые headless/unstyled primitives;
- Radix primitives, где нужны accessibility-first компоненты;
- Motion для анимаций;
- Lucide Icons либо единый icon set.

UI-палитра и визуальный язык должны быть собственными, а не копировать готовый продукт.

---

# 5. 3D / ПИКСЕЛЬНЫЙ ОФИС

## 5.1. Основная идея

В интерфейсе внутреннего кабинета должен быть интерактивный офис.

Не использовать полноценно фотореалистичную 3D-графику.

Целевая стилистика:

**pixel office / low-poly / isometric / 3D-like / cozy production studio**.

Образ:

- тёмный офис;
- небольшие рабочие столы;
- мониторы;
- диван;
- зона отдыха;
- студия;
- монтажный стол;
- музыкальная зона;
- переговорная;
- доска задач;
- маленькие персонажи сотрудников;
- мягкое свечение экранов;
- лёгкие анимации;
- различные состояния персонажа.

---

## 5.2. Главная механика офиса

Каждый внутренний пользователь имеет персонажа.

Персонаж связан с профилем.

Состояния:

```text
OFFLINE
ONLINE
WORKING
IN_MEETING
ON_BREAK
AWAY
BUSY
DO_NOT_DISTURB
```

Состояние должно отображаться:

- в офисе;
- в профиле;
- в Presence;
- в командных списках;
- при необходимости в Telegram.

---

## 5.3. Кнопка «Я на рабочем месте»

Основное действие рабочего дня:

**[ Я на рабочем месте ]**

После клика:

1. сервер фиксирует `work_session.started`;
2. user presence = `WORKING`;
3. персонаж появляется/переходит в рабочую позицию;
4. стартует рабочая сессия;
5. в activity feed появляется событие;
6. администратор видит сотрудника как активного;
7. другие участники получают realtime-обновление;
8. Telegram не должен спамить пользователя сообщениями. Telegram используется только для значимых уведомлений.

Кнопка сменяется на:

**[ Завершить рабочий день ]**

После завершения:

- фиксируется end time;
- рассчитывается длительность;
- presence меняется;
- персонаж уходит / завершает рабочую анимацию;
- создаётся activity event.

---

## 5.4. Точное требование к 3D

Офис должен быть **progressively enhanced**.

При отсутствии WebGL / слабом устройстве:

- не ломать кабинет;
- автоматически снижать качество;
- отключать вторичные эффекты;
- переходить к облегчённой 2.5D / sprite-версии;
- в крайнем случае показывать статичный low-motion вариант.

Нельзя делать 3D обязательным условием работы системы.

---

## 5.5. Рендеринг

Предпочтительный вариант:

### React Three Fiber + Three.js

Использовать для интерактивной сцены, камер, персонажей, объектов и анимаций.

Three.js предоставляет WebGL renderer и инструменты для управления ресурсами, render targets и контролем pixel ratio; renderer имеет параметры для low/high power устройств. Не использовать тяжёлые постэффекты без необходимости.

Официально:
- https://threejs.org/docs/

Альтернативный вариант для особо лёгкой pixel-сцены:

### PixiJS

Использовать, если визуально выбран спрайтовый 2D/2.5D стиль. PixiJS ориентирован на высокопроизводительный GPU-рендеринг; для production-дизайна предпочтителен WebGL renderer, а WebGPU оставлять как optional enhancement, поскольку официальная документация отмечает его как более новый, но ещё менее зрелый путь.

Официально:
- https://pixijs.com/8.x/guides/components/renderers

Агент должен выбрать один движок после создания минимального prototype scene и объяснить выбор в ADR.

---

# 6. ВАЖНОЕ РАЗДЕЛЕНИЕ: ОФИС ≠ ОСНОВНОЙ UI

Офис является визуальным слоем и dashboard surface.

Он **не заменяет**:

- задачи;
- списки;
- таблицы;
- календарь;
- чат;
- поиск;
- forms.

Пользователь должен иметь возможность скрыть/свернуть офис:

**[ Свернуть офис ]**

После сворачивания основное рабочее пространство становится более компактным.

На мобильных устройствах офис может быть уменьшен до блока/карты или переключаться в simplified mode.

---

# 7. TELEGRAM — ЦЕНТРАЛЬНЫЙ КАНАЛ ИДЕНТИЧНОСТИ

Telegram не является просто каналом рассылки. Он является одним из основных идентификаторов пользователя.

Система должна связать:

```text
Telegram User ID
        ↕
Internal User ID
        ↕
Profile
        ↕
Web Session
        ↕
Mini App Session
```

Один человек должен иметь одну внутреннюю сущность пользователя.

---

# 8. TELEGRAM BOT

Обязательные команды:

```text
/start
/help
/profile
/application
/tasks
/notifications
```

Но основное взаимодействие должно происходить не через огромное количество inline-кнопок, а через открытие Mini App.

Главная кнопка:

**Открыть кабинет**

Также использовать menu button Web App.

Telegram Bot API поддерживает `MenuButtonWebApp`, `WebAppInfo`, `web_app_data` и другие элементы Web App интеграции.

Официально:
- https://core.telegram.org/bots/api

---

# 9. TELEGRAM MINI APP

Mini App должен использовать тот же backend и тот же domain layer, что и web-приложение.

Запрещено создавать отдельную бизнес-логику Mini App.

Архитектура:

```text
Shared Domain Logic
       │
 ┌─────┴─────┐
 ▼           ▼
Web UI    Mini App UI
```

Mini App должен быть адаптирован под узкий экран:

- нижняя навигация;
- быстрые действия;
- task cards;
- profile;
- notifications;
- application status;
- AI.

---

# 10. TELEGRAM AUTHENTICATION

Нельзя считать любой `user_id`, присланный браузером, доказательством личности.

Telegram Web App передаёт `initData`, которое должно валидироваться на сервере.

Алгоритм:

```text
Telegram Web App
      ↓
initData
      ↓
HTTPS request to server
      ↓
Server verifies Telegram signature/hash
      ↓
Extract Telegram user identity
      ↓
Find/create internal user
      ↓
Issue internal session
```

Нельзя выполнять доверенную авторизацию исключительно на клиенте.

Также нельзя доверять `web_app_data`: Telegram Bot API прямо предупреждает, что клиент может отправить произвольные данные в этом поле, поэтому сервер должен проверять источник и смысл операции.

Официальная документация:
- https://core.telegram.org/bots/api
- https://core.telegram.org/bots/webapps

---

# 11. СИНХРОНИЗАЦИЯ TELEGRAM ↔ WEB

Главный принцип:

> **Все каналы управляют одной внутренней сущностью данных.**

Пример:

Админ в браузере меняет:

`application.status = REVIEW`

Событие:

```text
application.status_changed
```

Далее:

```text
Database
 ↓
Event Bus / Domain Event
 ├── Realtime → Admin UI
 ├── Realtime → Candidate UI
 ├── Notification Engine → Telegram
 └── Audit Log
```

Пользователь получает Telegram:

> Ваша заявка теперь находится на рассмотрении.

---

# 12. REALTIME

Realtime нужен для:

- чата;
- уведомлений;
- статусов заявок;
- задач;
- присутствия;
- пиксельного офиса;
- activity feed;
- live dashboard;
- review updates.

Рекомендуемый слой:

### Supabase Realtime

Использовать комбинацию:

- Broadcast — для адресных low-latency событий;
- Presence — для online/working/away состояний;
- Postgres Changes — только там, где подходит более простой сценарий.

Актуальная документация Supabase рекомендует Broadcast для масштабируемых и защищённых сценариев, а Presence предназначен для медленно изменяющегося состояния вроде online/offline/current page.

Официально:
- https://supabase.com/docs/guides/realtime
- https://supabase.com/docs/guides/realtime/subscribing-to-database-changes
- https://supabase.com/docs/guides/realtime/presence

---

# 13. REALTIME-ПРАВИЛА

Запрещено:

```text
subscribe('*')
```

для всего приложения.

Подписки должны быть ограничены контекстом:

```text
user:{userId}
project:{projectId}
task:{taskId}
application:{applicationId}
workspace:{workspaceId}
```

Private channels обязательны для приватных данных.

Каждый channel должен иметь authorization policy.

Presence не использовать для высокочастотной передачи координат/анимаций каждого кадра.

Пиксельный офис должен синхронизировать **состояния**, а локальная анимация выполняется клиентом.

---

# 14. СТРУКТУРА ДОМЕННЫХ СОБЫТИЙ

Использовать единый формат:

```text
entity.action
```

Примеры:

```text
application.created
application.submitted
application.status_changed
application.reviewed

creator.approved
creator.rejected
creator.paused

work_session.started
work_session.ended
presence.changed

project.created
project.member_added
project.status_changed

 task.created
task.assigned
task.submitted
task.approved
task.revision_requested
task.overdue

message.created
notification.created

document.created
document.sent
document.viewed
document.signed
```

Каждое событие должно иметь:

- event id;
- timestamp;
- actor id;
- entity type;
- entity id;
- payload version;
- correlation id.

---

# 15. БАЗА ДАННЫХ

Рекомендуемый основной storage:

### PostgreSQL

Основные сущности:

```text
users
profiles
roles
user_roles
creator_profiles
social_accounts

applications
application_answers
application_reviews
application_status_history

projects
project_members
project_roles

work_sessions
presence_states

 tasks
task_assignees
task_checklists
task_comments
task_files
task_reviews

opportunities
opportunity_applications

messages
message_threads
message_participants

notifications
notification_preferences
notification_deliveries

academy_categories
academy_courses
academy_lessons
academy_progress

media_assets
files

documents
contracts

ai_conversations
ai_messages
ai_tool_calls

activity_events
audit_logs
system_settings
feature_flags
```

---

# 16. ИДЕНТИФИКАТОРЫ

Использовать UUID/ULID для внутренних entity IDs.

Telegram user ID хранить отдельно как external identifier.

Не использовать Telegram ID как primary key внутренних таблиц.

---

# 17. ROLES / RBAC

Минимальные роли:

```text
SUPER_ADMIN
ADMIN
MANAGER
PRODUCER
MODERATOR
CREATOR
MUSICIAN
BLOGGER
ARTIST
STAFF
```

Роль и тип профиля — не одно и то же.

Например:

```text
role = CREATOR
profile_type = MUSICIAN
```

или:

```text
role = CREATOR
profile_type = BLOGGER
```

---

# 18. PERMISSIONS

Не проверять права только по названию страницы.

Разрешения должны быть action-oriented:

```text
users.read
users.update
users.delete

applications.read
applications.review
applications.update

projects.create
projects.read
projects.update
projects.delete

 tasks.create
tasks.assign
tasks.review
tasks.approve

documents.read
documents.manage

messages.read
messages.send

analytics.read
settings.manage
```

Каждый server action / API route обязан выполнять permission check.

---

# 19. ОТКРЫТАЯ ЗОНА ДЛЯ НОВИЧКОВ

После регистрации пользователь получает понятный интерфейс.

Основные разделы:

```text
Главная
Как всё работает
Академия
AI-помощник
Мои материалы
Подать заявку
Профиль
```

Основное сообщение:

> BEZNIGATIVA помогает авторам, блогерам и артистам развиваться, создавать контент и участвовать в реальных проектах.

Не перегружать человека внутренними терминами.

---

# 20. АКАДЕМИЯ

Категории:

- блогинг;
- short-form video;
- сценарии;
- личный бренд;
- музыка;
- продвижение;
- монетизация;
- реклама;
- коллаборации;
- production basics.

Каждый материал:

```text
title
cover
category
reading_time
content
attachments
related_lessons
```

Прогресс:

```text
not_started
in_progress
completed
```

---

# 21. ПОДАЧА ЗАЯВКИ

Кнопка:

# ХОЧУ В КОМАНДУ

Форма должна состоять из понятных шагов.

### Шаг 1. Основное

- имя;
- никнейм;
- город/регион по желанию;
- дата рождения только если действительно нужна для бизнес-процесса;
- тип участника.

### Шаг 2. Социальные площадки

- Telegram;
- Instagram;
- TikTok;
- YouTube;
- VK;
- другие.

### Шаг 3. Контент

- направление;
- форматы;
- опыт;
- сильные стороны;
- примеры работ.

### Шаг 4. Мотивация

- почему хочешь работать с командой;
- какие проекты интересны;
- что готов делать;
- доступность.

### Шаг 5. Медиа

- фото;
- видео;
- музыка;
- портфолио.

### Шаг 6. Подтверждение

Пользователь должен увидеть summary всей заявки до отправки.

---

# 22. СТАТУСЫ ЗАЯВОК

```text
DRAFT
SUBMITTED
NEW
IN_REVIEW
NEED_MORE_INFO
INTERVIEW
APPROVED
REJECTED
WAITLIST
ARCHIVED
```

Frontend labels:

```text
Черновик
Отправлена
Новая
На рассмотрении
Нужно уточнение
Собеседование
Одобрена
Отклонена
В резерве
Архив
```

---

# 23. ADMIN INBOX / ЧАТ ЗАЯВОК

Это один из центральных экранов.

Логика похожа на inbox/helpdesk.

Левая колонка:

```text
Все
Новые
В работе
Требуют ответа
Одобрены
Отклонены
Архив
```

По каждой заявке:

- avatar;
- имя;
- тип;
- время;
- статус;
- AI score;
- последний комментарий;
- unread indicator.

Правая часть:

- профиль;
- заявка;
- сообщения;
- внутренние заметки;
- история;
- действия.

Главные действия:

```text
Взять в работу
Запросить информацию
Назначить менеджера
На собеседование
Одобрить
Отклонить
Архивировать
```

---

# 24. СООБЩЕНИЯ ПО ЗАЯВКЕ

Система должна различать:

### Внешние сообщения

Видит кандидат.

### Внутренние заметки

Не видит кандидат.

Например:

```text
МЕНЕДЖЕР
«Сильный потенциал. Позвать на тестовую съёмку.»
```

Это internal note.

А сообщение кандидату:

> Спасибо! Ваша заявка находится на рассмотрении.

Это external message.

---

# 25. TELEGRAM УВЕДОМЛЕНИЯ ПО ЗАЯВКЕ

Сценарии:

### SUBMITTED

> ✅ Заявка получена.
>
> Мы получили твою заявку в BEZNIGATIVA. Сейчас команда её рассматривает.

### IN_REVIEW

> 👀 Заявка сейчас на рассмотрении у команды.

### NEED_MORE_INFO

> Нам нужна дополнительная информация по заявке. Открой кабинет, чтобы посмотреть запрос.

### INTERVIEW

> 🎬 Мы хотим продолжить знакомство. В твоём кабинете появились дальнейшие шаги.

### APPROVED

> 🎉 Ты принят в команду BEZNIGATIVA.
>
> Тебе открыт рабочий кабинет.

### REJECTED

Сообщение должно быть уважительным, без токсичного тона.

---

# 26. ЛИЧНЫЙ КАБИНЕТ УЧАСТНИКА

Основная навигация:

```text
Главная
Мой офис
Мои задачи
Проекты
Календарь
Возможности
Академия
Документы
Сообщения
AI-помощник
Профиль
```

---

# 27. ГЛАВНЫЙ ЭКРАН УЧАСТНИКА

Первый вопрос экрана:

> **Что мне делать сейчас?**

Верхний блок:

- приветствие;
- статус рабочего дня;
- current presence;
- основная задача.

Карточка:

```text
СЛЕДУЮЩЕЕ ДЕЙСТВИЕ

Подготовить первый вариант ролика
до 18:00

[ Открыть задачу ]
```

---

# 28. INTERACTIVE OFFICE DASHBOARD

Основная часть кабинета:

```text
┌─────────────────────────────────────────────┐
│                  OFFICE                     │
│                                             │
│     👤       💻       👤                    │
│                                             │
│          TABLE        TABLE                 │
│                                             │
│   🎬 STUDIO        🎧 MUSIC                 │
│                                             │
│          ☕ LOUNGE                          │
│                                             │
└─────────────────────────────────────────────┘
```

Персонажи должны иметь имя/tooltip при наведении.

Клик по персонажу открывает quick profile:

- имя;
- роль;
- online status;
- текущая задача;
- текущий проект.

Не показывать приватную информацию пользователям без permission.

---

# 29. ПИКСЕЛЬНЫЕ ПЕРСОНАЖИ

Персонажи должны иметь минимальный набор анимаций:

```text
idle
walk
sit
work
typing
phone
meeting
coffee
celebrate
leave
```

Анимации должны быть loopable и максимально дешёвыми для GPU.

Не использовать full-body physics.

Положение персонажа не обязано синхронизироваться каждый кадр.

Синхронизировать только semantic state:

```text
seat = editor_03
state = WORKING
```

Клиент сам проигрывает визуальную анимацию.

---

# 30. ADMIN DASHBOARD

Основная страница админа должна содержать:

### Верхняя панель

- глобальный поиск;
- command palette;
- уведомления;
- профиль;
- быстрые действия.

### KPI

- активные участники;
- сотрудники сейчас онлайн;
- новые заявки;
- задачи сегодня;
- задачи с просрочкой;
- активные проекты.

### Live activity

- кто вошёл;
- кто взял задачу;
- кто отправил работу;
- кто получил review;
- новые заявки.

### Attention

Автоматически показывать то, что требует действия.

---

# 31. COMMAND CENTER

Горячая клавиша:

`Ctrl/Cmd + K`

Поиск + команды.

Примеры:

```text
Найди блогеров с аудиторией больше 50 000
Покажи заявки за сегодня
Покажи просроченные задачи
Создай задачу
Открой проект BEZNIGATIVA SHOW
Покажи всех музыкантов
```

На первом этапе Command Center может выполнять структурированный поиск.

На втором — подключить AI tool calling.

---

# 32. ПРОЕКТЫ

Каждый проект содержит:

```text
Overview
Команда
Задачи
Календарь
Медиа
Документы
Чат
Активность
```

Project status:

```text
IDEA
PLANNING
ACTIVE
PAUSED
COMPLETED
ARCHIVED
```

---

# 33. ЗАДАЧИ

Task entity:

```text
title
description
project_id
creator_id
assignees
priority
status
due_at
attachments
checklist
review_state
created_by
created_at
updated_at
```

Статусы:

```text
TODO
IN_PROGRESS
WAITING_REVIEW
REVISION
APPROVED
DONE
CANCELLED
```

---

# 34. TASK REVIEW

Сценарий:

```text
Creator
  ↓
Submit
  ↓
WAITING_REVIEW
  ↓
Manager opens
  ↓
APPROVE / REVISION
```

При revision обязательно:

- комментарий;
- reason;
- timestamp.

Пример:

> Пересними первые 5 секунд. Нужен более сильный hook.

Участник получает:

- in-app notification;
- Telegram notification;
- изменение статуса задачи.

---

# 35. OPPORTUNITIES

Раздел для:

- рекламы;
- коллабораций;
- кастингов;
- шоу;
- музыкальных проектов;
- спецпроектов.

Opportunity:

```text
title
type
description
requirements
budget_range
deadline
slots
status
visibility
```

Участник нажимает:

**Откликнуться**

Заявка попадает менеджеру.

---

# 36. ДОКУМЕНТЫ

Раздел:

```text
Мои документы
Контракты
NDA
Рекламные договоры
Шаблоны
```

Документные состояния:

```text
DRAFT
SENT
VIEWED
SIGNED
EXPIRED
CANCELLED
```

На MVP можно сделать хранение файлов и статусную модель без полноценной электронной подписи.

---

# 37. ЧАТ

Три типа:

### Direct

Менеджер ↔ участник.

### Project

Команда проекта.

### Task

Рабочая переписка конкретной задачи.

Должны поддерживаться:

- текст;
- emoji;
- файлы;
- изображения;
- ссылки;
- reply;
- unread;
- typing indicator;
- online state;
- message status.

---

# 38. AI-СЛОЙ

AI не должен быть «одним большим чатом».

Должна существовать система агентов/skills.

Пример:

```text
AI COPILOT
 ├── Profile Analyst
 ├── Content Planner
 ├── Script Assistant
 ├── Growth Advisor
 ├── Application Analyst
 ├── Production Assistant
 └── Internal Workspace Assistant
```

---

# 39. AI TOOL ACCESS

AI получает только разрешённые инструменты.

Пример:

```text
get_user_profile
get_tasks
get_project
search_creators
search_applications
get_analytics
create_task
update_task
send_notification
search_academy
search_documents
```

Критические инструменты должны требовать confirmation.

Например:

AI может подготовить сообщение, но отправка пользователю должна быть отдельным permission/action.

---

# 40. AI ДЛЯ ЗАЯВОК

AI может анализировать заявку.

Вывод:

```text
Potential: HIGH
Content fit: HIGH
Activity: MEDIUM
Communication: HIGH
Uniqueness: MEDIUM

Recommendation:
INVITE TO INTERVIEW
```

AI не принимает окончательное кадровое решение автоматически.

Администратор должен иметь возможность игнорировать рекомендацию.

---

# 41. AI ДЛЯ CREATOR

AI знает только данные, которые ему разрешены.

Например:

> Ты начинающий блогер. Вот твои цели, активность и задачи.

AI может:

- предложить контент;
- расписать план;
- объяснить задачу;
- подготовить сценарий;
- разобрать результат.

---

# 42. NOTIFICATION ENGINE

Единый сервис:

```text
Domain Event
    ↓
Notification Rule
    ↓
Recipients
    ↓
Channels
 ├── In-App
 ├── Telegram
 └── Web Push (optional)
```

Примеры событий:

```text
task.assigned
task.revision_requested
task.approved
application.status_changed
project.member_added
message.created
document.sent
opportunity.created
deadline.soon
```

---

# 43. НЕ СПАМИТЬ TELEGRAM

Нельзя отправлять пользователю сообщение на каждое realtime-событие.

Например:

Не отправлять Telegram при каждом изменении presence.

Не отправлять Telegram при каждой внутренней заметке менеджера.

Telegram должен получать только значимые события.

---

# 44. ACTIVITY FEED

Activity feed должен объединять события проекта/команды.

Примеры:

> Алексей начал рабочий день.

> Мария отправила задачу «Сценарий шоу #12».

> Никита получил новую задачу.

> Заявка от @creator пришла на рассмотрение.

Каждая запись имеет actor, timestamp, type и entity link.

---

# 45. ПЕРСОНАЛЬНАЯ РАБОЧАЯ СЕССИЯ

Сущность:

```text
work_session

id
user_id
started_at
ended_at
source
status
created_at
```

`source`:

```text
WEB
MINI_APP
TELEGRAM
```

Один пользователь не должен иметь несколько активных рабочих сессий.

При конфликте устройств сервер должен разрешить его детерминированно.

---

# 46. СИНХРОНИЗАЦИЯ УСТРОЙСТВ

Пример:

Пользователь на компьютере нажал:

**Я на рабочем месте**

Через realtime на телефоне состояние тоже:

**Работаю**.

Если пользователь завершил работу на телефоне:

web должен получить событие и обновить состояние.

---

# 47. OFFLINE / RECONNECT

Обязательно предусмотреть:

```text
ONLINE
RECONNECTING
OFFLINE
DEGRADED
```

Если WebSocket отключился:

- UI показывает небольшой status indicator;
- приложение не ломается;
- изменения сохраняются после восстановления;
- critical action должен быть повторяемым/idempotent.

---

# 48. IDempotency

Особенно важно для:

- отправки заявок;
- отправки сообщений;
- создания задач;
- отправки Telegram notifications;
- start/end work session.

Пример:

Если пользователь дважды нажал «Отправить заявку», в базе остаётся одна логическая операция.

Использовать idempotency keys для mutation operations, где риск двойной отправки реален.

---

# 49. ФАЙЛЫ И МЕДИА

Не хранить видео/большие файлы в базе.

Использовать object storage.

Для файлов:

- mime type;
- size;
- checksum;
- owner;
- entity reference;
- visibility;
- created_at.

Чувствительные файлы отдавать через временные signed URLs.

---

# 50. PERFORMANCE

Цель:

> Приложение должно ощущаться быстрым даже при большом количестве данных.

Не допускать:

- massive client-side bundle;
- waterfall requests;
- blocking 3D initialization;
- загрузки всех creator records сразу;
- загрузки всех сообщений одной пачкой.

Использовать:

- Server Components там, где интерактивность не нужна;
- dynamic imports для тяжёлых client components;
- route-level code splitting;
- virtualization длинных списков;
- pagination/cursor pagination;
- parallel data fetching;
- cache/tag-based invalidation там, где подходит;
- Suspense и streaming;
- lazy load 3D;
- image optimization;
- prefetch маршрутов.

Next.js App Router поддерживает streaming через `loading.tsx` и Suspense, а route-based code splitting/prefetching помогает сделать навигацию быстрой. Эти механизмы нужно использовать осознанно, а не превращать каждый элемент страницы в отдельный loading state.

Официально:
- https://nextjs.org/learn/dashboard-app/streaming
- https://nextjs.org/learn/dashboard-app/navigating-between-pages

---

# 51. LOADING UX

Каждый экран должен иметь:

### Initial loading

Skeleton.

### Partial loading

Загружать независимые зоны независимо.

### Slow module

Показывать fallback.

### Error

Показывать понятную ошибку и retry.

### Empty

Показывать объяснение, почему данных нет и что делать.

Пример:

> Здесь пока нет задач.
>
> Как только менеджер назначит задачу, она появится здесь.

---

# 52. ERROR HANDLING

Нельзя показывать:

```text
Error 500
undefined
Something went wrong
```

в обычном пользовательском интерфейсе.

Пользователь получает:

> Не удалось загрузить заявки.
>
> Попробуйте ещё раз.
>
> [ Повторить ]

Разработчику логируется stack/error id.

---

# 53. СЕТЕВЫЕ ЗАПРОСЫ

API/client calls должны иметь:

- timeout;
- abort controller;
- retry policy только для безопасных операций;
- exponential backoff для network retry;
- error mapping.

Не retry автоматически mutation без idempotency.

---

# 54. ДИЗАЙН-СИСТЕМА

Основной стиль:

# BLACK LIQUID GLASS

Но стекло используется умеренно.

### База

Очень тёмный background.

### Surface

Dark graphite surfaces.

### Glass

Полупрозрачные слои только для:

- sidebar;
- floating panels;
- modal;
- command center;
- selected cards;
- office overlay.

### Borders

Тонкие светлые границы с низкой opacity.

### Typography

Современная grotesk/sans-serif.

Большие заголовки.

Плотная информационная иерархия.

---

# 55. VISUAL HIERARCHY

Основной экран должен иметь 3 уровня:

### Level 1

Главное действие / информация.

### Level 2

Основные рабочие блоки.

### Level 3

Secondary details.

Не превращать всё в cards.

Использовать whitespace и grid.

---

# 56. АНИМАЦИИ

Анимации должны быть:

- короткими;
- полезными;
- interruptible;
- low-motion friendly.

Примеры:

- sidebar transitions;
- modal open;
- task status change;
- realtime arrival;
- office character state;
- notification.

Запрещены:

- бесконечные тяжёлые glow loops;
- сильный blur на больших слоях;
- десятки анимированных DOM элементов одновременно;
- постоянная camera motion.

---

# 57. REDUCED MOTION

Поддерживать `prefers-reduced-motion`.

При включении:

- минимизировать transitions;
- убрать camera movement;
- сократить character animations;
- отказаться от декоративных loop-анимаций.

---

# 58. MOBILE

Mobile first для Creator/Mini App.

Desktop first допускается только для admin layout, но admin также обязан быть usable на планшете.

Для телефонов:

- bottom navigation;
- sticky action bar;
- большие touch targets;
- no hover-only interactions;
- no tiny icons without labels;
- 3D office simplified mode.

---

# 59. ADMIN DESKTOP LAYOUT

Рекомендуемая структура:

```text
┌────────────────────────────────────────────────────────────┐
│ Topbar                                                     │
├──────────────┬─────────────────────────────────────────────┤
│ Sidebar      │ Main Workspace                              │
│              │                                             │
│              │                                             │
│              │                                             │
└──────────────┴─────────────────────────────────────────────┘
```

Sidebar:

```text
BEZNIGATIVA

Главная
Заявки
Создатели
Команда
Проекты
Задачи
Календарь
Возможности
Материалы
Документы
Сообщения
AI
Аналитика
Настройки
```

---

# 60. CREATOR NAVIGATION

```text
Главная
Мой офис
Задачи
Проекты
Календарь
Возможности
Академия
Документы
Сообщения
AI
Профиль
```

---

# 61. ПОИСК

Глобальный поиск должен искать:

- пользователей;
- заявки;
- проекты;
- задачи;
- документы;
- сообщения;
- материалы.

Результаты группировать по типу.

---

# 62. АУДИТ

`audit_logs` обязателен.

Логировать минимум:

- изменение ролей;
- одобрение/отклонение заявки;
- изменение статуса creator;
- удаление;
- создание/изменение задач;
- документы;
- административные настройки;
- отправку критических сообщений;
- AI actions с side effects.

Формат:

```text
id
actor_id
action
entity_type
entity_id
before
 after
ip_hash_or_safe_meta
user_agent_meta
created_at
correlation_id
```

Не хранить лишние персональные данные в логах.

---

# 63. БЕЗОПАСНОСТЬ

Обязательные правила:

- server-side authorization;
- RLS для данных, если используется Supabase database access;
- private realtime channels;
- signed file URLs;
- encrypted secrets;
- secure cookies;
- CSRF-safe architecture;
- rate limiting;
- webhook verification;
- input validation;
- output encoding;
- file type checks;
- upload size limits;
- antivirus/content scanning для загружаемых файлов при масштабировании.

---

# 64. ВАЛИДАЦИЯ

Использовать schema validation, например Zod, на границах системы.

Проверять:

```text
Frontend input
Server Action input
API input
Webhook payloads
AI tool arguments
Database writes
```

Не доверять типам TypeScript как runtime validation.

---

# 65. AI SECURITY

AI нельзя разрешать:

- выполнять произвольный SQL;
- читать всю базу;
- отправлять любое Telegram сообщение без policy;
- удалять пользователей без confirmation;
- обходить RBAC;
- раскрывать hidden/internal notes creator'у.

AI должен видеть только разрешённый context.

---

# 66. TELEGRAM WEBHOOK

Webhook endpoint должен:

- проверять секрет webhook;
- валидировать payload;
- обрабатывать повторные обновления idempotently;
- быстро подтверждать получение;
- тяжёлую работу переносить в background job, если она действительно нужна.

---

# 67. BACKGROUND JOBS

Для тяжёлых задач предусмотреть job architecture:

- AI analysis;
- video metadata extraction;
- document processing;
- notification fan-out;
- social analytics sync;
- thumbnail generation.

Не выполнять длительную операцию внутри пользовательского HTTP request, если она может превысить разумный timeout.

---

# 68. STORAGE / MEDIA PIPELINE

Pipeline:

```text
Upload
 ↓
Validation
 ↓
Storage
 ↓
Metadata
 ↓
Optional processing job
 ↓
Ready
 ↓
Signed access
```

Frontend должен показывать upload progress.

---

# 69. DESIGN OF APPLICATION INBOX

Очень важный экран.

Не делать огромную таблицу.

Использовать split layout:

```text
┌───────────────┬───────────────────────────────────────────┐
│ Фильтры       │ Заявка                                   │
│               │                                           │
│ Новые         │ Имя                                       │
│ В работе      │ Статистика                                │
│ Нужен ответ   │ Соцсети                                   │
│ Одобрены      │                                           │
│ Архив         │ Сообщения                                 │
│               │                                           │
│ Candidate 1   │ Internal notes                            │
│ Candidate 2   │                                           │
│ Candidate 3   │ [Взять] [Ответить] [Одобрить]            │
└───────────────┴───────────────────────────────────────────┘
```

---

# 70. ВНУТРЕННИЕ СОСТОЯНИЯ UI

Каждый сложный компонент обязан предусматривать:

```text
loading
empty
error
success
partial
stale
offline
permission denied
```

Например, чат:

- загрузка истории;
- нет сообщений;
- сообщение отправляется;
- сообщение отправлено;
- сообщение не отправилось;
- reconnect;
- offline.

---

# 71. ДАННЫЕ И КЭШ

Разделить:

### Server truth

PostgreSQL / server domain layer.

### Client state

UI state, opened modals, selected filters.

### Realtime state

Live events/presence.

Не делать database state единственным источником для каждого пиксельного frame.

---

# 72. CACHE STRATEGY

Статические материалы и относительно стабильные данные — кэшировать.

Персональные dashboards — использовать dynamic rendering / appropriate cache boundaries.

Realtime updates должны делать точечную инвалидацию или обновление состояния, а не полный reload страницы.

Next.js docs отдельно подчёркивают, что часто обновляемые персонализированные dashboards не следует бездумно обслуживать как статический контент.

---

# 73. DATABASE ACCESS PATTERN

Не писать SQL из UI.

Использовать domain/repository/service слой:

```text
UI
 ↓
Server Action / API
 ↓
Permission Check
 ↓
Domain Service
 ↓
Repository
 ↓
PostgreSQL
```

---

# 74. FOLDER STRUCTURE

Рекомендуемая структура:

```text
src/
  app/
    (public)/
    (auth)/
    (creator)/
    (admin)/
    api/

  components/
    ui/
    layout/
    office/
    tasks/
    applications/
    chat/
    notifications/

  features/
    auth/
    applications/
    creators/
    projects/
    tasks/
    messages/
    academy/
    documents/
    ai/
    notifications/
    presence/

  lib/
    auth/
    telegram/
    realtime/
    storage/
    validation/

  server/
    services/
    repositories/
    permissions/
    events/

  ai/
    agents/
    tools/
    prompts/
    policies/

  db/
    schema/
    migrations/

  types/
```

---

# 75. СОСТОЯНИЯ АВТОРИЗАЦИИ

```text
LOADING
AUTHENTICATED
UNAUTHENTICATED
FORBIDDEN
SESSION_EXPIRED
TELEGRAM_VALIDATION_FAILED
```

Не редиректить человека в неожиданные места.

---

# 76. СТРАНИЦЫ

Минимальный sitemap:

```text
/
/about
/academy
/apply
/login

/app
/app/dashboard
/app/office
/app/tasks
/app/projects
/app/calendar
/app/opportunities
/app/academy
/app/documents
/app/messages
/app/ai
/app/profile

/admin
/admin/dashboard
/admin/applications
/admin/creators
/admin/team
/admin/projects
/admin/tasks
/admin/calendar
/admin/opportunities
/admin/documents
/admin/messages
/admin/ai
/admin/analytics
/admin/settings
/admin/logs
```

---

# 77. ЧТО ДОЛЖЕН ВИДЕТЬ НОВЫЙ ПОЛЬЗОВАТЕЛЬ

Он должен за 10–20 секунд понять:

1. Что такое BEZNIGATIVA.
2. Для кого это.
3. Что здесь можно получить.
4. Что нужно сделать, чтобы попасть в команду.
5. Где находится заявка.

---

# 78. ЧТО ДОЛЖЕН ВИДЕТЬ УЧАСТНИК

Он должен моментально понимать:

1. Я сейчас на работе или нет.
2. Что мне делать.
3. До какого времени.
4. В каком проекте я участвую.
5. Есть ли у меня новые сообщения.
6. Есть ли замечания.
7. Есть ли новые возможности.

---

# 79. ЧТО ДОЛЖЕН ВИДЕТЬ АДМИН

Он должен моментально понимать:

1. Сколько людей сейчас активно.
2. Сколько новых заявок.
3. Какие заявки требуют внимания.
4. Какие задачи просрочены.
5. Какие проекты требуют вмешательства.
6. Кто сейчас работает.
7. Какие события произошли.

---

# 80. DESIGN ANTI-PATTERNS

Запрещено:

- gradients everywhere;
- glass everywhere;
- huge shadows;
- excessive blur;
- tiny text;
- 3D everywhere;
- animation for every click;
- excessive rounded cards;
- generic SaaS template;
- purple AI clichés;
- neon cyberpunk without business rationale;
- mobile UI squeezed into desktop;
- desktop UI squeezed into mobile.

---

# 81. PIXEL OFFICE ART DIRECTION

Визуальный mood:

**маленькая современная продакшн-студия внутри цифрового мира.**

Не средневековый RPG.

Не киберпанк.

Не Minecraft.

Не корпоративный офис.

Нужно ощущение:

> «Я вошёл внутрь живой команды BEZNIGATIVA».

Объекты:

- чёрные рабочие столы;
- мониторы;
- камеры;
- микрофоны;
- студийный свет;
- диван;
- кофейная зона;
- музыкальные инструменты;
- editing station;
- whiteboard;
- neon/light accents очень умеренно.

---

# 82. ОФИСНЫЕ ЗОНЫ

Пример:

```text
LOBBY
STUDIO
EDITOR DESK
MUSIC ROOM
MEETING ROOM
LOUNGE
CONTENT LAB
ADMIN ROOM
```

Зоны можно сделать кликабельными.

Например, клик на MUSIC ROOM → открывает Music Projects.

---

# 83. ОФИС КАК NAVIGATION LAYER

Офис — дополнительная навигация.

Пример:

Клик на:

🎬 Studio

открывает:

**Проекты → Продакшн → Съёмки**

Клик на:

💻 Desk

открывает:

**Мои задачи**

Клик на:

📋 Board

открывает:

**Календарь / задачи**

Но обычная sidebar navigation всегда остаётся доступной.

---

# 84. ANALYTICS

Основные metrics:

### Recruitment

- applications;
- approval rate;
- time to review;
- conversion.

### Creators

- active creators;
- retained creators;
- activity.

### Work

- tasks completed;
- overdue rate;
- average review time.

### Production

- project completion;
- content output;
- campaign performance.

---

# 85. ОБРАБОТКА АНКЕТ AI + HUMAN

Правило:

```text
AI assists
Human decides
```

AI может:

- summarize;
- score;
- detect missing info;
- suggest questions;
- recommend next step.

Human:

- принимает финальное решение.

---

# 86. ПЕРСОНАЛЬНЫЕ ДАННЫЕ

Сохранять только необходимые данные.

Не спрашивать данные просто «потому что можно».

У каждого чувствительного поля должна быть причина хранения.

---

# 87. NOTIFICATION CENTER

В web:

иконка 🔔.

Категории:

```text
Все
Задачи
Проекты
Сообщения
Заявки
Документы
Система
```

Unread counter должен приходить realtime.

---

# 88. WEB PUSH

Опциональный этап.

Не обязателен для MVP.

Telegram остаётся главным внешним notification channel.

---

# 89. OBSERVABILITY

Обязательно подключить:

- structured logging;
- error tracking;
- performance monitoring;
- DB monitoring;
- realtime connection metrics;
- background job status.

Логи не должны содержать секреты.

---

# 90. ТЕСТИРОВАНИЕ

Обязательные уровни:

### Unit

- domain logic;
- validators;
- permission checks.

### Integration

- application lifecycle;
- Telegram event handling;
- notification flow;
- database operations.

### E2E

Минимальный сценарий:

```text
Telegram user starts
→ profile
→ submits application
→ admin receives
→ admin changes status
→ user receives Telegram notification
→ user approves onboarding
→ creator workspace opens
```

---

# 91. E2E REALTIME ТЕСТ

Проверить:

```text
Browser A changes task
↓
Browser B receives update
↓
Telegram notification sent if policy says yes
↓
Activity log created
```

---

# 92. ACCEPTANCE TEST: WORKDAY

```text
User logs in
→ office loaded
→ clicks «Я на рабочем месте»
→ work_session created
→ presence updated
→ office character changes
→ admin sees user online
→ second device receives update
→ click «Завершить рабочий день»
→ session closes
→ all clients update
```

---

# 93. ACCEPTANCE TEST: APPLICATION

```text
User starts from Telegram
→ opens Mini App
→ fills application
→ submits
→ DB record created
→ application appears in admin inbox
→ admin opens chat
→ status IN_REVIEW
→ user receives Telegram notification
→ admin approves
→ creator role/access granted
→ user receives onboarding message
→ Team Workspace available
```

---

# 94. ACCEPTANCE TEST: TASK

```text
Admin creates task
→ assigns creator
→ creator receives realtime update
→ Telegram message sent
→ creator submits file
→ task = WAITING_REVIEW
→ manager reviews
→ revision
→ creator gets Telegram + in-app
→ creator resubmits
→ manager approves
→ task DONE
```

---

# 95. MVP — ЧТО РЕАЛИЗОВАТЬ ПЕРВЫМ

### Phase 1

- Telegram Bot;
- Telegram auth;
- Mini App;
- web auth;
- PostgreSQL;
- users/profiles;
- applications;
- admin inbox;
- Telegram notifications;
- creator dashboard;
- tasks;
- realtime;
- basic office scene.

### Phase 2

- projects;
- calendar;
- chat;
- documents;
- opportunities;
- Academy.

### Phase 3

- AI agents;
- advanced analytics;
- advanced office interactions;
- production tooling;
- campaign management.

---

# 96. ПЕРВЫЙ VERTICAL SLICE

Агент не должен сначала месяц делать весь UI.

Сначала реализовать один полный вертикальный поток:

```text
Telegram
→ auth
→ application
→ admin inbox
→ status change
→ Telegram notification
→ approval
→ creator workspace
→ work session
→ realtime
```

После успешной демонстрации этого vertical slice — масштабировать остальные модули.

---

# 97. ПРАВИЛО НЕ ПЕРЕСТРАИВАТЬ ВСЁ

После создания базы агент не должен каждый раз менять архитектуру целиком.

Любое существенное архитектурное изменение:

1. создать ADR;
2. описать проблему;
3. описать текущую архитектуру;
4. описать новую;
5. указать миграционный план;
6. только после этого менять код.

---

# 98. ENVIRONMENT VARIABLES

Пример:

```text
DATABASE_URL
DIRECT_DATABASE_URL
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY
SUPABASE_SECRET_KEY
TELEGRAM_BOT_TOKEN
TELEGRAM_WEBHOOK_SECRET
TELEGRAM_MINI_APP_URL
AI_API_KEY
AI_MODEL
STORAGE_BUCKET
APP_URL
```

Не коммитить реальные значения.

Актуальная документация Supabase указывает переход к publishable/secret API keys и ожидаемую постепенную замену старых naming schemes, поэтому при реализации использовать актуальные ключевые форматы проекта и следить за документацией провайдера.

---

# 99. DEPLOYMENT

Рекомендуемая схема:

```text
GitHub
  ↓
CI
  ↓
Lint
Typecheck
Tests
Build
  ↓
Deploy
```

Использовать preview deployments для pull requests.

Production и development среды разделить.

---

# 100. DATABASE MIGRATIONS

Все изменения schema только через migrations.

Запрещено вручную менять production schema без migration history.

---

# 101. SEED DATA

Для development разрешены seed users:

```text
Admin Demo
Manager Demo
Blogger Demo
Musician Demo
Producer Demo
```

В production seed/demo data удаляется.

---

# 102. DESIGN DELIVERY PROCESS

После технического foundation перейти к дизайну в таком порядке:

1. Design tokens.
2. Shell / sidebar.
3. Admin Dashboard.
4. Application Inbox.
5. Creator Dashboard.
6. Task screen.
7. Project screen.
8. Office scene.
9. Mobile Mini App.
10. Notifications / modals / empty states.

Не рисовать все страницы одновременно.

---

# 103. ДИЗАЙН-ПРИНЦИП

Система должна выглядеть дорого не за счёт количества эффектов, а за счёт:

- композиции;
- типографики;
- глубины;
- материала surfaces;
- качественных иллюстраций;
- маленьких деталей;
- последовательной motion system;
- хорошо продуманного 3D/pixel office.

---

# 104. ЗАПРЕТ НА GENERIC AI UI

Не использовать типовой интерфейс:

```text
dark background
purple gradient
AI sparkles
3 cards
huge rounded corners
```

как основной дизайн.

AI — часть системы, но не тема всего интерфейса.

---

# 105. ПРИНЦИП «ПОНЯТНО С ПЕРВОГО ВЗГЛЯДА»

Любой человек должен понимать:

**Где я?**

**Что происходит?**

**Что от меня требуется?**

**Что будет дальше?**

Это приоритетнее декоративности.

---

# 106. ОСОБЕННО ВАЖНО ДЛЯ GLM-АГЕНТА

Перед началом любой реализации агент обязан прочитать:

- этот файл;
- README проекта;
- `ARCHITECTURE.md`, если существует;
- `DESIGN_SYSTEM.md`, если существует;
- `TASKS.md`, если существует;
- `.env.example`.

Перед изменением существующего модуля агент обязан найти его текущую реализацию.

Запрещено:

- создавать дубликаты компонентов;
- создавать две системы auth;
- создавать вторую database client abstraction;
- создавать новый UI kit поверх существующего;
- менять naming conventions без причины;
- удалять работающую функциональность ради упрощения.

---

# 107. ОБЯЗАТЕЛЬНЫЕ CHECKS ПЕРЕД COMMIT

```text
npm run lint
npm run typecheck
npm run test
npm run build
```

При наличии E2E:

```text
npm run test:e2e
```

Нельзя считать задачу завершённой, если build падает.

---

# 108. ОБЯЗАТЕЛЬНЫЙ DEVELOPMENT WORKFLOW

Перед реализацией функции:

```text
1. Understand requirement
2. Find existing module
3. Check domain model
4. Check permission model
5. Check realtime events
6. Implement server truth
7. Implement UI
8. Implement loading/error/empty states
9. Add tests
10. Run validation
```

---

# 109. DEFINITION OF DONE

Функция считается завершённой только если:

- работает серверная логика;
- работает UI;
- есть validation;
- есть permission check;
- есть loading;
- есть error;
- есть empty state;
- есть realtime, если функция этого требует;
- есть Telegram notification, если функция этого требует;
- есть audit event, если действие административное;
- есть тесты на критический flow;
- проект собирается.

---

# 110. ФИНАЛЬНАЯ ПРОДУКТОВАЯ МОДЕЛЬ

BEZNIGATIVA должен восприниматься пользователем не как:

> «ещё один сайт для задач».

А как:

> **цифровой офис команды, в который человек реально входит работать.**

Ключевой эмоциональный слой:

```text
Telegram
   ↓
Я вошёл
   ↓
Я вижу свой офис
   ↓
Я на рабочем месте
   ↓
У меня есть задача
   ↓
Команда рядом
   ↓
Я могу спросить AI
   ↓
Я отправляю результат
   ↓
Получаю review
   ↓
Получаю следующий шаг
```

---

# 111. ОСНОВНАЯ ФОРМУЛА ПРОДУКТА

```text
BEZNIGATIVA
=
Recruitment
+
Creator Development
+
Production Management
+
Team Workspace
+
Telegram Integration
+
Realtime
+
AI
+
Interactive Office
```

---

# 112. КЛЮЧЕВЫЕ ТЕХНИЧЕСКИЕ РЕШЕНИЯ

Зафиксировать как baseline:

- Next.js 16.3+ / App Router;
- TypeScript strict;
- PostgreSQL;
- Supabase Realtime / storage where appropriate;
- Telegram Bot API;
- Telegram Mini App;
- server-side Telegram validation;
- RBAC + permission layer;
- domain events;
- notification engine;
- React Three Fiber + Three.js или PixiJS после prototype benchmark;
- Motion;
- Zod;
- testing + CI;
- structured logging + error monitoring;
- progressive enhancement для 3D.

---

# 113. ИСТОЧНИКИ И ТЕХНИЧЕСКАЯ ПРОВЕРКА

Основные официальные источники, которые агент обязан сверять с актуальной версией документации перед реализацией интеграций:

- Next.js Docs: https://nextjs.org/docs
- Next.js Blog / Releases: https://nextjs.org/blog
- Next.js Streaming: https://nextjs.org/learn/dashboard-app/streaming
- Next.js Navigation: https://nextjs.org/learn/dashboard-app/navigating-between-pages
- Telegram Bot API: https://core.telegram.org/bots/api
- Telegram Mini Apps: https://core.telegram.org/bots/webapps
- Supabase Realtime: https://supabase.com/docs/guides/realtime
- Supabase Realtime Broadcast / Database Changes: https://supabase.com/docs/guides/realtime/subscribing-to-database-changes
- Supabase Presence: https://supabase.com/docs/guides/realtime/presence
- Three.js Docs: https://threejs.org/docs/
- PixiJS Renderers: https://pixijs.com/8.x/guides/components/renderers

---

# 114. КРАТКИЙ PROMPT ДЛЯ АГЕНТА

> Ты реализуешь BEZNIGATIVA — полноценную Creator / Artist / Production платформу.
>
> Это не простой Telegram-бот и не обычная CRM. Telegram является центральным каналом идентичности и коммуникации, Web App — основной рабочей средой, Mini App — мобильной точкой входа, а интерактивный pixel/3D office — визуальным слоем рабочего пространства.
>
> Все интерфейсы на русском языке.
>
> Следуй этому ТЗ буквально. Сначала изучи существующий проект, архитектуру, package.json, database schema, env example и существующие компоненты. Не создавай дубликаты архитектурных слоёв.
>
> Реализуй сначала вертикальный рабочий flow: Telegram auth → application → Admin Inbox → status change → Telegram notification → approval → Creator Workspace → work session → realtime.
>
> Главные требования: server truth, RBAC, realtime, idempotency, Telegram synchronization, loading/error/empty states, mobile support, performance, progressive enhancement 3D.
>
> Не доверяй client-side Telegram data без серверной валидации. Не давай AI неограниченный доступ. Не делай mock data вместо рабочего backend. Не перегружай интерфейс glass-эффектами. Не делай 3D блокирующим.
>
> Если изменение архитектуры неизбежно — сначала зафиксируй ADR и только затем меняй архитектуру.

---

## КОНЕЦ ТЗ
