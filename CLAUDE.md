# em-case

A lightweight Engineering Manager case study for **XYZ**. The deliverable is a short slide deck built with [Slidev](https://sli.dev/) — Markdown-driven presentation slides.

The full assignment brief is in `docs/case.md`. The deck must cover: Product/Project Overview, Tech Stack & Architecture, Key changes introduced, and Team Setup — within ~5 slides, and lean on AI tooling (which is itself part of the assessment).

## Layout

- `slides.md` — the deck entry point (Slidev convention). This is the main artifact.
- `docs/case.md` — the original assignment brief (source of requirements).
- `.claude/skills/grill-me/` — local skill to stress-test plans/designs.

## Working with Slidev

- Dev server (live preview at http://localhost:3030): `npx slidev` (or `npm run dev`)
- Export to PDF: `npx slidev export` → `slides-export.pdf`
- Build static SPA: `npx slidev build` → `dist/`
- Slides are separated by `---` on its own line. The first block is _headmatter_ (theme, title, etc.); each subsequent slide can open with YAML _frontmatter_ (e.g. `layout: two-cols`).
- Presenter/voiceover notes go in an HTML comment at the end of a slide: `<!-- notes here -->`. Use these for the spoken track, since the voiceover is graded.
- Useful built-in layouts: `cover`, `center`, `section`, `two-cols`, `image-right`, `quote`.

## Conventions

- Keep the deck to **max 5 content slides** — clarity and substance over polish (per the brief).
- Edit `slides.md` directly; don't hand-author files under `dist/`.
- This is a personal case repo — no CI, no tests.
