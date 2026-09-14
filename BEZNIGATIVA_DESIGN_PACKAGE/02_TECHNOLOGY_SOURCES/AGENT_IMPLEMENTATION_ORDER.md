# BEZNIGATIVA — Mandatory Implementation Order

The coding agent MUST follow this order unless a documented blocker forces a change.

## Phase 1 — Specification lock

1. Read `00_DOCUMENTATION/BEZNIGATIVA_TZ_v1.md`.
2. Read `00_DOCUMENTATION/BEZNIGATIVA_DESIGN_SPEC_v1.md`.
3. Read `05_RULES/DESIGN_LOCK.md`.
4. Read `05_RULES/AI_AGENT_IMPLEMENTATION_RULES.md`.
5. Read `03_EXTERNAL_ASSET_SOURCES/PIXEL_ASSET_SOURCE_REGISTRY.md`.

Do not code before these are understood.

## Phase 2 — Technology installation

Install only the registered baseline packages in `STACK_AND_LIBRARIES.md`.

Create:
- strict TypeScript;
- ESLint;
- formatting;
- test harness;
- route structure;
- environment-variable schema;
- CI checks.

## Phase 3 — Design system

Implement:
- black liquid-glass shell;
- typography;
- tokens;
- spacing;
- buttons;
- inputs;
- cards;
- status chips;
- tabs;
- modals;
- drawers;
- tables;
- skeletons;
- toast/notification system;
- responsive navigation.

Do not implement pixel-office scene yet.

## Phase 4 — Functional web platform

Implement desktop workflows first:
- authorization;
- dashboard;
- applications;
- candidate profile;
- participant workspace;
- tasks;
- task detail/review;
- team;
- chat;
- calendar;
- knowledge base;
- analytics;
- finance;
- opportunities;
- documents/contracts;
- settings.

## Phase 5 — Telegram

Connect:
- Bot API;
- Mini App;
- login/session bridge;
- application flow;
- notification events;
- deep links;
- Telegram message templates.

Validate Telegram init data server-side.

## Phase 6 — Realtime

Implement Presence/Broadcast/event-driven updates.

Verify:
- admin changes reflect on participant UI;
- participant status reflects on Team;
- task updates propagate without reload;
- chat receives messages without reload;
- Telegram notifications fire once per logical event.

## Phase 7 — Pixel Office

Only after the core application is stable:
- load PixiJS lazily;
- create room system;
- add navigation nodes;
- add character manager;
- bind employee status;
- add clock;
- add particles and subtle effects;
- add interaction overlays.

## Phase 8 — Optional 3D

Add R3F/Three only where the storyboard calls for actual 3D.

## Phase 9 — Verification

Every screen must be checked against the visual storyboard.

Reject the build when:
- sidebar changes shape without specification;
- colors drift;
- typography changes;
- glass effect becomes excessive;
- pixel office style changes;
- mobile navigation diverges;
- animation causes visible lag;
- a visual asset is replaced by an unrelated stock component.
