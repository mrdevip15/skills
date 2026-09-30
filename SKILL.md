---
name: pixel-icon-production-pipeline
description: Generate consistent game icons, build deterministic sprite atlases, create coordinate manifests, automatically slice and resize assets, validate them, and package production-ready PNG files into the requested project structure.
---

# Pixel Game Icon Generator, Atlas Builder & Auto Slicer

## 1. Skill Purpose

This skill is designed to systematically generate a coherent set of game icons from user specifications, then process them into production-ready assets directly integrable into a game project.

The skill manages the entire pipeline:

**Icon Specification → Visual Generation → Style Consistency → Grid Atlas → Coordinate JSON Mapping → Programmatic Slicing → Resizing → Multi-Point Validation → Folder Structuring → ZIP Packaging**

Ideal use cases include:

- RPG skill and ability icons
- Gameplay resource gauges (HP, Mana, Stamina, Energy)
- Status effect and debuff icons
- Upgrade, stat, and talent tree icons
- Achievement and title badges
- Class, faction, and evolution path crests
- Difficulty selection medallions
- Inventory and equipment icons
- Item and consumable sprites
- Buff and aura indicators
- Any modular UI asset collections structured as icon sets

---

## 2. Expected Deliverables

The skill must produce:

1. Individual PNG files for every requested icon.
2. True transparent alpha backgrounds (RGBA).
3. Final dimensions strictly conforming to specifications.
4. Exact filenames matching the user's inventory.
5. Directory structures segregated by category.
6. Sprite sheets / master icon atlases when requested or generated during the pipeline.
7. Coordinate JSON manifests mapping atlas coordinates to individual files.
8. A consolidated ZIP archive containing the complete asset suite.
9. Programmatic validation of total icon count.
10. Programmatic validation of image dimensions.
11. Programmatic validation of RGBA format and transparency.

Example output hierarchy:

```text
asset/
└── icons/
    ├── skills/
    │   ├── slash.png
    │   ├── charge.png
    │   └── ...
    │
    ├── resources/
    │   ├── mana.png
    │   └── ...
    │
    ├── status/
    ├── boons/
    ├── stats/
    ├── paths/
    ├── titles/
    └── difficulty/
```

Accompanied by:

```text
icon_grid_manifest.json
README.txt
icons.zip
```

---

## 3. Minimum Required Inputs

To execute the skill effectively, the user should provide at least:

### A. Icon Inventory List

Example:

```text
slash.png
charge.png
thrust.png
quake.png
```

Preserving exact filenames is critical if assets are bound directly to game engine source code.

Never alter filenames without explicit user confirmation.

---

### B. Icon Descriptions

Example:

```text
slash.png
a steel sword releasing a glowing golden crescent slash

charge.png
a steel shoulder pauldron rushing forward with impact lines
```

The more detailed and distinct the visual description, the more consistent and distinguishable the output will be.

---

### C. Final Target Dimensions

Example:

```text
Skill icons      : 32×32 px
Resource icons   : 32×32 px
Path emblems     : 48×48 px
Title icons      : 48×48 px
```

If multiple sizes are requested, partition them into separate atlases based on resolution class.

Never mix 32×32 and 48×48 icons into a single slicing grid unless explicitly requested by the user.

---

### D. Visual Style Specification

Example:

```text
Pixel art RPG
SNES / 16-bit aesthetic
1 px crisp dark outline
10-color restricted palette
Top-left directional lighting
No anti-aliasing
Moody dark fantasy atmosphere
```

This specification serves as the project's **Master Style**.

---

## 4. Optional Inputs

The user may also supply:

### Custom Palette

Example:

```text
Mana      #7fa8ff
Stamina   #a6e07a
Resolve   #ffd06a
Focus     #ff9f6a
```

### Restricted Colors (Colors to Avoid)

Example:

```text
Avoid using #f07b4f as a dominant color because it is reserved for enemy telegraphed attacks.
```

### Reference Game Sprites / Aesthetics

For example:

```text
Tiny RPG
Final Fantasy SNES
Secret of Mana
Chrono Trigger
```

### Target Output Directories

Example:

```text
asset/icons/skills/
```

### Game Source Code Mappings

For example:

```text
progression.js
skills.js
items.json
```

This context guarantees that filenames remain 100% compatible with existing engine dictionaries.

---

## 5. Stage 1 — Specification Analysis

Before generating any images, the skill must compute and assert:

- Total icon count
- Number of functional categories
- Dimension specifications per category
- Duplicate filename detection across categories
- Output folder destinations
- Icons per generation batch
- Identification of potential visual ambiguities or overlapping concepts

Example analysis:

```text
Total icons: 88

32×32 Resolution:
Skills         36
Resources       4
Status          4
Boons           6
Stats          12
Subtotal:      62

48×48 Resolution:
Paths          12
Titles         10
Difficulty      4
Subtotal:      26
```

Total verification:

```text
62 + 26 = 88 icons
```

---

## 6. Stage 2 — Duplicate Filename Detection & Namespacing

Identical filenames are permissible provided they reside in different category folders.

Example:

```text
skills/guard.png
boons/guard.png
```

or:

```text
resources/focus.png
boons/focus.png
```

Never rely on the bare filename alone as the unique identifier.

Always use:

```text
category + filename
```

or full output relative paths:

```text
asset/icons/boons/focus.png
```

---

## 7. Stage 3 — Master Style Creation

Before synthesizing the entire icon suite, lock in a unified foundation style.

Master Prompt template:

```text
pixel art RPG game icon,
crisp hard-edged pixels,
no anti-aliasing,
limited palette,
1-pixel dark outline,
single centered symbol,
top-left lighting,
three shading steps,
moody moonlit dark-fantasy,
16-bit SNES RPG style,
no text
```

Negative Prompt:

```text
blurry,
soft gradients,
anti-aliasing,
3D render,
photorealistic,
painterly,
letters,
numbers,
watermark,
signature,
clutter,
multiple unrelated objects,
jpeg artifacts
```

---

## 8. Stage 4 — Benchmark Generation

Never attempt to generate dozens of icons simultaneously on the first pass.

First, produce a calibration benchmark set.

Ideal benchmark batch size:

```text
6–12 icons
```

Select icons spanning diverse visual properties:

```text
slash   (metallic weapon / streak effect)
guard   (armor / shield silhouette)
storm   (elemental lightning / magic)
burst   (radiant energy / explosion)
heal    (holy light / organic iconography)
mana    (crystalline / liquid gemstone)
```

Evaluate benchmark results against:

- Outline thickness and contrast
- Color saturation and palette harmony
- Silhouette clarity and contrast against dark backgrounds
- Visual readability at small scales
- Simplicity (preventing over-detailed noise that deteriorates when scaled down)
- Distinct readability at target sizes (e.g., 32×32)

---

## 9. Stage 5 — Category-Based Batching

For optimal visual consistency, icons should be synthesized in coherent batches.

Example: Soldier hero skill set:

```text
Soldier:
slash
charge
thrust
quake
dive
whirl
```

Structure into:

```text
3 × 2 grid
```

or:

```text
6 × 1 strip
```

Batching by character class or functional category ensures unified color temperature and lighting across related abilities.

---

## 10. Critical Rules for Sprite Sheets

When output will be programmatically sliced, the layout grid **MUST** be deterministic.

Every cell must conform to:

- Identical cell dimensions
- Mathematical grid coordinates
- Zero overlap between adjacent cells
- Generous internal padding/margins
- Zero elements crossing or touching cell boundaries

Example grid layout:

```text
Atlas width   : 1120 px
Columns       : 8
Cell width    : 140 px

Atlas height  : 1400 px
Rows          : 10
Cell height   : 140 px
```

Mathematical positioning:

```text
cell x = column × cell_width
cell y = row × cell_height
```

Example coordinates:

```text
row 0, col 0  →  x = 0,   y = 0
row 0, col 1  →  x = 140, y = 0
row 1, col 0  →  x = 0,   y = 140
```

---

## 11. Prompting for Sliceable Atlases

To enforce a sliceable grid layout in generative vision models, include explicit spatial rules:

```text
icons arranged on a perfectly uniform grid,
equal cell dimensions,
equal spacing,
each icon fully contained inside its own cell,
no icon crossing cell boundaries,
no overlapping glow,
consistent scale,
centered inside each cell,
large empty padding around every icon
```

This constraint is vital because generative image models frequently generate visually aligned layouts that lack true mathematical uniformity unless strictly prompted.

---

## 12. Stage 6 — Generate Atlas

Synthesize sprite sheets partitioned by category or resolution tier.

Example:

```text
Sheet 1: Soldier skills
Sheet 2: Knight skills
Sheet 3: Archer skills
```

Or, if the model reliably maintains grid uniformity across larger sheets:

```text
Skills Atlas:
36 icons arranged in a 6 columns × 6 rows grid
```

Smaller batches (16 to 48 icons) are recommended for tighter visual consistency and fewer grid drift artifacts.

---

## 13. Stage 7 — Determine Actual Grid Bounds & Margins

After synthesizing the atlas, inspect its actual dimensions:

```text
actual image width
actual image height
```

Example:

```text
1122 × 1402 px
```

If the intended grid configuration is:

```text
8 columns
10 rows
```

and the usable grid region is:

```text
1120 × 1400 px
```

Then:

```text
cell width  = 1120 / 8  = 140 px
cell height = 1400 / 10 = 140 px
```

Record unused border pixels in the manifest:

```json
{
  "unused_right_px": 2,
  "unused_bottom_px": 2
}
```

Never distort or stretch the grid cells to accommodate residual edge pixels.

---

## 14. Stage 8 — Generate Coordinate Manifest JSON

Every icon must have an explicit coordinate record in the manifest before slicing.

Example entry:

```json
{
  "index": 0,
  "row": 0,
  "col": 0,
  "x": 0,
  "y": 0,
  "width": 140,
  "height": 140,
  "file": "slash.png",
  "category": "skills",
  "hero": "soldier",
  "output": "asset/icons/skills/slash.png"
}
```

---

## 15. Manifest Structure

Standard manifest schema:

```json
{
  "version": 1,
  "coordinate_system": "top-left origin; x increases right; y increases down",
  "sheets": {
    "32x32": {
      "source": "icons_32.png",
      "source_width": 1120,
      "source_height": 1400,
      "grid": {
        "columns": 8,
        "rows": 10,
        "cell_width": 140,
        "cell_height": 140,
        "origin_x": 0,
        "origin_y": 0,
        "used_grid_width": 1120,
        "used_grid_height": 1400,
        "unused_right_px": 0,
        "unused_bottom_px": 0
      },
      "final_icon_size": 32,
      "icons": []
    }
  }
}
```

---

## 16. Stage 9 — Programmatic Auto-Slicing

For each icon entry in the manifest:

```text
Read x, y, width, height
         ↓
Extract / Crop cell
         ↓
Resize to target dimensions
         ↓
Export PNG RGBA
```

Implementation pattern (Node.js / Sharp):

```javascript
for (const icon of manifest.icons) {
  const crop = image.extract({
    left: icon.x,
    top: icon.y,
    width: icon.width,
    height: icon.height,
  });

  await crop
    .resize(manifest.final_icon_size, manifest.final_icon_size, {
      kernel: "nearest",
    })
    .png()
    .toFile(icon.output);
}
```

---

## 17. Resizing Rules

Pixel art assets **MUST** be rescaled using:

```text
Nearest Neighbor
```

Strictly avoid interpolation algorithms such as:

```text
Bilinear
Bicubic
Lanczos
```

Interpolation introduces:

- Blurry outlines
- Unwanted semi-transparent edge halos
- Spurious color shifts
- Unauthentic anti-aliasing artifacts

---

## 18. Transparency Handling

Exported PNGs must strictly use:

```text
RGBA (4 channels with true alpha)
```

Never export as:

```text
RGB (3 channels without alpha)
```

If the generated source employs a solid chroma-key background (e.g., `#ff00ff` magenta):

```text
Set chroma pixels → alpha = 0
```

If the generated source already has native alpha transparency, do not run naive chroma removal that could inadvertently erase matching interior colors of the icon.

---

## 19. Stage 10 — Automated Validation

Immediately following the slicing phase, execute automated validation checks:

### Count Validation

```text
Expected  : 88
Generated : 88
Result    : PASS
```

---

### Dimension Validation

```text
skills/slash.png   → 32 × 32 px → PASS
paths/lancer.png   → 48 × 48 px → PASS
```

---

### Format Validation

Verify that all files are:

```text
Format : PNG
Space  : RGBA (Alpha channel present)
```

---

### Missing File Validation

Compare the filesystem contents directly against the manifest registry:

```text
Manifest items : 88 files
Filesystem     : 88 files
Missing        : 0 files
```

If any file is missing, fail immediately and resolve the issue before creating the ZIP archive.

---

## 20. Visual Quality Control (QC)

In addition to programmatic checks, conduct visual quality inspection:

### Transparency Inspection

Verify absence of:

- Solid white backgrounds
- Solid black bounding boxes
- Magenta chroma key fringes or leaks

### Boundary & Edge Clearance

Ensure no icon touches or clips against the edges of its bounding box.

### Pixel Fidelity

Verify that outlines remain sharp and distinct post-resizing.

### Readability at Scale

Icons must remain instantly readable and recognizable at their native resolution (e.g., `32×32`), not merely when magnified.

---

## 21. Stage 11 — Output Folder Architecture

Organize assets strictly according to the manifest categories:

```text
asset/icons/skills/
asset/icons/resources/
asset/icons/status/
asset/icons/boons/
asset/icons/stats/
asset/icons/paths/
asset/icons/titles/
asset/icons/difficulty/
```

---

## 22. Stage 12 — ZIP Packaging Standards

The ZIP archive must preserve the complete directory hierarchy.

Correct structure:

```text
icons.zip
├── asset/
│   └── icons/
│       ├── skills/
│       ├── resources/
│       └── ...
├── icon_grid_manifest.json
└── README.txt
```

Never produce a flat archive where all files are dumped into the root directory:

```text
slash.png
charge.png
mana.png
...
```

A flat structure destroys categorization and causes catastrophic collisions for shared filenames.

---

## 23. Technical README Generation

Include a concise technical summary in `README.txt`:

```text
Total icons: 88

32×32 Resolution:
Skills         36
Resources       4
Status          4
Boons           6
Stats          12

48×48 Resolution:
Paths          12
Titles         10
Difficulty      4

Technical Specifications:
- Format: PNG RGBA
- Background: Transparent (Alpha channel)
- Resampling Kernel: Nearest-Neighbor
- Coordinates: Refer to icon_grid_manifest.json
```

---

## 24. Completion Criteria

A task is considered complete **ONLY** when:

- [x] Total icon count matches specification exactly.
- [x] All filenames match the user's inventory verbatim.
- [x] All output file paths conform to requested directory structures.
- [x] Dimensions match specified resolution classes.
- [x] RGBA alpha transparency is verified.
- [x] Zero expected files are missing on disk.
- [x] Consolidated ZIP package is generated and verified.
- [x] Coordinate manifest JSON is complete and valid.

---

## 25. Summary Workflow Diagram

```text
USER SPECIFICATION
       │
       ▼
PARSE ICON LIST & GROUPINGS
       │
       ▼
ASSERT COUNTS / SIZES / DUPLICATE NAMESPACES
       │
       ▼
ESTABLISH MASTER STYLE & PALETTE
       │
       ▼
GENERATE BENCHMARK TEST (6–12 ICONS)
       │
       ▼
SYNTHESIZE DETERMINISTIC SPRITE ATLASES
       │
       ▼
CALCULATE EXACT GRID BOUNDS & MARGINS
       │
       ▼
CONSTRUCT JSON COORDINATE MANIFEST
       │
       ▼
PROGRAMMATIC SLICING (HEADLESS EXECUTION)
       │
       ▼
NEAREST-NEIGHBOR RESIZING
       │
       ▼
ENFORCE RGBA ALPHA TRANSPARENCY
       │
       ▼
MULTI-POINT AUTOMATED VALIDATION
       │
       ▼
CONSTRUCT DIRECTORY HIERARCHY
       │
       ▼
PACKAGE PRODUCTION ZIP & DELIVER
```

---

## 26. Handling User Requests

Example user prompt:

> "Generate 40 pixel-art RPG icons at 32×32 based on this list and deliver as a ZIP package."

Execution protocol:

1. Parse the complete list into memory.
2. Count and categorize entries.
3. Group items into batches by class or theme.
4. Preserve requested filenames strictly.
5. Generate the master style and benchmarks.
6. Synthesize the sprite atlas sheets.
7. Compute actual grid cell boundaries.
8. Generate the coordinate manifest JSON.
9. Slice and crop each icon.
10. Rescale via nearest-neighbor interpolation.
11. Run automated validation assertions.
12. Assemble categorized directory tree.
13. Bundle into the final ZIP archive.
14. Present the deliverable summary to the user.

Proceed autonomously through all technical stages without asking for redundant confirmations, unless core requirements (such as missing icon lists or ambiguous resolutions) are fundamentally unspecified.

---

## 27. Recommended Information to Request from User

Ideal specification payload:

```text
Project name:
Infinity Forest

Style:
16-bit SNES dark fantasy pixel art

Sizes:
32×32 px and 48×48 px

Background:
Transparent RGBA

Outline:
1 px crisp dark outline

Lighting:
Top-left directional

Palette:
8–12 colors per icon

Icon list:
[List of filenames and visual descriptions]

Folder structure:
[Target category directories]

Naming:
Use filenames exactly as provided
```

If all of the above parameters are provided or inferable, begin execution immediately.

---

## 28. User Input Template

Users can structure their requests using this template:

```text
PROJECT:
[Game title or codename]

STYLE:
[Pixel art / Dark fantasy / Sci-Fi / Cyberpunk / etc.]

ICON SIZE:
[32×32 / 48×48 / 64×64 / etc.]

BACKGROUND:
[Transparent RGBA]

OUTLINE:
[1 px dark outline]

LIGHTING:
[Top-left directional]

PALETTE:
[Optional specific hex codes or color themes]

ICONS:

Category: Skills
Folder: asset/icons/skills/

slash.png
- golden sword releasing a crescent blade slash

fireball.png
- flaming magical projectile with trailing sparks

Category: Resources
Folder: asset/icons/resources/

mana.png
- glowing blue multifaceted magic crystal

stamina.png
- vibrant green winged vitality emblem

OUTPUT REQUIREMENTS:
- Individual PNG files
- JSON coordinate manifest
- Consolidated ZIP archive
```

---

## 29. Image Generation Prompt Template

```text
Create a uniform pixel-art RPG icon atlas.

STYLE:
crisp hard-edged pixel art,
no anti-aliasing,
limited palette,
1-pixel dark outline,
top-left lighting,
strong readable silhouettes,
16-bit RPG visual language.

GRID:
perfectly uniform grid,
equal-width and equal-height cells,
all icons centered,
consistent scale,
large internal padding,
no icon crossing cell boundaries,
no overlap between icons,
no overlapping glow.

BACKGROUND:
transparent background.

ICONS IN EXACT ORDER:
1. [ICON 1 DESCRIPTION]
2. [ICON 2 DESCRIPTION]
3. [ICON 3 DESCRIPTION]
...

IMPORTANT:
Preserve the exact icon order.
Each icon must stay entirely inside its own grid cell with generous padding.
```

---

## 30. Core Principles

The skill rigorously adheres to three foundation principles:

### 1. Visual Consistency
Every icon looks like it belongs to the exact same universe, engine, and artistic direction.

### 2. Programmatic Predictability
Every sprite sheet is mathematically uniform and sliceable by headless scripts without manual intervention.

### 3. Production Readiness
Deliverables are not merely illustrative concept mockups; they are fully formatted, validated, clean assets ready to drop directly into a game project's asset tree.

---

## 31. Recommended Operating Modes

The skill supports three operating modes:

### Mode A — Generate Only
**Deliverable**:
- Consolidated sprite sheet / icon atlas.

---

### Mode B — Generate + Manifest
**Deliverables**:
- Consolidated sprite sheet / icon atlas.
- Coordinate manifest JSON.

---

### Mode C — Production Ready (Default)
**Deliverables**:
- Sliced individual PNG files.
- Categorized folder hierarchy.
- JSON coordinate manifest.
- Technical README documentation.
- Consolidated ZIP archive.

For game development workflows, **Mode C** is the mandatory default.

---

## 32. Strict Constraints & Guardrails

- Never alter filenames, categories, target sizes, or icon sequences without explicit user consent.
- Never assume a generated visual atlas has a mathematically perfect grid without measuring actual image bounds.
- Always measure pixel dimensions prior to computing slicing offsets.
- Never use smoothing or blurring interpolation (bilinear/bicubic) on pixel art.
- Never package or deliver a ZIP archive prior to running full validation checks.
- Never estimate file counts based on assumptions. Always enforce absolute parity:

```text
requested_count === generated_count === manifest_count === output_file_count
```

All four numbers must match exactly.

---

## 33. Example Final Report

Upon successful execution, present a concise report:

```text
========================================
ICON SUITE GENERATION COMPLETE
========================================

Total Assets: 88 PNG Files

32×32 px Icons (62 files):
- Skills     : 36
- Resources  :  4
- Status     :  4
- Boons      :  6
- Stats      : 12

48×48 px Icons (26 files):
- Paths      : 12
- Titles     : 10
- Difficulty :  4

Validation Assertions:
✓ Filename parity
✓ Dimension conformity
✓ RGBA alpha channel verified
✓ Transparent backgrounds confirmed (no color fringing)
✓ Coordinate manifest generated
✓ Categorized directory hierarchy constructed
✓ 88/88 files verified on filesystem

Deliverables:
- icons.zip (Production archive)
- icon_grid_manifest.json (Atlas coordinates)
- README.txt (Technical guide)
========================================
```

---

## 34. Skill Metadata

- **Name**: `pixel-icon-production-pipeline`
- **Display Name**: Pixel Icon Production Pipeline
- **Alternative Aliases**: `game-icon-atlas-builder`, `pixel-asset-forge`
- **Short Description**:
  > Generate consistent game icons, build deterministic sprite atlases, create coordinate manifests, automatically slice and resize assets, validate them, and package production-ready PNG files into the requested project structure.
