# BEZNIGATIVA — Technology & Library Registry

Version: 1.1
Purpose: canonical implementation registry for the coding agent.

## 0. Core rule

Install dependencies from official npm/GitHub/documentation sources. Do not invent equivalent libraries. Do not add a second animation engine just because a component is difficult. Prefer the registered stack and justify any deviation in `TECH_CHANGELOG.md`.

## 1. Application core

| Layer | Technology | Role | Source |
|---|---|---|---|
| Framework | Next.js 16 | Full-stack React application, routing, server/client boundaries, image/font optimization | https://nextjs.org/docs |
| UI runtime | React 19 | Component model | https://react.dev/ |
| Language | TypeScript | Strict typing everywhere | https://www.typescriptlang.org/docs/ |
| CSS | Tailwind CSS | Utility styling and tokens | https://tailwindcss.com/docs |
| UI primitives | shadcn/ui + Radix primitives | Accessible primitives; adapt visually to BEZNIGATIVA, do not ship default shadcn styling | https://ui.shadcn.com/ |

## 2. State, server data, forms

| Package | Role | Source |
|---|---|---|
| `zustand` | Local/global client state; office UI state, filters, session-adjacent UI state | https://zustand.docs.pmnd.rs/ |
| `@tanstack/react-query` | Server-state caching, mutations, invalidation, prefetching | https://tanstack.com/query/latest/docs/framework/react/installation |
| `zod` | Runtime schema validation and typed boundaries | https://zod.dev/ |

## 3. Pixel / interactive 2D engine

| Package | Role | Source |
|---|---|---|
| `pixi.js` 8.x | Pixel-office renderer, character sprites, particles, interactive 2D scene | https://pixijs.com/ |
| `@pixi/react` | React integration for PixiJS | https://github.com/pixijs/pixi-react |

### PixiJS rule
Use PixiJS for the living pixel-office layer, not for ordinary HTML dashboards. HTML remains the canonical semantic UI. Pixi is an enhancement layer.

## 4. 3D engine

| Package | Role | Source |
|---|---|---|
| `three` | 3D rendering | https://threejs.org/ |
| `@react-three/fiber` | React renderer for Three.js | https://r3f.docs.pmnd.rs/ |
| `@react-three/drei` | Camera, controls, loaders, text, helpers and reusable abstractions | https://drei.docs.pmnd.rs/ |

### 3D rule
Use R3F/Three only for real 3D scenes or 3D presentation effects. Do not replace the entire application UI with WebGL.

## 5. Animation stack

### Motion for React
`motion` — page transitions, layout transitions, dialogs, micro-interactions, gestures and responsive animation.
Source: https://motion.dev/docs/react

### GSAP
`gsap` — timeline-heavy sequences, complex sequencing, advanced SVG/canvas choreography, scroll-driven sequences and production-grade orchestration.
Source: https://gsap.com/docs/v3/Installation/

### GSAP React integration
`@gsap/react` — scoped GSAP lifecycle / cleanup in React.
Source: https://gsap.com/docs/v3/Installation/

### Animation decision rule
1. CSS transitions first for trivial hover/focus/color changes.
2. Motion for normal React UI motion.
3. GSAP for complex timelines or imperative sequencing.
4. PixiJS ticker/scene animation for pixel-office actors and particles.
5. R3F `useFrame` only for actual 3D scene updates.
6. Never animate the same property of the same element through two engines at once.

## 6. Icons

Primary utility icon source:
- `@tabler/icons-react` — MIT, tree-shakable React components: https://www.npmjs.com/package/@tabler/icons-react

Alternative utility icon source:
- `lucide-react` — ISC: https://www.npmjs.com/package/lucide-react

### Icon rule
Use utility icon libraries for conventional interface symbols. Pixel-art navigation, status badges and branded decorative icons must use our custom BEZNIGATIVA assets, not generic outline icons.

## 7. Realtime

Supabase Realtime:
- Presence for slow-changing state such as online/offline and current office presence.
- Broadcast for low-latency transient events and interaction signals.
- Postgres Changes only where database change propagation is actually needed.
Source: https://supabase.com/docs/guides/realtime

## 8. Telegram

Telegram Mini App integration is based on the official Telegram Web App API.
Source: https://core.telegram.org/bots/webapps

Critical security rule: validate Telegram `initData` on the server. Never trust `initDataUnsafe` for authorization.

## 9. Package baseline

Recommended production baseline:

```bash
npm install next@16 react@19 react-dom@19 typescript tailwindcss motion gsap @gsap/react pixi.js @pixi/react three @react-three/fiber @react-three/drei zustand @tanstack/react-query zod @tabler/icons-react
```

Add only project-required packages after implementation evidence. Keep the lockfile committed.

## 10. Performance requirements

- Do not render the pixel office on routes that do not need it.
- Lazy-load PixiJS and R3F scenes.
- Do not load all character sprites on first page load.
- Use texture atlases or logically grouped assets for the office scene.
- Respect `prefers-reduced-motion`.
- Pause or degrade non-critical scene animation when the tab is hidden.
- Prefer transforms/opacity over layout-triggering properties for frequent motion.
- Never run a high-frequency React state update for every animation frame.
- Virtualize large lists/tables.
- Use Next.js image/font optimization and route-level code splitting.
- Keep dashboard interaction responsive even when the pixel office is active.
