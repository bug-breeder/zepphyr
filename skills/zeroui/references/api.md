# ZeRoUI v0.2.0 API Reference

## Layout modes (480-unit design canvas)

### LAYOUT.FULL
```
LAYOUT.FULL.TITLE:  { x: 120, y: 24,  w: 240, h: 44  }
LAYOUT.FULL.MAIN:   { x: 80,  y: 74,  w: 320, h: 312 }
LAYOUT.FULL.ACTION: { x: 140, y: 392, w: 200, h: 48  }
```

### LAYOUT.NO_TITLE
```
LAYOUT.NO_TITLE.MAIN:   { x: 80,  y: 62,  w: 320, h: 324 }
LAYOUT.NO_TITLE.ACTION: { x: 140, y: 392, w: 200, h: 48  }
```

### LAYOUT.NO_ACTION
```
LAYOUT.NO_ACTION.TITLE: { x: 120, y: 24,  w: 240, h: 44  }
LAYOUT.NO_ACTION.MAIN:  { x: 80,  y: 74,  w: 320, h: 342 }
```

### LAYOUT.MINIMAL
```
LAYOUT.MINIMAL.MAIN: { x: 80, y: 62, w: 320, h: 354 }
// Fullscreen / immersive — no title, no action button. Was MAIN_ONLY.
```

---

## configure()

```
configure({ accent: 'green' })   // preset: 'green'|'blue'|'red'|'orange'|'purple'
configure({ accent: { primary: 0x007aff, primaryLight: 0x4da3ff, primaryTint: 0x001f4d, primaryPressed: 0x0051d5 } })
```

Call once in `app.js`. Mutates `COLOR.PRIMARY*` in place — all pages see updated values.

---

## renderPage()

```
renderPage({
  layout,       // required: LAYOUT.*
  title,        // optional: string
  action,       // optional: { text, onPress, variant?: 'primary'|'secondary' }
  scrollable,   // optional: bool, default true
  buildFn,      // optional: (col: Column) => void — create content here
}) → Column
```

Render order (z-order, low → high):
1. FILL_RECT bg
2. VIEW_CONTAINER (if scrollable)
3. buildFn(col) — Column content
4. Top mask FILL_RECT (hides scroll overflow above MAIN)
5. Title TEXT
6. Bottom mask FILL_RECT
7. Action BUTTON

---

## Column methods

### Text
```
col.label(text, { color?, align? }): widget
// color: 'muted'(default)|'default'|'primary'|'danger'|'warning'|'success'|hex
// align: 'center'(default)|'left'|'right'
// h=34, gap=8

col.text(text, { size?, color?, align?, wrap?, h? }): widget
// size: 'caption'|'body'(default)|'subheadline'|'title'|'largeTitle'
// color: 'default'(default)|'muted'|'disabled'|'primary'|'danger'|'warning'|'success'|hex
// wrap: false(default)|true — requires explicit h for y-tracking
// h: auto (fontSize+4) or explicit

col.heroNumber(value, { color? }): widget
// Large centered number. color same options as col.text.
// h=64
```

### Interactive
```
col.chip(text, { selected?, onPress?, variant? }): widget
// variant: 'default'(default)|'primary'|'secondary'|'danger'|'ghost'
// selected applies only to variant='default'
// h=48, gap=4

col.chipRow(options, { selected?, onPress?, variant? }): widget[]
// N equal-width chips in a row
// h=48, gap=4
```

### Display
```
col.card({ title, value, valueColor?, h? }): widget[]
// valueColor: string alias or hex. h default 80. Returns [bg, value, label].
// gap=4

col.progressBar(value, { color?, h?, radius? }): [track, fill]
// value: 0.0–1.0. color: 'primary'(default)|'secondary'|'danger'|hex
// h default 8, radius default 4

col.image(src, { w?, h? }): widget
// Centered in column width. Default w and h = col.w.
```

### Structure
```
col.divider({ color?, margin? }): widget
// color: 'surface'(default) or string alias. margin default 4 (above+below).

col.spacer(n): void
// Advance y by n units, no widget.
```

### Escape hatch
```
col.widget(type, props, h): widget
// Raw hmUI widget. Read col.currentY FIRST for y in props. Advances y by h.
// e.g.: col.widget(hmUI.widget.ARC, { x: 40, y: col.currentY, w: 400, h: 400 }, 400)
```

### Lifecycle
```
col.finalize(): void        // required for scrollable — sets VIEW_CONTAINER height
col.clearContent(): void    // destroy children, reset y — use in rebuild loops
col.destroyAll(): void      // full teardown — call only in onDestroy()
col.currentY: number        // current y position (read-only)
```

---

## Standalone components

```
bg(): void
// FILL_RECT 480×480 COLOR.BG — only needed when not using renderPage()

title(text, { layout? }): widget
// TEXT in layout.TITLE zone. layout default: LAYOUT.FULL

column(zone?, opts?): Column
// Factory — only needed for custom layouts outside renderPage()
// zone default: LAYOUT.FULL.MAIN

actionButton(text, { onPress?, variant?, layout? }): widget
// variant: 'primary'(default)|'secondary'
// layout default: LAYOUT.FULL
```

---

## Token tables

### COLOR
| Token | Hex | Notes |
|---|---|---|
| `BG` | `0x000000` | OLED black |
| `SURFACE` | `0x1c1c1e` | chip/card bg |
| `SURFACE_PRESSED` | `0x2c2c2e` | |
| `SURFACE_BORDER` | `0x2c2c2e` | |
| `PRIMARY` | `0x30d158` | mutable via configure() |
| `PRIMARY_LIGHT` | `0x52d985` | mutable via configure() |
| `PRIMARY_TINT` | `0x0c2415` | mutable via configure() |
| `PRIMARY_PRESSED` | `0x25a244` | mutable via configure() |
| `SECONDARY` | `0x007aff` | |
| `SECONDARY_PRESSED` | `0x0051d5` | |
| `DANGER` | `0xfa5151` | |
| `SUCCESS` | `0x34c759` | |
| `WARNING` | `0xff9f0a` | |
| `TEXT` | `0xffffff` | |
| `TEXT_MUTED` | `0x8e8e93` | |
| `TEXT_DISABLED` | `0x3a3a3c` | |

### TYPOGRAPHY
| Token | Size | Use |
|---|---|---|
| `largeTitle` | 60 | hero numbers |
| `title` | 44 | section titles |
| `body` | 40 | body text |
| `subheadline` | 34 | chips, buttons |
| `caption` | 30 | labels, hints |

### RADIUS
| Token | Value |
|---|---|
| `pill` | 999 |
| `chip` | 12 |
| `card` | 12 |

### SPACING
| Token | Value |
|---|---|
| `xs` | 4 |
| `sm` | 8 |
| `md` | 16 |
| `lg` | 24 |
| `xl` | 32 |
| `chipGap` | 4 |
| `sectionGap` | 8 |
