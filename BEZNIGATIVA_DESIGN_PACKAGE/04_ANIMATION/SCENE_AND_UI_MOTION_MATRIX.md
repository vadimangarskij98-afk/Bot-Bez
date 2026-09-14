# MOTION MATRIX

| Interaction | Layer | Default | Mobile | Notes |
|---|---|---:|---:|---|
| Button hover | CSS/Motion | 140ms | disabled/minimal | translateY(-1px), subtle glow |
| Button press | CSS/Motion | 90–120ms | 90ms | scale .98 |
| Drawer open | Motion | 240ms | 220ms | slide + opacity |
| Modal open | Motion | 260ms | 220ms | scale 0.98→1 |
| List reorder | Motion layout | 240–360ms | 220ms | shared layout only when clear |
| Route transition | View Transition/Motion | 220–320ms | 180–260ms | never delay navigation |
| KPI count | Motion | 500–700ms | 350–500ms | only on meaningful change |
| Chart reveal | Motion/GSAP | 500–800ms | 400–600ms | stagger <= 80ms |
| Office room switch | Pixi/GSAP | 450–700ms | 300–500ms | camera pan/fade |
| Character walk | Pixi | data-driven | lower FPS | sprite frames, no DOM per frame |
| Ambient particles | Pixi | 30–60fps | 15–30fps | optional/reducible |
| Chat typing | Motion/CSS | 300ms loop | 300ms | subtle |
| Notification arrival | Motion | 220ms | 200ms | no screen shake |

## Hard limits
- No animation should block critical input.
- No perpetual CSS animation on dozens of DOM nodes.
- No full-scene rerender for one character status change.
- No unbounded particle counts.
- Canvas/WebGL hidden when feature not visible.
