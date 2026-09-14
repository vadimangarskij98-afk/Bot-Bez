# ТЕХНИЧЕСКАЯ АРХИТЕКТУРА

## Целевой стек
- Next.js 16 + App Router.
- React 19.3.
- TypeScript (strict).
- Tailwind CSS + CSS variables/design tokens.
- shadcn/ui или Radix-подобные headless primitives для доступных компонентов.
- TanStack Query для server state/cache/mutations.
- PostgreSQL через Supabase.
- Supabase Realtime: Broadcast для событий/чатов, Presence для slow-changing online state.
- Telegram Bot API + Telegram Mini Apps.
- Redis — по необходимости для rate limit, очередей/короткоживущего cache и coordination, но не как источник истины.
- Object Storage для media/files.
- Motion для UI transitions/layout/gestures.
- GSAP для сложных sequenced/timeline effects, когда Motion неудобен.
- PixiJS 8 для 2D pixel-office сцены, если нужна high-sprite-count canvas-рендеризация.
- React Three Fiber / Three.js — только для ограниченных 3D overlays/rooms, НЕ для всего приложения.

## Правило выбора технологии
1. DOM/CSS first.
2. Motion для UI micro-interactions.
3. GSAP только там, где нужна сложная timeline orchestration.
4. PixiJS для большого количества пиксельных sprite-объектов.
5. R3F/Three.js только для реального 3D, не заменять им обычный UI.

## Архитектурные слои
Presentation → Feature modules → Domain services → API/BFF → Postgres/Supabase → Integrations.

## Routes
`/login`
`/onboarding`
`/app`
`/app/tasks`
`/app/tasks/[id]`
`/app/team`
`/app/chat`
`/app/calendar`
`/app/knowledge`
`/app/analytics`
`/app/finance`
`/app/opportunities`
`/app/documents`
`/app/settings`
`/admin/applications`
`/admin/applications/[id]`
`/admin/creators/[id]`
`/admin/projects`

## State ownership
- Server state: TanStack Query.
- UI state: local state or small scoped store.
- Global UI: minimal Zustand-like store only when genuinely needed.
- Realtime event bus: typed event envelopes.

## Event envelope
```ts
{
  id: string,
  type: 'task.updated' | 'application.updated' | 'presence.changed' | 'message.created' | ...,
  actorId: string,
  entityId: string,
  timestamp: string,
  version: number,
  payload: unknown
}
```
Все mutations идемпотентны где возможно. Клиент не считается источником истины.

## Telegram sync
- Telegram user id связывается с internal user id.
- Bot webhook -> server action/route handler -> domain event -> DB/realtime/notification.
- Web/Mini App используют одну сущность пользователя и единый permission model.
- Уведомления из backend имеют preference layer.
- Смена статуса сотрудника должна отражаться: DB → Realtime → Web/Pixi → Telegram notification, если уведомление требуется.
