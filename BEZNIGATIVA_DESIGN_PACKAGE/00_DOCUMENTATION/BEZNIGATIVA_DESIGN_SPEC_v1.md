# BEZNIGATIVA — DESIGN SPECIFICATION + VISUAL STORYBOARD v1.0

> Назначение документа: это обязательная визуальная и UX-спецификация для реализации web-платформы BEZNIGATIVA, Telegram Mini App и кабинета участников. Документ рассчитан на AI-агента-разработчика. Агент НЕ должен самовольно менять структуру, названия разделов, визуальный язык, порядок блоков или поведение экранов без явной причины.

## 0. Главная задача

BEZNIGATIVA — русскоязычная creator/production-платформа для блогеров, музыкантов и другой креативной команды.

Продукт состоит из:

- Telegram Bot — вход, заявки, уведомления, быстрые действия.
- Telegram Mini App — мобильный кабинет внутри Telegram.
- Web App — полноценная рабочая среда в браузере.
- Admin Panel — управление кандидатами, участниками, задачами, проектами, коммуникациями, знаниями, аналитикой и финансами.
- AI Layer — ассистент и автоматизация, но с ограниченными инструментами и серверной авторизацией.
- Pixel Office — визуальный слой рабочего пространства с живыми пиксельными персонажами.

Ключевой принцип: интерфейс должен выглядеть как современная digital-production platform, а не как старая CRM.

---

# 1. Дизайн-направление

## 1.1. Визуальный образ

Стиль: `Dark Liquid Glass + Pixel Office + Neon Production OS`.

Основные ощущения:

- дорого;
- технологично;
- живо;
- немного игровое;
- понятно с первого взгляда;
- без перегруза декоративными эффектами.

Пиксельный офис — это не фон ради фона. Он должен отражать состояние команды и быть функциональной частью продукта.

## 1.2. Запрещено

- Светлая корпоративная CRM-эстетика.
- Bootstrap-подобный визуальный шаблон.
- Одинаковые glass-card на каждом блоке.
- Многоцветная радуга без функциональной семантики.
- Неон, который снижает читаемость.
- Английские названия разделов в пользовательском интерфейсе.
- Случайная смена размеров, отступов, шрифтов или радиусов.
- Анимации длиннее, чем необходимо для понимания изменения состояния.
- Сложные 3D-сцены, которые ухудшают performance.

---

# 2. Канонический canvas и сетка

## Desktop

Базовый макет: `1440 × 1024 px`.

- Sidebar: `248 px`.
- Main content: остаток ширины.
- Header/topbar: `72 px`.
- Page horizontal padding: `24 px`.
- Page vertical padding: `20–24 px`.
- Grid gap: `16 px`.
- Большие блоки: radius `20 px`.
- Карточки: radius `16–18 px`.
- Inputs/buttons: radius `10–12 px`.

## Tablet

Базовый макет: `768 × 1024 px`.

- Sidebar превращается в compact rail `72 px` либо overlay drawer.
- Main padding: `16 px`.
- Grid gap: `12 px`.

## Mobile Web / Telegram Mini App

Базовый макет: `375 × 812 px`.

- Horizontal padding: `16 px`.
- Safe area учитывается обязательно.
- Нижняя навигация: `68–76 px`.
- Контент не должен находиться под home indicator.
- Touch target для интерактивного элемента: минимум `44 × 44 px`.

---

# 3. Цветовые токены

Базовые значения можно менять только централизованно через design tokens.

```text
BG-0       #06070A
BG-1       #0A0C10
BG-2       #10141A
SURFACE    #121821
SURFACE-2  #151B24
BORDER     rgba(255,255,255,.08)
TEXT-1     #F5F7FA
TEXT-2     #B8C0CC
TEXT-3     #778191
PRIMARY    #8B5CFF
SECONDARY  #0BC6D4
SUCCESS    #22C55E
WARNING    #F59E0B
ERROR      #EF4444
INFO       #3B82F6
```

Цвет статуса персонала:

- зелёный — в сети;
- жёлтый — на задаче;
- фиолетовый — на созвоне;
- красный — отошёл;
- серый — не в сети.

Цвет статуса не должен быть единственным способом передачи смысла: рядом должен находиться текст/иконка.

---

# 4. Типографика

Основные шрифты: `Inter` для интерфейса, `Manrope` для крупных display-заголовков.

| Style | Size | Line Height | Weight |
|---|---:|---:|---:|
| H1 | 32 px | 40 px | 700 |
| H2 | 24 px | 32 px | 700 |
| H3 | 18 px | 24 px | 650 |
| Body | 14 px | 20 px | 400 |
| Body Large | 16 px | 24 px | 400 |
| Caption | 12 px | 16 px | 500 |
| Tiny | 11 px | 14 px | 500 |
| Numeric KPI | 28–36 px | 1.0 | 700 |

Русский UI должен быть типографически первичным. Не использовать английские fallback-строки в реальном продукте.

---

# 5. Навигация

Desktop sidebar:

1. Логотип BEZNIGATIVA.
2. Главная.
3. Заявки.
4. Команда.
5. Задачи.
6. Проекты.
7. Календарь.
8. Чат.
9. База знаний.
10. Возможности.
11. Аналитика.
12. Финансы.
13. Настройки.
14. Профиль.

Нижняя часть sidebar:

- текущий пользователь;
- статус;
- быстрый переход в Telegram;
- переключатель темы (если тема предусмотрена, но по умолчанию Dark).

---

# 6. Общие UI-компоненты

## 6.1. Header

Высота `72 px`.

Слева:

- breadcrumb;
- название страницы;
- при необходимости description.

Справа:

- глобальный поиск;
- уведомления;
- помощь;
- профиль.

## 6.2. Global Command Center

Открывается по `Ctrl/Cmd + K`.

Ширина модального окна: `640–760 px`.

Содержит:

- поиск по разделам;
- поиск людей;
- поиск задач;
- быстрые действия;
- AI-команды.

Примеры:

`Найди блогеров с аудиторией больше 50 000`
`Покажи просроченные задачи`
`Создай задачу для проекта SHOW`

## 6.3. Cards

Карточка не должна иметь сильную тень по умолчанию.

Использовать:

- тонкую border;
- мягкий backdrop blur только там, где это необходимо;
- внутренние группы через surface-2;
- hover через яркость/translation не более `2 px`.

---

# 7. PAGE 01 — Авторизация / вход

### Desktop

Левая часть — визуальный hero:

- пиксельный офис;
- 3–5 персонажей;
- короткая фраза;
- логотип.

Правая часть — auth panel шириной `420 px`.

Структура:

1. Logo.
2. Заголовок: `Добро пожаловать в BEZNIGATIVA`.
3. Подзаголовок.
4. Основная кнопка `Войти через Telegram`.
5. Вторичный способ входа только для админов/служебного режима.
6. Текст о безопасности.

### Mobile

Сначала logo, затем компактная pixel-сцена, затем кнопка Telegram.

### States

- loading;
- Telegram redirecting;
- success;
- expired session;
- error.

---

# 8. PAGE 02 — Главная / Dashboard / Pixel Office

Это главный экран продукта.

## Layout desktop

Верх:

- приветствие;
- дата;
- global actions.

Главный блок: `≈ 65%` ширины.

Внутри — живой изометрический офис.

Справа — KPI stack `≈ 35%`.

### Pixel Office

Камера: фиксированная изометрия.

Персонажи:

- 12–20 visible entities максимум одновременно;
- idle animation;
- walking animation;
- work animation;
- status bubble;
- click/hover interaction.

Интерактив:

клик по сотруднику → мини-карточка:

`Алексей / Монтажёр / На задаче / 78% задач выполнено`

Нижняя часть:

- мои задачи;
- новые заявки;
- команда онлайн;
- ближайшие события.

### Mobile

Pixel Office должен сохранять смысл, но быть укороченным до viewport-safe scene. Под сценой — вертикальные cards.

---

# 9. PAGE 03 — Система заявок

## Назначение

Основной рабочий inbox для обработки входящих кандидатов.

### Верх

- `Заявки`.
- поиск;
- фильтры;
- сортировка;
- массовые действия.

### Tabs

- Все.
- Новые.
- В работе.
- На проверке.
- Принятые.
- Отклонённые.

### Main list

Каждая заявка:

- avatar 40–48 px;
- имя;
- тип: блогер / музыкант / creator;
- главная площадка;
- аудитория;
- AI-score;
- время поступления;
- статус;
- CTA `Открыть`.

### Right drawer / detail

Ширина `460–520 px`.

Секции:

- профиль;
- ссылки;
- ответы анкеты;
- файлы;
- AI-анализ;
- история;
- internal notes;
- действия.

Главные CTA:

`Принять`
`Отклонить`
`На интервью`
`Запросить дополнительную информацию`

---

# 10. PAGE 04 — Карточка кандидата / участника

Header:

- avatar `72–96 px`;
- имя;
- username;
- роль;
- текущий статус;
- кнопка Telegram.

Tabs:

- Обзор.
- Задачи.
- Проекты.
- Контент.
- Документы.
- Активность.

KPI:

- аудитория;
- engagement;
- выполнено задач;
- активных проектов.

Ниже — timeline активности.

Справа — AI profile summary.

---

# 11. PAGE 05 — Кабинет участника

Участник должен сразу видеть `что делать сейчас`.

Hero:

`Привет, Макс`.

Большая карточка:

`Текущий статус — На рабочем месте`.

CTA:

`Я на месте` / `Завершить рабочий день`.

Ниже:

- мои задачи;
- ближайшие дедлайны;
- активные проекты;
- новые возможности;
- уведомления.

### Правило

Главный экран участника не должен повторять всю админку. Информация приоритизируется по личной работе.

---

# 12. PAGE 06 — Задачи

Desktop layout:

- title + create button;
- filter bar;
- board/list toggle.

Kanban columns:

1. К выполнению.
2. В работе.
3. На проверке.
4. Выполнено.

Task card:

- title;
- project;
- assignee;
- priority;
- due date;
- progress;
- attachments count.

Drag-and-drop должен иметь визуальный preview.

При клике — task drawer или отдельная page.

---

# 13. PAGE 07 — Команда / сотрудники

View modes:

- grid;
- list;
- pixel office.

Каждый сотрудник:

- avatar/pixel-avatar;
- имя;
- роль;
- department;
- online status;
- current task;
- last activity.

Filters:

- роль;
- статус;
- проект;
- отдел.

Отдельный switch:

`Показать офис`.

---

# 14. PAGE 08 — Чат / коммуникации

Двухколоночная desktop-структура:

Левая панель `320–360 px`:

- поиск;
- каналы;
- direct messages;
- unread counters.

Центр:

- chat header;
- сообщения;
- attachments;
- typing indicator;
- composer.

Правая панель опциональна:

- участники;
- pinned messages;
- связанные задачи;
- файлы.

Chat должен быть realtime.

---

# 15. PAGE 09 — Календарь

Views:

- День.
- Неделя.
- Месяц.

Desktop — time-grid.

События:

- съёмка;
- созвон;
- дедлайн;
- интервью;
- публикация;
- мероприятие.

Цвет используется по типу события, но текст остаётся читаемым.

Клик по событию открывает drawer.

---

# 16. PAGE 10 — База знаний

Main categories:

- Продвижение.
- Контент.
- Музыка.
- Монетизация.
- Контракты.
- Инструменты.
- FAQ.

Главный экран:

- search;
- featured cards;
- recent articles;
- progress.

Article page:

- title;
- author;
- last update;
- content;
- related articles;
- AI actions.

AI actions:

`Объяснить проще`
`Сделать чеклист`
`Сделать план действий`

---

# 17. PAGE 11 — Аналитика

Header:

- period selector;
- compare toggle;
- export.

Top KPIs:

- активные участники;
- прирост аудитории;
- выполнено задач;
- активность.

Charts:

- line chart;
- bar chart;
- donut/platform split;
- top performers.

Нельзя превращать страницу в стену графиков. На первом экране максимум 4 крупных KPI и 3–4 meaningful charts.

---

# 18. PAGE 12 — Финансы

Для внутреннего менеджмента.

Top cards:

- общий баланс;
- доходы;
- расходы;
- выплаты.

Ниже:

- transaction list;
- campaigns;
- creator payouts;
- documents.

Финансовые данные должны быть permission-gated.

---

# 19. PAGE 13 — Настройки

Left sub-navigation:

- Профиль.
- Безопасность.
- Уведомления.
- Telegram.
- Интеграции.
- Команда и роли.
- Роли и доступ.
- Внешний вид.
- AI.

Forms:

- labels всегда сверху;
- description под полем;
- save action fixed/visible;
- destructive actions отдельно.

---

# 20. PAGE 14 — Mobile Web App / Telegram Mini App

Bottom navigation:

1. Главная.
2. Задачи.
3. Заявки/Проекты — в зависимости от роли.
4. Чат.
5. Профиль.

Top bar:

- avatar;
- page title;
- notification icon.

Mobile cards:

- single column;
- `16 px` padding;
- no tiny text;
- no hover-only controls.

Swipe gestures разрешены только там, где действие очевидно и не разрушает навигацию.

---

# 21. PAGE 15 — Telegram Bot

Bot UI — это НЕ копия web-интерфейса.

Основные сценарии:

### Новый кандидат

`/start` → приветствие → анкета → подтверждение → отправка заявки.

### Кандидат после отправки

`Ваша заявка принята в обработку.`

### Решение

`Ваша заявка одобрена.`

Кнопка:

`Открыть кабинет`.

### Участник

Уведомления:

- новая задача;
- изменение статуса;
- комментарий менеджера;
- задача принята;
- запрос доработки;
- новый проект;
- новая возможность.

---

# 22. PAGE 16 — Pixel Office / Locations

Набор сцен:

- Ресепшен.
- Рабочая зона.
- Переговорная.
- Студия.
- Музыкальная комната.
- Зона отдыха.
- Кухня.
- Серверная.

## Камера

Фиксированная изометрическая камера.

## Слой объекта

```text
BACKGROUND
FLOOR
WALLS
PROPS
CHARACTERS
EFFECTS
UI OVERLAY
```

## Character state machine

```text
OFFLINE
IDLE
WALKING
WORKING
PHONE
MEETING
AWAY
CELEBRATE
```

Статус работника должен определять визуальное состояние персонажа.

---

# 23. Pixel Office — motion specification

Idle:

- 2–4 frame subtle loop.

Walk:

- 6–8 frames.

Work:

- 4–6 frame loop.

Phone:

- 4–6 frames.

Celebrate:

- 6–8 frames.

Эффекты:

- typing sparks;
- notification ping;
- music notes;
- coffee steam;
- progress sparkle.

Анимации должны останавливать rendering, когда сцену не видно.

---

# 24. Realtime UX

Изменения данных должны ощущаться мгновенными.

Пример:

Admin изменил задачу → creator получает изменение без reload.

Admin ответил в заявке → Telegram получает уведомление.

Creator нажал `Я на месте` → admin видит зелёный статус и персонаж перемещается в офис.

Рекомендуемый поток:

```text
UI action
↓
Server mutation
↓
Database
↓
Realtime event
↓
Web clients + Mini App
↓
Notification service
↓
Telegram Bot
```

Не использовать polling как основной механизм.

---

# 25. Loading / Empty / Error states

Каждая страница обязана иметь:

- loading;
- skeleton;
- empty;
- error;
- success;
- disabled permission state.

Skeleton должен повторять реальные размеры компонентов.

Не использовать бесконечный spinner посередине страницы, если можно показать структурный skeleton.

---

# 26. Performance

Обязательные правила:

- code splitting;
- lazy loading тяжёлых компонентов;
- dynamic import для pixel office;
- image optimization;
- WebP/AVIF там, где PNG не нужен;
- sprite atlas для пиксельных ассетов;
- virtualized lists для больших списков;
- debounced search;
- optimistic UI только там, где rollback безопасен;
- не загружать весь pixel office до первого экрана, если пользователь может его не увидеть.

Цель: UI должен быть responsive даже при слабом ноутбуке.

---

# 27. Accessibility

Минимум:

- keyboard navigation;
- visible focus;
- aria-label для icon-only buttons;
- контраст текста;
- не полагаться только на цвет;
- reduced motion support.

---

# 28. Responsive rules

### 1440+

Полная sidebar + 2–4 колонки content.

### 1024–1439

Sidebar full или compact.

### 768–1023

Compact navigation + drawers.

### < 768

Mobile-first layout.

Все desktop drawers должны превращаться в bottom sheets/full-screen sheets.

---

# 29. Telegram ↔ Web identity

Один человек = один canonical user identity.

Обязательно хранить связь:

```text
telegram_user_id
internal_user_id
web_session_id
role
status
```

Web login через Telegram должен вести в тот же профиль, а не создавать дубликат пользователя.

---

# 30. Действие «Я на рабочем месте»

Ключевой продуктовый сценарий.

### Вход

User opens dashboard.

Кнопка:

`Я на рабочем месте`.

После клика:

1. сервер фиксирует timestamp;
2. статус становится `На рабочем месте`;
3. создаётся activity event;
4. realtime отправляет статус admin/client;
5. pixel character появляется/переходит в офис;
6. Telegram можно уведомлять только при необходимости.

### Завершение

`Завершить рабочий день`.

При выходе не удалять исторические данные.

---

# 31. Заявка → Telegram flow

### Candidate sends application

Bot → backend → application created → admin Inbox.

### Admin changes status

`NEW → IN_REVIEW`.

Система:

- записывает audit event;
- обновляет realtime;
- отправляет Telegram notification.

### Admin approves

`APPROVED`.

Система:

- добавляет creator role;
- открывает team workspace;
- создаёт onboarding checklist;
- отправляет Telegram message с CTA `Открыть кабинет`.

---

# 32. UI microcopy

Тексты короткие, понятные, человеческие.

Не:

`Выполнение операции было успешно произведено.`

Да:

`Готово.`

Не:

`Данный пользователь в настоящий момент находится в активном состоянии.`

Да:

`В сети.`

Тон: дружелюбный, уверенный, современный.

---

# 33. Design QA checklist для AI-агента

Перед завершением каждой страницы агент должен проверить:

- Совпадает ли layout с этой спецификацией?
- Используются ли те же spacing tokens?
- Весь пользовательский текст на русском?
- Есть ли loading/empty/error/success?
- Есть ли mobile variant?
- Нет ли непредусмотренных компонентов?
- Не перегружена ли страница glass-эффектами?
- Не нарушена ли иерархия CTA?
- Не появляется ли горизонтальный scroll?
- Не загружаются ли тяжёлые pixel assets до необходимости?
- Realtime-изменения корректно отображаются?
- Доступ запрещённый пользователь получает понятный UI, а не техническую ошибку?

---

# 34. Состав визуальных reference-файлов

В папке `01_STORYBOARD/` находятся:

- `01_all_pages_overview.png` — общий визуальный direction;
- `02_16_page_storyboard.png` — карта 16 основных экранов;
- `03_ui_ux_design_system_reference.png` — UI/UX + design-system reference.

В `02_REFERENCES/` находятся предыдущие pixel-office и asset pack references.

---

# 35. Правило реализации

Этот документ является source of truth для фронтенда до появления новой версии Design Specification.

Если реализация конфликтует с документом, агент должен:

1. Сохранить описанную структуру.
2. Не придумывать новый раздел.
3. Не менять смысл CTA.
4. Не заменять пиксельный office direction на обычные иллюстрации.
5. Не заменять Telegram-first flow другим authentication flow.
6. Не убирать realtime.
7. Не убирать mobile.
8. Не упрощать безопасность ради удобства.
9. Все отклонения фиксировать в changelog.

# 36. Критерий готовности

Продукт готов по UI только тогда, когда:

- все 16 страниц реализованы;
- desktop + mobile states существуют;
- роли ограничивают данные;
- Telegram и Web используют единого пользователя;
- заявки полностью проходят end-to-end flow;
- задачи проходят create → assign → submit → review → approve/revision;
- чат realtime;
- статусы команды realtime;
- pixel office отражает live statuses;
- loading/error/empty states предусмотрены;
- performance не ухудшается из-за pixel layer.

---

## 37. Короткая формула продукта

`Telegram → Application → Review → Creator → Workspace → Tasks → Projects → Content → Analytics → Growth`

BEZNIGATIVA должна ощущаться как живой production office, который находится одновременно в Telegram и в браузере.
