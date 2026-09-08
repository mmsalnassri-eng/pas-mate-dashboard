# CLAUDE.md — pas-mate (OMSB Program Accreditation Dashboard)

## What this is
A single-file HTML tool tracking OMSB program accreditation applications
for a 4-person team (Malik — owner, Amna, Dina, Dr. Ammar). Deployed at
pasmate.pages.dev via Cloudflare Pages. This is real, in-use operational
software — live applications, named staff, no invented data.

## Working with this codebase — read before editing
- This is intentionally a SINGLE HTML file (`index.html`) with styles,
  logic, and bundled libraries (jsPDF) all inline. Do not split it into
  multiple files, add a build step, or introduce a bundler/framework
  unless explicitly asked — the single-file approach is a deliberate
  choice the owner has made more than once, not a shortcut to fix.
- No package.json, no npm install, nothing to build. Editing the file
  IS the deployment artifact.
- Verify changes by opening `index.html` directly in a browser — no dev
  server exists or is needed.
- The Firebase config embedded in the file (apiKey, database URL, etc.)
  is not a secret — it's a public client identifier. Security is
  enforced entirely by the Realtime Database rule below, not by hiding
  this config. Never suggest removing/obscuring it as a "fix."

## Backend
- Firebase Realtime Database, project "pas-mate", region europe-west1.
- Current security rule — replicate this exactly if this logic ever
  moves (e.g. a Supabase migration); do not loosen it:
  ```json
  { "rules": {
      ".read": "auth.uid === 'iHnvbP7VYhbaZ9oE8jwJ3YY6KC32'",
      ".write": "auth.uid === 'iHnvbP7VYhbaZ9oE8jwJ3YY6KC32'"
  } }
  ```
- Auth: Firebase Authentication (Email/Password), one shared team login.
- A self-hosted Supabase migration is planned but not started — ask
  before beginning any part of that rewrite; it's a multi-step project
  that should land as a series of reviewable commits, not one rewrite.

## The operational pathway — do not add stages without being asked
Simplified 10-stage checklist, chosen deliberately over the more granular
16-step official process:
Initial review → Initial decision → Correction plan → To re-reviewers →
Compliance report → PA-O1/O2 out → PA-A2 in → PA-R3 out → PA-R3 in →
Final decision.
Extend it (counters, workload views, etc.) — don't replace it with more
granularity.

## Git conventions
- This is a small internal tool with a small, non-technical user base
  relying on it daily — prefer a feature branch + review over committing
  straight to main for anything beyond a trivial fix.
- Write commit messages that explain *why*, not just what changed.
- Conventional prefixes where they fit: feat:, fix:, refactor:, docs:.
