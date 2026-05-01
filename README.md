# Astro 6 hybrid build — dynamic-import() chunk hash mismatch repro

Minimal reproduction for: **Astro 6 hybrid build emits dynamic-import URLs with chunk hashes that don't match emitted chunks (runtime 404 on `/_astro/*.js`)**.

## What this demonstrates

- `output: 'static'` + `adapter: vercel()`
- A single page with `export const prerender = false` (`src/pages/preview.astro`) silently flips the build into hybrid mode (the build log will say `mode: "server"` even though `output` is `"static"`)
- A prerendered page (`src/pages/index.astro`) mounts `<Sections client:load />`. Mounting with `client:load` (not `client:only`) is important: it forces the Server, Prerender, and Client Vite passes to all process the same component graph
- `Sections.vue` renders a `Section.vue` dispatcher per item. `Section.vue` uses a template-literal dynamic import (`import(\`../sections/${component}.vue\`)`) so Rollup expands it to a glob and emits a chunk per matching file
- Each section component statically imports `glightbox/dist/css/glightbox.min.css` at the top, then uses `await import('glightbox')` at runtime — both shapes appear in the chunk graph
- `MediaGalleryGrid.vue` adds a static top-level `import Hls from 'hls.js'` to widen the dependency graph
- When deployed to Vercel, runtime `import()` URLs baked into the parent chunk bodies reference content hashes that the Client pass never emitted, producing 404s on `/_astro/<chunk>.<wrongHash>.js`

## Versions

| Package | Version |
|---|---|
| astro | 6.1.10 |
| @astrojs/vercel | 10.0.6 |
| @astrojs/vue | 6.0.1 |
| vue | 3.5.33 |
| glightbox | 3.3.1 |
| hls.js | 1.5.20 |
| Node | 22.x |

## Steps to reproduce

1. Install and build:

   ```bash
   npm install
   VERCEL=1 npx astro build
   ```

   The build log will print `mode: "server"` even though `astro.config.mjs` declares `output: 'static'` — that's the silent flip into hybrid mode caused by `src/pages/preview.astro`'s `export const prerender = false`.

2. Deploy to Vercel:

   ```bash
   npx vercel deploy --prebuilt
   ```

3. Open the deployed `/` page in a browser, open DevTools → Network. The dynamically imported chunks (one per section component, plus glightbox) will return 404 on URLs like `/_astro/glightbox.min.<hash>.js?dpl=...` — the hash in the request URL does not match any file emitted on disk.

## Verifying the build artifacts (what to look for)

The hash mismatch is most visible in the deployed build because it appears to depend on the Vercel build environment (Linux + parallel Vite passes). On a local macOS build, the multi-pass hashes happen to converge and the artifacts look correct on disk:

```bash
ls .vercel/output/static/_astro/ | grep -E 'glightbox|BioCardGrid|Itinerary|MediaGalleryGrid'
# glightbox.min.CSueQNvw.js
# BioCardGrid.sG7YzeDA.js
# Itinerary.D4mcpWvm.js
# MediaGalleryGrid.CN7SAn08.js

grep -hoE '/_astro/(glightbox\.min|BioCardGrid|Itinerary|MediaGalleryGrid)\.[A-Za-z0-9_-]+\.js' .vercel/output/static/_astro/*.js | sort -u
```

After deploying to Vercel, the deployed `/_astro/*.js` parent chunks will contain `import()` URLs with **different** hashes than the on-disk filenames. The same comparison performed against the deployed assets shows the mismatch.

## Why this matters

The presence of any single `prerender = false` page is enough to flip an `output: 'static'` Astro 6 project into hybrid mode internally. Any project using Vue/React component code-splitting or any third-party library loaded via `import()` is exposed to runtime 404s in production.

The same project on Astro 5 / `@astrojs/vercel` v9 (which used the `/static` and `/serverless` subpath adapters that kept build environments fully separate) does not exhibit this bug.

## Related prior art

- withastro/astro#16209 — chunk-hash placeholder regression in 6.1.3 (Linux-specific)
- withastro/astro#16048 — hybrid-environment manifest invalidation fix
