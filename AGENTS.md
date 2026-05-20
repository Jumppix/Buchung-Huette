# AGENTS for Buchung-Huette

## What this repository is
- Single-page static frontend app in `index.html`.
- All CSS, HTML, and JavaScript live in one file.
- Uses CDN-hosted libraries: `fullcalendar` and `@supabase/supabase-js`.
- Provides internal family/club management features: booking calendar, news, members, documents, file uploads, contact form, and cookie consent.
- `index_old.html` and `index_v3.html` are legacy versions. The active source is `index.html`.

## Key behavior for agents
- Edit `index.html` directly. There is no build step, package manager, or separate source tree.
- Keep the inline structure intact: CSS in `<style>`, markup in `<body>`, and JS near the end of the file.
- Preserve the German UI text and existing styling conventions unless the task explicitly requires localization or redesign.
- Avoid introducing a new frontend framework or a complex build pipeline unless the user requests it and the repo grows beyond this single-file prototype.

## Important code areas
- Authentication / access gate: `checkPw()` and `#pw-gate`.
- Cookie consent: `#cookie-bar`, `openM('mo-cookie')`, `ckChoice()` and `#mo-cksettings`.
- Navigation: `go(sec, sub)` controls page sections and sub-navigation.
- Supabase data access: the inline configuration around `CFG.sbUrl` and `CFG.sbKey` and the Supabase initialization block.
- Dynamic content sections: news panels, booking/calendar views, members, uploads, editable content blocks, and contact/details modals.

## Special considerations
- This repo currently has no automated tests, CI, or build commands to follow.
- Supabase credentials are currently embedded in `index.html`. Treat this as a security-sensitive area and avoid exposing or changing credentials without clear need.
- If a feature touches data persistence, inspect the inline Supabase helper functions and the `CFG` object before modifying behavior.
- Legacy HTML files exist for reference, but do not update them unless the user specifically asks to preserve or compare earlier versions.

## When you need to document changes
- Prefer updating `README.md` with a short summary for user-visible features.
- Do not duplicate long implementation details from `index.html`; link to the file instead.

## Good first tasks for this codebase
- Fix visual or interactive bugs in the existing page flow.
- Improve accessibility and semantics within the current HTML structure.
- Clean up or refactor inline JavaScript to reduce duplication while leaving the single-file layout intact.
- Add comments near critical functions such as `checkPw()`, `go()`, and Supabase helpers.

## Not in scope unless requested
- Adding a backend repository or server-side code.
- Converting to a multi-file React/Vite/Next.js app without explicit user approval.
- Removing inline styling and scripts as part of routine maintenance.
