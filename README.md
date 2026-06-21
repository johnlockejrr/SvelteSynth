# SvelteSynth

Synthetic manuscript page generator for OCR/HTR training data. Design layouts in a form editor, render PNG pages at configurable DPI, and export matching PAGE-XML ground truth compatible with the [oxygraphos](https://github.com/) / `infer_to_xml` pipeline.

## What it produces

Each run outputs:

1. **PNG** — rasterized page image
2. **PAGE-XML** — region/line polygons + Unicode text (PAGE 2019-07-15 format)

## Visual demo

<img width="3834" height="2027" alt="image" src="https://github.com/user-attachments/assets/5a84cb46-9128-4e91-b909-7d9da937f0d9" />

## Quick start

See **[QUICKSTART.md](QUICKSTART.md)** for install, editor usage, CLI, and batch generation.

```bash
npm install
npx playwright install chromium
npm run dev          # editor at http://localhost:5173/editor
npm run build
npm run render -- templates/single-column.json --dpi 300 \
  --out-image outputs/images/page.png --out-xml outputs/pagexml/page.xml
```

## Architecture

- **SvelteKit** — form editor + single canonical `/render` route
- **Playwright** — headless Chromium capture at exact DPI
- **Zod** — layout schema shared by editor, renderer, and CLI

One render path serves the editor iframe preview and CLI/batch scripts — no duplicate renderers.

## Project layout

| Path | Purpose |
|------|---------|
| `src/routes/editor/` | Form-based layout editor |
| `src/routes/render/` | Canonical page renderer |
| `src/lib/schema.ts` | Layout JSON schema |
| `src/lib/groundtruth/` | Line extraction + PAGE-XML |
| `scripts/render-page.ts` | Single-page CLI |
| `scripts/generate-batch.ts` | Batch generation |
| `templates/` | Saved layout JSON |
| `static/fonts/` | Self-hosted fonts |
| `static/backgrounds/` | Background images |
| `corpus/` | Text files for batch (user-supplied) |
| `infer_to_xml/` | Reference PAGE-XML format |

## Documentation

- [QUICKSTART.md](QUICKSTART.md) — detailed usage
- [AGENTS.md](AGENTS.md) — full project specification
- [docs/superpowers/specs/2025-06-21-sveltesynth-design.md](docs/superpowers/specs/2025-06-21-sveltesynth-design.md) — design spec

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Development server |
| `npm run build` | Production build |
| `npm run preview` | Preview production build (port 4173) |
| `npm test` | Unit tests |
| `npm run test:e2e` | Playwright e2e tests |
| `npm run render` | Render one schema to PNG + XML |
| `npm run batch` | Batch-generate pages |

## License

Font files under `static/fonts/` are bundled under their respective open licenses (OFL). Background images are user-supplied assets.
