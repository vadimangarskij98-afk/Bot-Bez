# DEFINITION OF DONE

## Visual
- Pixel office visual language preserved.
- Black Liquid Glass treatment consistent.
- Typography and spacing use tokens.
- No arbitrary new accent colors.

## Functional
- API/domain logic implemented.
- Permissions enforced server-side.
- Telegram synchronization tested.
- Realtime events typed and handled.

## UX
- Loading skeleton.
- Empty state.
- Error state + retry.
- Success feedback.
- Mobile layout.
- Reduced motion.

## Engineering
- TypeScript strict passes.
- Lint passes.
- Unit/integration tests for critical flows.
- Production build passes.
- No secret leakage.
- No unbounded polling.
- No unnecessary render loops.

## Performance
- Heavy scenes lazy-loaded.
- Images optimized.
- Asset chunks separated.
- Pixel office pauses/reduces FPS when not visible.
- Realtime subscriptions unsubscribed on route unmount.
