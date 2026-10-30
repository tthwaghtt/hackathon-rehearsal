# Product

This is a **Remotion + Next.js video generator** — a web app where users preview an animated video in the browser, customize a title, and trigger a cloud render via AWS Lambda that produces a downloadable H.264 MP4.

## What it produces

An animated branded intro video: the Next.js logo draws itself via SVG path animation, a ring-burst transition plays, then the user-supplied title fades in with a diagonal mask sweep.

- **Output:** 1280×720 @ 30 fps, 200 frames (~6.67 seconds), H.264 MP4
- **Default title:** "Next.js and Remotion"

## Core user flow

1. User edits the title in the browser via `@remotion/player` preview
2. Clicks "Render video" → POST to `/api/lambda/render`
3. Progress is polled via `/api/lambda/progress`
4. Completed render is downloaded from S3
