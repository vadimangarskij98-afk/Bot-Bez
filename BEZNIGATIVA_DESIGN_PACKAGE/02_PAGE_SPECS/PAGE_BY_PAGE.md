# BEZNIGATIVA — PAGE-BY-PAGE VISUAL SPECIFICATION v1.1

Все размеры — CSS px для desktop reference width 1440. Контейнеры обязаны быть fluid между breakpoints, но пропорции и иерархия сохраняются.

## GLOBAL LAYOUT TOKENS

### Desktop
- viewport reference: 1440×900.
- sidebar: 264 px wide, fixed/sticky.
- topbar inside content: 64 px high.
- content max-width: 1440 px; outer gutter 24 px.
- page content top padding: 24 px.
- standard card gap: 16 px.
- card radius: 14 px; hero radius 18–22 px.
- control height: 40 px; compact 32 px; large CTA 48 px.
- icon sizes: 16 / 20 / 24 px.
- avatar: 32 / 40 / 56 / 72 px.

### Tablet
- sidebar collapses to 72 px rail or overlay.
- content gutter 20 px.
- 8-column grid.

### Mobile
- viewport reference 375×812.
- side navigation becomes bottom navigation or overlay.
- page gutter 16 px.
- bottom nav 64–72 px + safe-area inset.
- primary controls >=44×44 px.

### Surface hierarchy
1. Canvas/background.
2. Primary workspace surface.
3. Card.
4. Popover/modal.
5. Focus/hover highlight.

Не использовать одинаковую opacity на всех уровнях.

---

## 01 — АВТОРИЗАЦИЯ / ВХОД

**Роль:** вход для браузера и Telegram.

Desktop:
- full viewport.
- background pixel-office layer at 55–70% visual prominence.
- centered login card: width 460 px, min-height 520 px.
- logo block 56–72 px high.
- title H1 32 px.
- subcopy max-width 360 px.
- Telegram CTA 48 px high, full width.
- secondary CTA 48 px high, 12 px gap.
- security row 3 items, 12 px label.
- bottom footer 12–13 px.

Mobile:
- card width calc(100%-32 px); office backdrop reduced to keep text readable.

Animation:
- office ambient loop 6–12 s.
- card enter 350 ms; no dramatic zoom.

---

## 02 — ONBOARDING

Desktop card 640 px wide; mobile full-width content with 16 px gutter.
Progress: 4–6 px line under header.
Question title 24 px; answer rows 48–56 px high; selected state uses purple border/glow.
Sticky next button mobile.
Save after every step; never lose form progress.

---

## 03 — DASHBOARD / PIXEL OFFICE

Header 64 px.
Hero office module 48–56% of available width on desktop.
Stats strip beneath office: four cards, min-width 180 px each.
Right/secondary content: upcoming tasks, applications, activity.
Office scene has an explicit “В офисе / Онлайн / В работе / На созвоне” legend.

Pixel office viewport should support zoom levels 0.9–1.25, but default must show whole meaningful room.

---

## 04 — СИСТЕМА ЗАЯВОК

Header: title + count + create/import actions.
Status tabs height 40 px.
Search field 280–360 px.
Table row 64–72 px; avatar 40 px.
Columns: кандидат / тип / аудитория / AI-score / статус / дата / действие.
Right drawer on selection: 420–520 px.
Bulk actions appear only after selection.

Mobile: list cards 96–120 px; drawer becomes full-screen sheet.

---

## 05 — ПРОФИЛЬ КАНДИДАТА

Header block 120–160 px.
Avatar 72 px; name H1 28–32 px; role/status underneath.
Primary decisions aligned right.
Metrics: 3–5 compact cards.
Tabbed content below: обзор / соцсети / файлы / AI / заметки / история.
AI score must be supplementary; human review action remains visually primary.

---

## 06 — КАБИНЕТ УЧАСТНИКА

Top hero card 180–220 px.
Status control prominent: “Я на рабочем месте” 48 px.
Next Action module first, then tasks/projects/opportunities.
Use progressive disclosure: show top 3 active tasks before the rest.

---

## 07 — ЗАДАЧИ

Toolbar 56–64 px.
Desktop board: 4 status columns.
Column min-width 260 px; gap 16 px.
Task card min-height 112 px.
Card metadata order: priority → due date → project → assignee.

Mobile defaults to list; status filter opens bottom sheet.

---

## 08 — ПРОСМОТР ЗАДАЧИ

Main two-column layout 70/30.
Left: title, description, checklist, files, reference media, comments.
Right: status, priority, assignee, project, deadline, submit/review CTA.
Action bar sticky on desktop and mobile bottom sheet.
Review state must make “Принять” and “Запросить доработку” visually distinct.

---

## 09 — КОМАНДА / СОТРУДНИКИ

Toolbar: search + status + department filter.
Desktop list/grid toggle.
Person row 72 px high; avatar 40 px; status dot 8–10 px.
Optional pixel character preview appears in detail popover.
Presence is realtime; do not poll every few seconds.

---

## 10 — ЧАТ / КОММУНИКАЦИИ

Desktop 3-pane:
- chat list 280–320 px;
- message area flexible;
- context 280–340 px.
Message composer 52–60 px.
Use task/project reference cards inside chat.
Typing indicator below latest message and not as a global notification.
Mobile: one pane with animated push transition.

---

## 11 — КАЛЕНДАРЬ

Desktop week view default.
Sidebar/context 260–300 px; calendar remainder.
Time gutter 56 px.
Event min-height 40 px.
Header 56 px + date selector.
Mobile default agenda; day/week available via segmented control.

---

## 12 — БАЗА ЗНАНИЙ

Header search 320–520 px.
Category rail 220–240 px.
Article cards 240–320 px width.
Article body max-width 720–760 px for reading comfort.
Related links at bottom.
AI summary sits in a collapsible card, never above original source title without user intent.

---

## 13 — АНАЛИТИКА

Top 4 KPI cards, 24–32 px value typography.
Main chart 16:7 or similar wide ratio.
Secondary charts 1:1 or 4:3.
Date range and segment controls always visible above charts.
Use progressive rendering: KPIs first, charts second, deeper breakdowns on demand.

---

## 14 — ФИНАНСЫ

Sensitive area: explicit permission guard.
Overview cards: total income / expenses / payable.
Transaction table row 60–64 px.
No saturated red/green everywhere; reserve success/error for deltas/status.
Payout CTA 48 px height, sticky where appropriate.

---

## 15 — ВОЗМОЖНОСТИ

Cards 320–420 px desktop; one-column mobile.
Each card: type badge, title, short brief, requirements, deadline/budget, CTA.
Avoid making every card glow; only actionable/high-priority opportunities get emphasis.

---

## 16 — ДОКУМЕНТЫ / КОНТРАКТЫ

Tabs by type/status.
Document row 64–72 px.
File icon 32–40 px.
Preview drawer 560–720 px wide desktop.
Restricted documents show locked state with explanation.

---

## 17 — НАСТРОЙКИ

Settings shell: sidebar/section list 240 px + form area.
Form width max 720 px.
Danger zone isolated at bottom.
Save states: saved / saving / unsaved changes.
Language defaults to русский.

---

## 18 — TELEGRAM BOT

Conversation UI is Telegram-native.
Bot messages concise.
Primary buttons grouped max 2–3 per row.
Use Mini App deep link for large forms, task boards, knowledge pages.
Critical bot messages must be idempotent and traceable to event id.

---

## 19 — TELEGRAM MINI APP

Mobile-first 375×812 reference.
Bottom nav 5 items max.
Header 56 px.
Card radius 14–16 px.
Use Telegram safe-area variables.
Do not duplicate the entire desktop sidebar.
Mini App shares data/permissions with Web App.

---

## 20 — СТАТУС СОТРУДНИКА / ОФИС

Status selector includes:
- В сети;
- В офисе;
- В работе;
- На созвоне;
- Отошёл;
- Не в сети.

Primary control: “Я на рабочем месте”.
When toggled on:
1. persist status server-side;
2. broadcast presence event;
3. update pixel character state;
4. update team/dashboard cards;
5. optionally send Telegram acknowledgment.

---

## 21 — PIXEL OFFICE / ЛОКАЦИИ

Rooms:
- Ресепшен;
- Рабочая зона;
- Студия;
- Переговорная;
- Зона отдыха;
- Кухня;
- Музыкальная;
- Серверная;
- Creative room.

Each room is a real product workspace, not a decorative mini-game.
Every interactive station maps to real entities: user, task, project, meeting, opportunity or content.

## Pixel scene layers
1. static background/floor;
2. architecture;
3. furniture;
4. interactive objects;
5. characters;
6. status indicators;
7. effects/UI overlays.

Characters must use deterministic state from backend; random idle behavior may supplement but never override real status.
