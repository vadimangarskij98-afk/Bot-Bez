# BEZNIGATIVA — Animation & Pixel Office Architecture

## 1. Visual runtime model

The web application has three motion domains:

1. **DOM/UI motion** — navigation, cards, dialogs, filters, lists, hover/focus, responsive transitions.
2. **Pixel Office runtime** — sprites, NPC movement, status bubbles, particles, office clock and presence visualization.
3. **3D scene runtime** — optional true 3D scenes and presentation moments.

They communicate through application state, not by sharing an ad-hoc animation loop.

## 2. Library ownership

- Motion: UI-level declarative animation.
- GSAP: complex sequences and timelines.
- PixiJS: pixel-office render loop.
- R3F/Three: real 3D render loop.
- CSS: tiny isolated transitions.

## 3. Pixel office state machine

Each employee has:

```text
OFFLINE
  ↓
ENTERING
  ↓
IDLE
  ↘
   WALKING → WORKING
      ↘        ↘
      TALKING  THINKING
          ↘      ↙
           AWAY
            ↓
          EXITING
```

The authoritative state comes from backend presence/work-status data. The renderer interpolates the visual state locally.

## 4. Workday interaction

When a team member presses `Я НА РАБОЧЕМ МЕСТЕ`:

1. Client sends a server mutation.
2. Server validates the user and workspace permission.
3. Presence state becomes `at_work`.
4. Realtime publishes the state change.
5. The member's web UI updates.
6. The pixel office receives the event.
7. The character enters through the office entrance.
8. Character chooses a target workstation.
9. Character walks to target using pathfinding / predefined navigation nodes.
10. Character switches to `WORKING`.

No client-side fake status is accepted as authoritative state.

## 5. Clock

The office clock must be a real time display. It must not be driven by a fake counter. The UI uses a server-synchronized timestamp when exact consistency matters.

For timer/countdown UI:
- calculate from absolute timestamps;
- render locally;
- do not persist every second as a database write.

## 6. Sprite requirements

Production character files should be separate assets or logically packaged atlases, never a giant screenshot requiring runtime cropping.

Minimum animation set:
- idle
- walk cycle
- work
- interact
- talk
- phone
- think
- celebrate
- sleep/away
- enter
- exit

Character scale must be consistent across all roles.

## 7. Asset loading

Load asset groups progressively:
- shell/UI assets first;
- current character set second;
- active room third;
- decorative effects last.

Do not decode hundreds of large textures on first paint.

## 8. Motion accessibility

Honor `prefers-reduced-motion`.

Reduced motion mode:
- remove non-essential camera movement;
- reduce particle count;
- disable continuous idle bobbing where unnecessary;
- keep essential state transitions understandable through fades or instant changes.

## 9. Frame-budget rules

Desktop target: smooth 60fps baseline; higher refresh rates are supported when available.

Avoid:
- React state updates per animation frame;
- layout reads/writes inside tight loops;
- heavy blur/filter stacks over full-screen regions;
- dozens of independent DOM shadows/glows animating simultaneously;
- full-scene rerenders when one character changes.

Prefer:
- GPU-friendly transforms;
- batched Pixi sprites;
- atlas textures;
- dirty-region/local updates where possible;
- capped particle systems;
- visibility culling;
- page/route-level lazy loading.

## 10. Interaction hierarchy

The pixel office is a living visualization, not the only control surface. Every critical action must remain available in normal UI.

Example:
- clicking a character opens that person's profile;
- but the same status is available in the Team screen;
- a user never loses access because the canvas failed to load.

## 11. Failure mode

If PixiJS/WebGL fails, show a static office fallback with status cards. The rest of the application MUST continue to work.

If 3D is unavailable, replace it with the approved 2D/reference scene. Never block authentication, tasks, applications, chat or admin workflows because a visual layer failed.
