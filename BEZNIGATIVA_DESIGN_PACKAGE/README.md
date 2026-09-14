# BEZNIGATIVA — MASTER DESIGN & ENGINEERING PACKAGE

Версия: 1.1
Назначение: эталон для AI-агента разработки интерфейса и frontend/backend реализации.

## Главное правило
Этот пакет является визуальным и техническим источником истины. Агент НЕ должен самовольно менять визуальный стиль, информационную архитектуру, названия разделов, компоновку, сетку, типографику или поведение без явной причины и без сохранения общего направления.

Платформа: BEZNIGATIVA — рабочая среда для блогеров, музыкантов, креаторов и команды продакшна.
Основные поверхности: Web App, Admin Panel, Creator Workspace, Telegram Mini App, Telegram Bot.

## Состав
- `00_DOCUMENTATION/` — исходное ТЗ и контекст из предыдущих этапов.
- `01_STORYBOARD/` — обзорные storyboard/reference images.
- `02_PAGE_SPECS/` — отдельная спецификация каждой страницы.
- `03_TECHNICAL/` — архитектура, performance, realtime, security.
- `04_ANIMATION/` — motion, GSAP, Motion, PixiJS/R3F, accessibility.
- `05_RULES/` — design lock, запреты и acceptance checklist.
- `06_REFERENCES/` — актуальные web/visual references.

## Визуальный эталон
Фирменное направление: Black Liquid Glass + neon purple/blue accents + isometric pixel office.
Glass используется дозированно: sidebar, overlays, modals, premium/interactive surfaces; базовые data surfaces не превращаются в стеклянную кашу.

## Экраны
Авторизация, onboarding, dashboard/office, applications, candidate profile, creator workspace, tasks, task details, team, chat, calendar, knowledge base, analytics, finance, opportunities, documents/contracts, settings, Telegram bot, Telegram Mini App, employee status/office, pixel office locations.

## Критическое требование
UI полностью на русском языке. Тексты интерфейса, кнопки, статусы, ошибки, empty states, loading states, подсказки и notifications — на русском, кроме технических идентификаторов в коде.

## New canonical technology/assets directories

- `02_TECHNOLOGY_SOURCES/` — implementation stack, package registry and mandatory build order.
- `03_EXTERNAL_ASSET_SOURCES/` — vetted pixel/2D/3D asset sources and license policy.
- `04_ANIMATION_ARCHITECTURE/` — Motion/GSAP/PixiJS/R3F responsibilities and Pixel Office behavior.
