# Showcase: Pixel Icon Production Pipeline
## Case Study: "Infinity Forest" 88-Icon RPG Asset Suite

This showcase demonstrates the end-to-end execution of the **Pixel Icon Production Pipeline** skill. It illustrates how a complex, multi-category set of game icon specifications is parsed, synthesized into mathematically deterministic sprite atlases, mapped into coordinate manifests, sliced with zero anti-aliasing via nearest-neighbor resampling, validated through automated quality assurance, and packaged into a clean, game-engine-ready asset structure.

---

### Included
- Skill instructions: `SKILL.md`
- Web showcase: `showcase/index.html`
- Features: simple icon gallery, category and size filters, search, enlarged pixel previews, and individual PNG downloads.

---

## 1. Project Specification & Scope

### Project Overview
- **Game Title**: Infinity Forest
- **Aesthetic**: 16-bit SNES-era dark fantasy pixel art
- **Total Icons**: 88 unique icons
- **Size Classes**: Dual-resolution architecture (32×32 px & 48×48 px)
- **Color Model**: 4-channel RGBA (True transparency)
- **Resampling Method**: Strict Nearest-Neighbor (No bilinear/bicubic blur)

### Asset Breakdown by Category & Resolution

| Category | Size | Count | Description & Sub-grouping |
| :--- | :---: | :---: | :--- |
| **Skills** | 32×32 | 36 | 6 Hero Classes (Soldier, Knight, Archer, Mage, Rogue, Cleric) × 6 abilities each |
| **Resources** | 32×32 | 4 | Core gameplay energy gauges (`mana.png`, `stamina.png`, `resolve.png`, `focus.png`) |
| **Status Effects** | 32×32 | 4 | Combat debuffs (`stunned.png`, `slow.png`, `frozen.png`, `marked.png`) |
| **Boons** | 32×32 | 6 | Combat buffs (`empowered.png`, `ward.png`, `haste.png`, `vampiric.png`, `guard.png`, `focus.png`) |
| **Stats** | 32×32 | 12 | Core character attributes (`strength.png`, `vitality.png`, `agility.png`, `intellect.png`, etc.) |
| **Evolution Paths**| 48×48 | 12 | Hero class mastery crests (`lancer.png`, `templar.png`, `frostmage.png`, `shadowblade.png`, etc.) |
| **Titles** | 48×48 | 10 | Achievement medallions (`centurion.png`, `dawnbringer.png`, `gatebreaker.png`, etc.) |
| **Difficulty** | 48×48 | 4 | Game difficulty crests (`story.png`, `normal.png`, `hard.png`, `nightmare.png`) |
| **TOTAL** | — | **88** | **Complete production suite** |

---

## 2. Pipeline Execution Walkthrough

```text
[ User Specification ]
         │
         ▼
[ Stage 1: Spec Parsing & Namespace Protection ]
  • Calculated 88 icons, partitioned into 32px (62) & 48px (26)
  • Resolved duplicate names (e.g., boons/focus.png vs resources/focus.png)
         │
         ▼
[ Stage 2: Master Style & Cohesion Prompting ]
  • Top-left directional lighting, 1px crisp dark outlines
  • 8–12 color palette, SNES 16-bit RPG aesthetic, no anti-aliasing
         │
         ▼
[ Stage 3: Multi-Sheet Deterministic Atlas Synthesis ]
  • Sheet 1 (32x32 icons): 8 columns × 10 rows (140×140 cell dimensions)
  • Sheet 2 (48x48 icons): 5 columns × 6 rows (229×229 cell dimensions)
  • Generous cell margins to ensure zero glow/shadow boundary bleed
         │
         ▼
[ Stage 4: Coordinate Manifest Generation ]
  • Measured actual canvas bounds (1122×1402 px)
  • Truncated 2px padding, calculated (row, col, x, y, width, height)
  • Exported `icon_grid_manifest.json`
         │
         ▼
[ Stage 5: Programmatic Slicing & Nearest-Neighbor Resizing ]
  • Automated cropping of cells via headless image processing
  • Applied strict nearest-neighbor resampling to preserve sharp pixel edges
         │
         ▼
[ Stage 6: Multi-Point Quality Assurance Validation ]
  • Count check: 88/88 passed
  • Dimensions check: 62 @ 32×32, 26 @ 48×48 passed
  • Format check: True RGBA with zero opaque background boxes
         │
         ▼
[ Stage 7: Production Hierarchy Packaging ]
  • Output structured in `asset/icons/<category>/<file>`
  • Bundled with manifest and developer README into `icons.zip`
```

---

## 3. Deterministic Coordinate Manifest (`icon_grid_manifest.json`)

The skill generates a mathematical mapping of each icon before slicing begins:

```json
{
  "version": 1,
  "coordinate_system": "top-left origin; x increases right; y increases down",
  "sheets": {
    "32x32": {
      "source": "fantasy_pixel_art_ability_sprite_sheet.png",
      "source_width": 1122,
      "source_height": 1402,
      "grid": {
        "columns": 8,
        "rows": 10,
        "cell_width": 140,
        "cell_height": 140,
        "origin_x": 0,
        "origin_y": 0,
        "used_grid_width": 1120,
        "used_grid_height": 1400,
        "unused_right_px": 2,
        "unused_bottom_px": 2
      },
      "final_icon_size": 32,
      "icons": [
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
          "output": "asset/icons/skills/slash.png",
          "hero": "soldier"
        }
      ]
    }
  }
}
```

---

## 4. Programmatic Auto-Slicing Script

The skill enforces headless, reproducible slicing using Node.js (`sharp`):

```javascript
const sharp = require('sharp');
const fs = require('fs');
const path = require('path');
const manifest = require('./icon_grid_manifest.json');

async function processIcons() {
  for (const [sheetName, sheet] of Object.entries(manifest.sheets)) {
    console.log(`Processing Sheet: ${sheetName} (Target size: ${sheet.final_icon_size}x${sheet.final_icon_size})`);
    
    for (const icon of sheet.icons) {
      const outDir = path.dirname(icon.output);
      if (!fs.existsSync(outDir)) {
        fs.mkdirSync(outDir, { recursive: true });
      }

      await sharp(sheet.source)
        .extract({
          left: icon.x,
          top: icon.y,
          width: icon.width,
          height: icon.height
        })
        .resize(sheet.final_icon_size, sheet.final_icon_size, {
          kernel: sharp.kernel.nearest // Critical: Maintains hard pixel contours
        })
        .png()
        .toFile(icon.output);
    }
  }
  console.log('✓ All 88 icons sliced and resized.');
}

processIcons();
```

---

## 5. Quality Assurance & Validation Report

Before assets are delivered or zipped, the skill runs an automated validation checklist:

- [x] **File Count Parity**: 88/88 requested icons generated and verified on the file system.
- [x] **Dimension Conformance**:
  - `skills/`, `resources/`, `status/`, `boons/`, `stats/`: 62 files verified at **32×32 px**.
  - `paths/`, `titles/`, `difficulty/`: 26 files verified at **48×48 px**.
- [x] **Alpha Channel & Color Space**: 100% of icons verified as 4-channel **PNG RGBA**.
- [x] **Collision Safety**: Duplicate names across different categories (`skills/guard.png` vs `boons/guard.png`, `resources/focus.png` vs `boons/focus.png`) isolated in distinct directories without overwrite errors.
- [x] **Edge Clarity**: Verified zero cutoff edges or border bleed artifacts.
- [x] **Nearest-Neighbor Rescaling**: Confirmed zero anti-aliasing fuzziness or blurry edge fringes.

---

## 6. Packaged Output Structure

```text
icons.zip
├── asset/
│   └── icons/
│       ├── boons/         (6 icons  @ 32x32)
│       ├── difficulty/    (4 icons  @ 48x48)
│       ├── paths/         (12 icons @ 48x48)
│       ├── resources/     (4 icons  @ 32x32)
│       ├── skills/        (36 icons @ 32x32)
│       ├── stats/         (12 icons @ 32x32)
│       ├── status/        (4 icons  @ 32x32)
│       └── titles/        (10 icons @ 48x48)
├── icon_grid_manifest.json (Coordinate registry)
└── README.txt              (Technical developer notes)
```
