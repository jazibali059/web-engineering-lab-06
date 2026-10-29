# RENDERING-NOTES.md — Lab 6

> **Verification note:** Server-side results (View Source content, changes on refresh, TTFB/total time) were
> measured with `curl`, which shows exactly what View Source shows. The "JavaScript off" column follows directly
> from that HTML. **Please repeat Steps 5, 6 and 11 once in Chrome and replace the numbers / add your Lighthouse
> screenshot** — your instructor may ask to see the browser evidence.

## Part A — Task 3: four page routes

| Route | Does the source contain the time? | Does it change on refresh? | Works with JavaScript off? |
|-------|-----------------------------------|----------------------------|-----------------------------|
| /csr  | No — source only has "Loading…" plus a `<script>` | Yes, in the browser (script runs on every load), but the source never changes | **No** — stuck on "Loading…" |
| /ssr  | Yes — "Rendered on the server at 5:00:14 PM" | Yes — new time on every request (14 s → 16 s) | **Yes** |
| /ssg  | Yes — "Built at 5:00:11 PM" | **No** — same time until `build-static.mjs` is rerun | **Yes** |
| /isr  | Yes — "Regenerated at 5:00:18 PM" | Only after more than 5 s: refreshes inside 5 s show the same time; after waiting, 5:00:26 PM | **Yes** |

## Task 4: /stream

- Time to first byte: **≈ 0.001 s** (curl); total time: **≈ 1.503 s**.
- In the Network panel the "Waiting for server response" part is tiny while "Content Download" takes ~1.5 s.
- Streaming helps when a page has a fast shell (header, layout) but some slow data: the user sees and can read
  the shell immediately instead of staring at a blank page until everything is ready.

## Task 5: choosing a strategy

| Page | Strategy | Justification (freshness, speed, cost) |
|------|----------|----------------------------------------|
| Blog post | **SSG** (or ISR if posts are edited often) | Content changes rarely, so build the HTML once. Fastest (a static file from a CDN), cheapest (no per-visit server work), and great for search. Staleness until rebuild is acceptable. |
| Live stock-price dashboard | **CSR** (static shell + JavaScript fetching/streaming live data, e.g. WebSocket/polling) | Prices change every second, so any HTML built on the server is stale immediately. The page is interactive and not mainly for search. Cost: blank until JS runs, mitigated by a static shell/skeleton or streaming. |
| User's account page | **SSR** (CSR is also fine since it is behind login) | Personalised and must be fresh per user, so it cannot be one shared static file. SSR renders the user's data per request; cost is server work on every visit. Not publicly indexed, so SEO is irrelevant. |

## Part B — Task 11: CSR in the React app

- After `npm run build` / `npm run preview`, View Source of the preview shows only `<div id="root"></div>` plus
  `<script type="module" src="/assets/index-….js">` — no heading, table or status text.
- This is **client-side rendering**: the server sends an empty shell, and the browser downloads the JS bundle,
  runs React, and React builds the DOM. With JavaScript disabled the page is blank (like `/csr` in Part A).
- Because the HTML is not produced on the server, nothing is interactive-by-hydration here; React *creates*
  the DOM rather than hydrating existing HTML. SSR/SSG (next week's Next.js) would send finished HTML and then hydrate it.
- Hydration has a cost: the page looks ready before JS finishes downloading and attaching handlers, and
  more JavaScript means a longer wait. Islands reduce this by hydrating only the interactive parts.
- `npm run build` finishes with no TypeScript errors (`tsc -b && vite build`). Booking was tested programmatically
  (select, cancel, count 0→1→2→1, row class `selected`, label Select↔Cancel). Buttons are native `<button>`
  elements, so Tab + Enter/Space work.
- **Lighthouse (Accessibility) — TO DO in Chrome:** run it on `http://localhost:4173` and paste the screenshot
  here as `lighthouse.png`. The markup follows the lab's accessible design (skip link, landmarks, `th scope`,
  button `aria-label`s, `lang`, high-contrast CSS).
