# Project Structure

## Top-level Layout

```
/
├── src/                    # All application source code
├── types/                  # Shared TypeScript/Zod schemas (used by both Next.js and Remotion)
├── styles/                 # Global CSS (Tailwind v4 config lives here)
├── config.mjs              # AWS Lambda configuration constants
├── deploy.mjs              # Lambda deploy script (function + S3 site)
├── remotion.config.ts      # Remotion CLI configuration (Rspack, frame format, bundler override)
├── next.config.js          # Next.js configuration
├── postcss.config.mjs      # PostCSS with @tailwindcss/postcss
└── tsconfig.json           # TypeScript config
```

## `src/` Structure

```
src/
├── app/                    # Next.js App Router
│   ├── layout.tsx          # Root HTML layout, imports global.css
│   ├── page.tsx            # Home page: @remotion/player preview + RenderControls
│   └── api/lambda/
│       ├── render/route.ts    # POST — triggers renderMediaOnLambda
│       └── progress/route.ts  # POST — polls getRenderProgress
├── components/             # Next.js UI components (not Remotion compositions)
│   ├── RenderControls.tsx  # Title input + render button + progress/download UI
│   ├── ProgressBar.tsx
│   ├── DownloadButton.tsx
│   └── ...                 # Button, Input, Spinner, Tips, Spacing, etc.
├── helpers/
│   ├── use-rendering.ts    # State machine hook: init → invoking → rendering → done/error
│   └── api-response.ts     # executeApi() — typed Next.js route handler factory with Zod parsing
├── lambda/
│   └── api.ts              # Client-side fetch wrappers: renderVideo(), getProgress()
├── lib/
│   └── utils.ts            # cn() utility (clsx + tailwind-merge)
└── remotion/               # All Remotion video code (isolated from Next.js UI)
    ├── index.ts            # Entry point: registerRoot(RemotionRoot), imports global.css
    ├── Root.tsx            # Registers all <Composition> elements
    ├── webpack-override.mjs  # Enables Tailwind v4 in Remotion's Rspack bundler
    └── MyComp/             # The "MyComp" composition
        ├── Main.tsx        # Root composition component, orchestrates Sequences
        ├── NextLogo.tsx    # SVG Next.js logo with animated path drawing
        ├── Rings.tsx       # Radial ring burst transition
        └── TextFade.tsx    # Diagonal mask-sweep text reveal
```

## `types/` — Shared Source of Truth

```
types/
├── constants.ts    # COMP_NAME, CompositionProps (Zod), VIDEO_WIDTH/HEIGHT/FPS, DURATION_IN_FRAMES
└── schema.ts       # RenderRequest, ProgressRequest, ProgressResponse (Zod schemas for API routes)
```

These types are imported by both the Next.js app and the Remotion compositions — never duplicate them.

## Key Architectural Patterns

- **Compositions registered in `Root.tsx`** — add new `<Composition>` elements there; constants go in `types/constants.ts`
- **Zod for all prop types** — composition props and API request/response bodies are Zod schemas; use `z.infer<>` for TypeScript types
- **Remotion code is isolated under `src/remotion/`** — keep video/animation logic separate from the Next.js UI
- **`executeApi()` wraps all API routes** — provides typed request parsing and consistent error handling
- **`cn()` for all conditional Tailwind classes** — import from `src/lib/utils.ts`
- **Design tokens in CSS only** — extend the `@theme {}` block in `styles/global.css`; do not create a `tailwind.config.*` file
