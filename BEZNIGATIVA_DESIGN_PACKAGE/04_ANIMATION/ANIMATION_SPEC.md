# АНИМАЦИИ — MOTION / GSAP / PIXI / R3F

## Общая философия
Анимация должна объяснять состояние и навигацию, а не мешать работе.

### Durations
- Micro: 120–180 ms.
- Standard: 180–280 ms.
- Major transition: 280–450 ms.
- Office scene transition: 350–800 ms.

### Easing
- UI: spring/ease-out.
- Popover/modal: soft spring.
- Data/chart: ease-out.
- Pixel character movement: constant/lerp with discrete sprite frames.

## Motion
Использовать для:
- hover/tap;
- dialog enter/exit;
- layout changes;
- list reorder;
- drawer/side panel;
- subtle number transitions.

Использовать `layoutId` только для intentional shared-element transitions.

## GSAP
Использовать для:
- multi-step intro choreography;
- coordinated hero/office sequences;
- timeline-based storytelling;
- pointer/scroll-driven special effects.
Не использовать GSAP вместо CSS/Motion для простых fades/transforms.

## PixiJS
PixiJS предназначен для pixel-office сцены:
- персонажи;
- растения;
- мебель;
- particles;
- room props.
Использовать WebGL production path; WebGPU optional progressive enhancement.

## R3F / Three.js
Только для:
- 3D camera effects;
- optional room depth;
- special showcase scenes.
Не рендерить обычные cards, text, sidebar или таблицы через WebGL.

## Animation architecture
UI animation layer:
`motion/react`

Scene animation layer:
`pixi.js`

Special timeline layer:
`gsap`

3D showcase layer:
`@react-three/fiber`

## Reduced motion
При `prefers-reduced-motion: reduce`:
- отключить camera movement;
- убрать parallax;
- заменить spring на instant/opacity;
- pause ambient office motion;
- оставить смысловые status changes.

## Mobile
На слабом устройстве:
- 30 FPS scene cap;
- минимум particles;
- только видимые персонажи;
- static background;
- reduced blur.
