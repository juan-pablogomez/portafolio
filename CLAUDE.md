# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Personal portfolio site for Pablo Gómez, built with Astro + Tailwind CSS. Single-page site (`src/pages/index.astro`) composed of sections: hero, work experience, projects, about me, and tech stack.

## Commands

```sh
npm run dev       # Start dev server at localhost:4321
npm run build     # Type-check (astro check) then build to ./dist/
npm run preview   # Preview the production build locally
npm run astro ...  # Run Astro CLI commands (e.g. astro add, astro check)
```

There is no test suite or linter configured. `astro check` (run as part of `npm run build`) is the only correctness check — run it after making changes to `.astro` files.

## Architecture

- **Single page, section-based composition**: `src/pages/index.astro` is the only route. It imports and lays out section components in order inside `<SectionContainer>` wrappers: `Experience`, `Projects`, `Aboutme`, `Tecnologies`, `Footer`.
- **Content lives inline in components as data arrays**, not in a CMS or content collection:
  - `src/components/Experience.astro` — `EXPERIENCE` array (jobs, dates, tasks), rendered via `ExperiencieItem.astro`.
  - `src/components/Projects.astro` — `PROJECTS` array (title, description, link, image, tags) plus a `TAGS` dictionary mapping tag keys to display name/color class/icon component. Project images live in `public/images/`.
  - `src/components/Tecnologies.astro` — `tech` array of `{ name, icon }` for the stack grid.
  - To add/edit a job, project, or tech entry, edit the relevant array directly in these files — there is no separate data layer.
- **Icons are individual `.astro` components**, not an icon library. General icons live in `src/components/icons/`; technology/brand icons live in `src/components/icons/TechIcons/`. Adding a new tech badge means adding a new icon component there and referencing it from `Tecnologies.astro` and/or the `TAGS` map in `Projects.astro`.
- **Layout/theme**: `src/layouts/Layout.astro` wraps every page (`data-theme="dark"` is hardcoded on `<html>`; Tailwind's `darkMode: 'class'` is configured but the site does not currently expose a theme toggle). `Header.astro` is rendered globally inside the layout.
- **Styling**: Tailwind CSS only (no CSS modules/styled-components). Utility classes are used directly in markup; component-scoped `<style>` blocks are used sparingly (e.g. `Tecnologies.astro`).
