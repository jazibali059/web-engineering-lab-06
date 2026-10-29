# AI-USAGE.md

## Lab 6
- **Generated:** An AI assistant (Claude) produced the lab deliverables: the demo/ files, the Vite React app
  components (types.ts, Greeting, StatusMessage, SlotRow, SlotTable, App), a stylesheet, the notes and README.
  The component and server code follows the lab statement's listings; I did not invent a different design.
- **Reviewed / tested:** `npm run build` (no TypeScript errors); the demo routes were checked with `curl`
  (source content, refresh behaviour, ISR 5 s window, stream TTFB vs total); booking logic was tested in jsdom.
- **Changed from the statement:** `demo/server.mjs` resolves `dist/ssg.html` relative to its own file so it
  works from any folder; `styles.css` was rewritten because my Lab 5 stylesheet was not available to the assistant.
- **Rejected:** Storing the booked count in state — it is derived from `booked.length`.
- **Not done by AI:** Chrome-only evidence (JS-disabled test, Network panel, Lighthouse score/screenshot) must be
  collected by me. _(Student: edit this file to describe your own review honestly before submitting.)_
