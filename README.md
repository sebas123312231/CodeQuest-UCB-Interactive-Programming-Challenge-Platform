# CodeQuest UCB — Interactive Programming Challenge Platform

[![Astro](https://img.shields.io/badge/Astro-5-FF5D01?logo=astro&logoColor=white)](https://astro.build/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=111827)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Supabase](https://img.shields.io/badge/Supabase-Data%20services-3ECF8E?logo=supabase&logoColor=111827)](https://supabase.com/)
[![Vitest](https://img.shields.io/badge/Vitest-5-6E9F18?logo=vitest&logoColor=white)](https://vitest.dev/)

CodeQuest UCB is an **interactive programming challenge platform** designed to run a live, station-based technology event. It turns a set of coding, logic, design and operator-led activities into one coherent web product with multiple routes, reusable game surfaces, team progress, validation workflows and a final prize draw.

The university setting is the context of the event. The engineering problem was broader: provide a reliable platform that operators could use throughout an in-person activity to launch different challenges, identify teams, record progress and keep the experience moving from one station to the next.

## About the Project

The event needed several distinct experiences with a shared operating model. Every station had to feel like part of the same product while still supporting its own interaction model, timing rules and completion criteria.

The platform supports this flow:

1. An operator registers or imports the event teams.
2. A station is opened from the challenge launcher or a direct station route.
3. A team completes the selected experience on the event computer or display.
4. The operator selects the team and validates the station with the operator PIN.
5. The team route and completion state are updated, making the final prize workflow possible.

The result is a multi-module event application rather than a static informational page: it combines gameplay state, operational controls, data services, local resilience and testable domain logic.

## My Role & Contributions

This was a collaborative project, but the Git history shows a sustained and substantial implementation contribution from **Sebastian Arce Antezana**. The primary `sebas123312231` identity authored 46 commits, with two additional commits under related aliases. The contribution claims below are limited to the areas represented by those commits and the current code paths they changed.

- **Product architecture and domain modeling:** refined the event and team types, centralized station metadata and route sequencing, added the four-digit team ID generator and structured station progress, scores, timings and completion details.
- **Persistence and resilience:** implemented the local event state layer with `localStorage`, normalization of persisted data, remote-team hydration, individual and full-event resets, prize state and winner history.
- **Data and service integration:** built the Supabase browser/server clients, typed database model and teams migration; implemented the protected `/api/teams` and `/api/operator` routes, client API helpers and the remote/local team-loading hook.
- **Operator authentication and validation:** implemented the server-side PIN flow, HMAC-signed operator cookie, station validation hook and reusable team validation controls that connect gameplay completion to event progress.
- **Interactive product UI:** implemented the shared game shell, rules dialog, fullscreen control, accessible modal primitives and team detail controls; redesigned the launcher, station navigation and responsive visual system around the event workflow.
- **Challenge experiences and event operations:** developed the current typing, bug-hunt, Hanoi, web-design and Wendo station experiences; implemented the final prize roulette, team directory and moderator console with manual entry, CSV import, editing, activation and reset workflows.
- **Quality and follow-through:** authored the four Vitest test files covering the core catalogs and domain rules, fixed the Bug Hunt evaluation-feedback regression and documented the visual/operational handoff.

The original project baseline and early README are also preserved in the repository history under **Leonardo Camacho Quiroga**. This README distinguishes the platform's complete feature set from the work attributable to my commits.

## Core Experiences / Challenges

### Posta 01 — Speed Test / Typing Challenge

Teams transcribe code snippets in a controlled editor. The experience includes **JavaScript, Python and TypeScript**, with **easy, intermediate and difficult** levels for each language. The React state tracks whether a round has started or finished, elapsed time, character-level accuracy and words per minute. Paste, drop and common paste shortcuts are blocked so the result reflects the team's typing performance.

### Posta 02 — Wendo Blocks

This is an operator-led physical coordination challenge. The interface communicates the observe → coordinate → deliver sequence and provides a ten-minute countdown with pause, resume and reset controls. The physical block construction remains part of the in-person station; the platform supplies the timing and operator validation layer.

### Posta 03 — Web Design / UX Brief

Teams receive one of four briefs, such as a tutoring platform, hackathon dashboard, open-source project network or AI scheduling assistant. They have ten minutes to define the structure, differentiator and pitch for a wireframe or solution, then present it to the station operator. The application manages the brief selection, countdown, reset and completion validation.

### Posta 04 — Bug Hunt / Error Detection

Teams inspect code line by line and select the lines containing intentional bugs. The catalog covers **JavaScript, C++ and TypeScript**, each with three difficulty levels. The evaluator calculates a score from correct and incorrect selections, marks correct, missed and wrong lines, reveals explanations and allows a failed level to be retried before the station is validated.

### Posta 05 — Tower of Hanoi

The Hanoi station is a stateful puzzle with **3, 4 or 5 disks**. Players select a source tower and destination tower, while the domain logic rejects illegal moves and only recognizes a win when all disks reach Tower 3. The UI reports moves, elapsed time and the theoretical minimum, and animates disk movement while respecting reduced-motion preferences.

### Final — Prize Roulette

The final route turns completed event progress into an operator-controlled draw. In strict mode, only teams that completed all five stations are eligible. Operators can select teams, choose and edit prize inventory, run a demo mode, toggle optional sound and record the winner locally. The wheel is drawn on Canvas 2D, uses optional Web Audio ticks during the spin and displays the result in an accessible modal.

## Platform Features

- **Team management:** four-digit IDs, automatic collision-aware generation, team names, member lists, active/inactive status and CSV import with `id,nombre,integrantes` columns.
- **Team directory and search:** search by team ID or name, accent-insensitive filtering, team detail modal, member summary and per-station completion status.
- **Station navigation:** launcher at `/tv`, direct station URLs, in-app station switching, return-to-launcher behavior and shared station metadata.
- **Progress tracking:** circular station routes, next-station calculation, completion count, per-station score/time/details and individual or full-event reset actions.
- **Operator workflows:** server-validated PIN login, quick station approval, team creation/editing, activation toggles, roster refresh, CSV import and reset controls.
- **Shared game shell:** consistent briefing modal, reset action, fullscreen control, station status, timing metadata and reusable team-validation panel across the challenge modules.
- **Local persistence:** team progress, prize inventory and winner history are stored in the browser for event continuity.
- **Remote/local data modes:** active teams can be read from the server or Supabase, with a local catalog fallback when remote services are unavailable.
- **Responsive behavior:** layouts adapt across desktop, tablet and mobile breakpoints, including stacked game panels, compact operator controls, touch-sized actions and single-column team workflows.
- **Accessible interaction details:** skip link, visible focus states, semantic status messages, keyboard-friendly controls, Escape-to-close dialogs, focus restoration and reduced-motion handling.

## Application Routes

| Route | Purpose |
| --- | --- |
| `/` | Event launcher and overview of the five stations. |
| `/tv` | Operator-facing station launcher and station switcher. |
| `/tv/[posta]` | Direct access to a specific station experience. |
| `/equipo` | Searchable active-team directory and route lookup. |
| `/equipo/[id]` | Direct team lookup by ID. |
| `/moderador` | Authenticated operator console for roster and progress management. |
| `/ruleta` | Final prize roulette, inventory editor and local winner history. |
| `/api/operator` | Operator session status, PIN login and logout endpoints. |
| `/api/teams` | Team catalog read, create and update endpoints. |

## Technical Architecture

### Astro as the application shell

Astro 5 provides the route structure, server output configuration and shared `Layout.astro`. The layout includes the global navigation, event metadata, accessibility entry points and `astro:transitions` client-side navigation.

Interactive surfaces are React components mounted as client-only islands where needed. This keeps the route layer straightforward while allowing each game, the moderator panel, the team dashboard and the roulette to own their local interactive state.

### React and typed domain logic

The product is organized around typed domain models such as `Posta`, `Equipo`, `ProgresoPosta`, `Premio` and `SorteoResultado`. Station metadata is centralized in `src/utils/routing.ts`, while challenge catalogs and pure rules live in dedicated utilities:

- `src/components/games/` — the five station experiences.
- `src/components/ui/` — reusable shell, modal, rules, team and validation primitives.
- `src/hooks/` — team synchronization and station-validation workflows.
- `src/lib/` — Supabase clients, API helpers and operator-session logic.
- `src/utils/` — routing, challenge catalogs, Hanoi rules, ID generation and browser persistence.
- `src/pages/` — Astro routes and server API endpoints.
- `tests/` — Vitest coverage for challenge catalogs and pure domain rules.

### Data, persistence and service boundaries

Supabase provides the remote team catalog through a typed `public.teams` table. Row Level Security is enabled and the public policy only exposes active teams for reads. Server-side writes use the service-role client behind the protected API route; the service-role key and operator secrets are not used in client code.

The client uses `useEventTeams` to reconcile the remote catalog with the browser's local event state. `localStorage` preserves team progress, prize inventory and winner history, while `BroadcastChannel` plus a local custom event notify other tabs in the same browser when event data changes.

Operator authentication is intentionally separate from participant data access. The server validates `MODERATOR_PIN`, issues an HMAC-signed `HttpOnly`, `SameSite=Strict` cookie and checks its lifetime on protected requests. Station validation uses the same operator session before marking a team complete.

## Tech Stack

| Area | Technologies |
| --- | --- |
| Frontend | Astro 5, React 18, TypeScript 5, Tailwind CSS 3, centralized CSS design system |
| Application structure | Astro server output, Vercel adapter, React client-only islands, Astro ClientRouter |
| Data / backend services | Supabase JS, Supabase CLI, typed teams table, Astro API routes |
| State and persistence | React state/hooks, `localStorage`, `BroadcastChannel`, browser custom events |
| Testing | Vitest 5 with focused unit tests for catalogs, puzzle rules and ID generation |
| Browser APIs | Canvas 2D, Web Audio API, Fullscreen API, `matchMedia` for reduced motion |
| Supporting libraries | `canvas-confetti`, `lucide-react` |

## Engineering Highlights

### Resilient event operation

The team data path is designed for a real event environment. Public reads try the application API and can fall back to a direct Supabase read for active teams; if neither is available, the UI loads the local catalog and surfaces the current data source. Moderator mutations attempt remote persistence and retain local changes when the remote service cannot be reached.

### Reusable interaction architecture

`GameShell` standardizes navigation, briefing, fullscreen and reset behavior for every station. `TeamValidationPanel` connects each experience to the same team-selection and operator-validation flow. The shared `Modal` primitive handles Escape, backdrop close, initial focus, focus trapping and focus restoration, so rules, editing and winner flows do not need separate interaction implementations.

### Browser-native visual and audio interaction

The prize wheel is rendered directly with Canvas 2D and animated with `requestAnimationFrame`. Optional Web Audio ticks provide feedback during segment changes, and the audio path is isolated so unavailable audio support does not interrupt the draw. The station shell also uses the Fullscreen API for event displays.

### Testable game rules

The Hanoi move validator, render-order transformation, solved-state rule and team ID allocation are separated from React rendering. Typing and Bug Hunt catalogs are also data-driven, which makes language/difficulty matrices straightforward to inspect and test without a browser.

### Responsive operator interfaces

The global stylesheet includes dedicated responsive rules for the station launcher, game panels, validation form, team directory, moderator console and roulette. The same product can move from a wide operator/display layout to a compact single-column workflow without changing the underlying event flow.

## Testing

The repository uses Vitest through the real package script:

```bash
npm test
```

The current suite contains four focused test files:

- `tests/typing.test.ts` checks the JavaScript, Python and TypeScript catalog, the three-level ordering and progressive snippet sizes.
- `tests/bug-hunt.test.ts` checks the JavaScript/C++/TypeScript difficulty matrix, intentional bug counts and the difficult challenge definitions.
- `tests/hanoi.test.ts` checks initial state, render order, illegal moves, Tower 3 as the only winning destination and a complete traditional solution.
- `tests/team-id.test.ts` checks four-digit IDs, collision avoidance and deterministic fallback when random candidates are exhausted.

The tests focus on deterministic domain and catalog logic. There is no lint script configured in `package.json`; the repository's additional validation commands are Astro type checking and the production build.

## Running Locally

### Requirements

- Node.js `>=22.12.0`
- npm
- Docker only if you need the local Supabase stack

### Install, validate and run

```bash
npm ci
npx astro check
npm test
npm run build
npx astro dev --background
```

The development server uses Astro's default port `4321`. If it is occupied, use `npx astro dev --background --port 4322`.

### Environment variables

Copy `.env.example` to `.env.local` and configure only the variables required for the environment:

```bash
PUBLIC_SUPABASE_URL=
PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
MODERATOR_PIN=
MODERATOR_SESSION_SECRET=
```

The `PUBLIC_` values may be exposed to the browser. `SUPABASE_SERVICE_ROLE_KEY`, `MODERATOR_PIN` and `MODERATOR_SESSION_SECRET` are server-only secrets and must not be committed or embedded in client-side code.

The application can display a local team catalog and preserve browser state without a remote Supabase catalog. Server-authenticated operator login, official station validation and remote team mutations require the corresponding server configuration.

### Supabase workflow

The schema migration is stored at `supabase/migrations/20260909190000_create_teams.sql`. When working with Supabase, the repository provides these CLI commands:

```bash
npx supabase migration list
npx supabase db push
npx supabase start
```

`npx supabase start` requires Docker and is intended for local Supabase development.

## Project Context

CodeQuest UCB was built for an in-person technology activity organized within the Ingeniería de Sistemas UCB environment. The event brought together several programming-related stations and physical or design challenges, so the platform had to support more than one type of interaction while remaining understandable to operators and participants.

The implementation reflects that context: station views prioritize quick setup and feedback, team routes make progress visible, operator tools keep the roster and validations manageable, and the final route closes the activity with an eligibility-aware prize draw.

## Collaboration / Credits

This is a collaborative event product. The repository history credits Leonardo Camacho Quiroga with the initial project baseline and early documentation, while the subsequent architecture, service integration, responsive system, current challenge implementations, operator tooling, testing and follow-up fixes are represented by Sebastian Arce Antezana's commits and aliases.

The README intentionally separates **Platform Features** from **My Role & Contributions** so the platform can be presented at its full scope without attributing every original line of collaborative code to one person.
