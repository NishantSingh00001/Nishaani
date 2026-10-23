# Nishaani — Community Memory Workbench

Nishaani is a local-first prototype for the careful stewardship of oral-history collections. It focuses on the work around a recording: provenance, transcript revision, access labels, exhibition drafts, and portable backups.

## Run it

This is a static site. Open `index.html` directly or deploy the folder to Vercel as a static project. No build command, environment variables, paid service, or database is required. Three.js and GSAP are loaded from public CDNs for the 3D recorder and motion; if those scripts fail, the hero falls back to an inline illustration and the core pages remain usable.

## What is real in this demo

- All seven pages and their local interactions run in the browser.
- Records, transcript edits, rights labels, exhibitions, and activity are stored in this browser's `localStorage`.
- Selected audio can be previewed in the current tab with a waveform visualization. The audio binary is not uploaded and is not included in archive backups.
- Transcript indexing suggestions are local word-frequency suggestions, not AI transcription or analysis.
- Eight fictional Thane-area records demonstrate workflows. They are not historical testimony or verified community accounts.
- Rights labels are organizational metadata, not legal consent records. Exhibition drafts are previews and are never published.

## Limits before real community use

This is not a hosted full-stack service: there is no authentication, shared database, encrypted storage, multi-user access control, server-side AI, or automatic transcription. Browser storage is device-specific and is not a secure archival backup. Obtain informed permission, establish an actual governance process, and use secure storage before importing real testimony. The product is scoped as stewardship tooling rather than as another public archive; existing regional archive initiatives already document community histories.

## Files

- `index.html` — app shell and page navigation
- `styles.css` — editorial archive visual system
- `motion.css` — motion language, responsive readability and reduced-motion support
- `app.js` — local data, page rendering and interactions
- `FEATURES.md` — capability inventory and demo boundaries
