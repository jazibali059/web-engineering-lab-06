# Web Engineering Lab — Lab 6 (Tasks 1–12)

Requires **Node.js LTS** and **VS Code**. Open this folder (`web-engineering-lab`) in VS Code
(File → Open Folder), then use the integrated terminal (Ctrl+`). All commands run from this folder unless stated.

## Task map
| Task | Where |
|------|-------|
| 1 | `demo/page.mjs`, `demo/build-static.mjs` → creates `demo/dist/ssg.html` |
| 2 | `demo/server.mjs` |
| 3, 4, 5 | `RENDERING-NOTES.md` |
| 6 | `app/` (Vite + React + TS scaffold, boilerplate removed, `src/styles.css`) |
| 7 | `app/src/types.ts`, `Greeting.tsx`, `StatusMessage.tsx` |
| 8 | `app/src/SlotRow.tsx` |
| 9 | `app/src/SlotTable.tsx` |
| 10 | `app/src/App.tsx` |
| 11 | build/preview commands below + notes in `RENDERING-NOTES.md` |
| 12 | `AI-USAGE.md` (and git commands below) |

## Part A — rendering demo (Tasks 1–5) — no install needed
```
node demo/build-static.mjs     # Task 1: writes demo/dist/ssg.html
node demo/server.mjs           # Task 2: http://localhost:3000   (Ctrl+C to stop)
```
Open /csr, /ssr, /ssg, /isr, /stream. For Task 3/4 use Ctrl+U (View Source), DevTools → Ctrl+Shift+P →
"Disable JavaScript", and the Network panel on /stream. Fill in the browser results in `RENDERING-NOTES.md`.
(Run the commands from the project root: `build-static.mjs` writes to the relative path `demo/dist`.)

## Part B — React app (Tasks 6–11)
```
cd app
npm install
npm run dev                    # http://localhost:5173 — click Select/Cancel
npm run build                  # Task 11: must finish with no TypeScript errors
npm run preview                # http://localhost:4173 — View Source shows an empty <div id="root">
```
Lighthouse: Chrome DevTools → Lighthouse → Accessibility only → run on the preview; expected score 100.
Save a screenshot as evidence.

## Task 12 — Git
```
git init                       # if not already a repo (or copy these folders into your web-engineering-lab repo)
git add demo app RENDERING-NOTES.md AI-USAGE.md README.md .gitignore public
git commit -m "Lab 6: rendering demo and React office hours app"
git remote add origin <your GitHub URL>
git push -u origin main
```
Edit `AI-USAGE.md` to honestly reflect your own use before committing.

## Notes
- `public/styles.css` is the stylesheet copied to `app/src/styles.css` (Step 1 of Part B). Replace it with your own Lab 5
  stylesheet if you wish.
- `node_modules` is not included; `npm install` restores it from `package-lock.json`.
