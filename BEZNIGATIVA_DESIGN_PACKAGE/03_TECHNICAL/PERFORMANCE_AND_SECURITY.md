# PERFORMANCE / SECURITY / RELIABILITY

## Loading UX
Каждый маршрут имеет:
- route-level loading UI;
- skeleton совпадающий по геометрии с реальным content;
- progressive reveal;
- error boundary;
- retry action;
- optimistic UI только для безопасных операций.

## Performance budget
Target:
- быстрый first contentful render;
- минимальный client JS;
- heavy canvas/3D isolated in lazy-loaded client boundary;
- images optimized, modern formats where possible;
- no giant initial asset bundle;
- lazy-load below-the-fold images;
- prefetch вероятных next routes, но не всё подряд.

## Pixel Office
- Asset atlas/sprite batching.
- Frustum/viewport culling.
- Animation tick only for active/visible characters.
- Background scene can be static texture.
- Pause or reduce FPS when tab hidden/backgrounded.
- Reduce visual effects on mobile/low-power devices.
- Max active animated characters per device class is configurable.

## Realtime
Presence:
- online/offline;
- current page/zone;
- current coarse activity state.
Не использовать Presence для high-frequency cursor/mouse movement.
Broadcast:
- chat typing;
- transient UI events;
- game/office effects;
- notification signals.
DB changes:
- durable state transitions.

## Security
- RBAC + server-side authorization.
- RLS for tenant/entity boundaries.
- Never trust client-supplied role or user id.
- Telegram init data validated server-side.
- Signed file URLs for protected assets.
- Rate limit bot/webhook/public APIs.
- Audit logs for privileged actions.
- 2FA/passkey for admins where available.
- Sanitize user content.
- Never expose service keys in browser bundle.

## Accessibility
- keyboard navigation;
- visible focus;
- semantic buttons;
- aria labels for icon-only actions;
- reduced-motion mode;
- sufficient contrast;
- touch target >= 44x44 px on mobile.
