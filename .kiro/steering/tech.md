# Tech Stack

## Core Frameworks & Runtime

| Package | Version |
|---|---|
| `next` | 16.4.3 |
| `react` / `react-dom` | 19.2.3 |
| `remotion` | 4.0.539 |
| `typescript` | 5.9.3 |
| `tailwindcss` | 4.2.0 |
| `zod` | 4.5.4 |

## Remotion Packages (all 4.0.539)

- `@remotion/cli` — studio and local render CLI
- `@remotion/player` — in-browser composition preview
- `@remotion/lambda` — AWS Lambda render pipeline
- `@remotion/bundler` — programmatic bundling
- `@remotion/google-fonts` — font loading inside compositions
- `@remotion/paths` — SVG path animation utilities (`evolvePath`)
- `@remotion/shapes` — shape primitives
- `@remotion/tailwind-v4` — Tailwind v4 support inside Remotion's bundler

## UI Utilities

- `clsx` 2.1.1 + `tailwind-merge` 3.0.1 — combined as `cn()` in `src/lib/utils.ts`

## Bundler

- Remotion uses **Rspack** (set via `Config.setRspack(true)` in `remotion.config.ts`)
- Tailwind v4 is injected into the Remotion bundler via `src/remotion/webpack-override.mjs`
- Next.js uses its own built-in compiler

## Tailwind v4

- **No `tailwind.config.*` file** — configured entirely in CSS
- `styles/global.css` uses `@import "tailwindcss"` and an `@theme {}` block to define design tokens
- Custom tokens (colors, spacing, radius, animations) are CSS variables exposed through `@theme`
- PostCSS plugin: `@tailwindcss/postcss`

## TypeScript

- `strict: true`, `noUnusedLocals: true`, `isolatedModules: true`
- `moduleResolution: "Bundler"`, `module: "esnext"`, `jsx: "react-jsx"`
- All composition props use Zod schemas; component prop types are derived via `z.infer<typeof Schema>`

## AWS Lambda

- Config in `config.mjs`: region `us-east-1`, RAM 3009 MB, disk 10240 MB, timeout 240s
- Credentials via env vars: `REMOTION_AWS_ACCESS_KEY_ID` / `REMOTION_AWS_SECRET_ACCESS_KEY`
- Deploy script: `deploy.mjs`

## Common Commands

```bash
npm run dev        # Next.js dev server (localhost:3000)
npm run build      # Production Next.js build
npm run start      # Serve production build
npm run lint       # ESLint across the whole project
npm run remotion   # Open Remotion Studio (composition preview)
npm run render     # Local CLI render (not Lambda)
npm run deploy     # Deploy Lambda function + S3 site to AWS
```
