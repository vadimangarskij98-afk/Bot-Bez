# RULES FOR AI DEVELOPMENT AGENT

## MUST
1. Read all files in this package before implementing.
2. Preserve Russian UI.
3. Preserve the exact visual direction from storyboard images.
4. Implement reusable components; do not duplicate page markup.
5. Every async surface must support loading, error, empty, success.
6. Realtime must update UI without manual refresh for presence, tasks, applications, chat, notifications.
7. Keep heavy pixel/3D code isolated in lazy client boundaries.
8. Validate all permissions server-side.
9. Use design tokens; do not hardcode dozens of arbitrary colors.
10. Keep animation purposeful and respect reduced motion.
11. When uncertain, choose the simplest implementation that preserves the reference visual behavior.
12. Before finishing each feature, run typecheck, lint, tests and build.

## MUST NOT
- Do not redesign the information architecture.
- Do not replace pixel office with generic stock/3D art.
- Do not silently rename sections.
- Do not remove Telegram integration because web works.
- Do not create a separate source of truth for Telegram and Web user profiles.
- Do not put secrets in client code.
- Do not use canvas/WebGL for ordinary dashboard UI.
- Do not add dependencies without reason.
- Do not fetch giant assets globally.
- Do not ship console errors.

## Acceptance criteria
A feature is not complete until:
- desktop is correct;
- mobile is correct;
- loading/error/empty states exist;
- Telegram flow works where applicable;
- realtime behavior works;
- keyboard/focus behavior is acceptable;
- no layout shift on async data insertion;
- performance is acceptable on mid-range mobile;
- visual deviation from storyboard is minimal.
