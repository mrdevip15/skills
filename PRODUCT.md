# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

delegated: static HTML/CSS/JavaScript in `showcase/index.html`, preserving the repository's file based showcase and manifest data

## Users

Game makers, artists, and developers who want to use ChatGPT to create a coherent set of RPG game assets, then inspect and reuse the exported files.

## Product Purpose

The showcase makes the asset generation workflow understandable and tangible. It lets visitors browse the generated Infinity Forest icon pack, inspect an individual icon, understand the deterministic pipeline, and copy a useful prompt pattern for their own asset requests.

## Positioning

The showcase connects a visual asset gallery to the production facts behind it: categories, target sizes, grid coordinates, transparency, nearest-neighbor scaling, and packaging are all shown together so the work can be reused with confidence.

## Operating Context

Visitors arrive at a static local page, scan the artifact set, filter by category or size, select an icon for inspection, and read the pipeline and validation report before adapting the workflow to a new prompt.

## Capabilities and Constraints

- The existing manifest data is the source of truth for icon counts, categories, sizes, outputs, and coordinates.
- The page must work from a file based static server and keep the local PNG asset paths intact.
- Search, category filtering, size filtering, icon selection, magnification, background switching, pipeline stage selection, and manifest/code tabs are existing supported interactions.
- The visual direction is a modern retro RPG website with a clear, useful catalog experience.

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
