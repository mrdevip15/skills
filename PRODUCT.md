# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

delegated: static HTML/CSS/JavaScript in `showcase/index.html`, preserving the repository's file based showcase and manifest data

## Users

Game makers, artists, and developers who want to use ChatGPT to create a coherent set of RPG game assets, then inspect and reuse the exported files.

## Product Purpose

The showcase presents the Infinity Forest icon results. Visitors browse, filter, preview, and download individual icons. Prompts and generation instructions belong in SKILL.md.

## Positioning

The gallery shows real exported assets with their categories, file paths, sizes, and hero classes.

## Operating Context

Visitors open the static page, browse results, filter by category or size, and select an icon to preview or download. A link leads to SKILL.md for prompts and instructions.

## Capabilities and Constraints

- The existing manifest data is the source of truth for icon counts, categories, sizes, outputs, and coordinates.
- The page must work from a file based static server and keep the local PNG asset paths intact.
- Search, category filtering, size filtering, icon selection, enlarged preview, and individual PNG downloads are supported interactions.
- Show the results simply. Omit statistics panels, process steps, QC claims, and prompt blocks from the website.
- Keep prompts and instructions in SKILL.md.
- The theme is a quiet, simple gallery with a subtle RPG palette.

## Brand Commitments

The product is presented as a practical asset forge with a warm, crafted, game world tone. Copy should be specific and plain enough for creators to act on.

## Evidence on Hand

- 88 sliced PNG icons in `showcase/asset/icons/`.
- `showcase/manifest-data.js` and `showcase/icon_grid_manifest.json` with source and output metadata.
- `showcase/README.txt` describing the generated pack and folder structure.

## Product Principles

- Show the artifact first.
- Make production details easy to verify.
- Turn examples into reusable starting points.
- Keep exploration quick across categories and sizes.

## Accessibility & Inclusion

Use semantic controls, visible focus states, readable contrast, reduced motion support, and responsive layouts that keep the catalog usable on narrow screens.
