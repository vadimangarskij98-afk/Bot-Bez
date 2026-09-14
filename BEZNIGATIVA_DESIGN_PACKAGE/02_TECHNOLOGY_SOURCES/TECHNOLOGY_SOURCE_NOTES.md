# Verified source notes — 2026-09-14

The registry was checked against current official sources/search results on 2026-09-14.

- Next.js docs: https://nextjs.org/docs
- Motion for React: https://motion.dev/docs/react
- GSAP installation/docs: https://gsap.com/docs/v3/Installation/
- PixiJS: https://www.npmjs.com/package/pixi.js
- PixiJS React: https://www.npmjs.com/package/@pixi/react
- React Three Fiber: https://r3f.docs.pmnd.rs/
- Drei: https://drei.docs.pmnd.rs/
- TanStack Query: https://tanstack.com/query/latest/docs/framework/react/installation
- Zustand: https://zustand.docs.pmnd.rs/
- Zod: https://zod.dev/
- Tabler Icons React: https://www.npmjs.com/package/@tabler/icons-react
- Lucide React: https://www.npmjs.com/package/lucide-react
- Supabase Realtime: https://supabase.com/docs/guides/realtime
- Telegram Mini Apps: https://core.telegram.org/bots/webapps

Current source registry observations:
- PixiJS 8.x is the intended 2D engine; the npm package currently publishes 8.20.1.
- @pixi/react supports React 19 and PixiJS 8.
- R3F 9.x pairs with React 19.
- Drei provides ready-made helpers such as cameras, controls, Text3D, loaders and useAnimations.
- Motion supports React layout/gesture/scroll animation and a client import for Next.js App Router.
- GSAP provides timelines and advanced plugins and is available through npm.
- Supabase Realtime separates Presence for slower-changing shared state from Broadcast for low-latency transient events.
- Telegram's official Web App API documents server validation of `initData` and warns against trusting `initDataUnsafe`.

Do not hard-code these observed versions into the application without checking the package manager at install time. Pin versions through the lockfile after installation and regression testing.
