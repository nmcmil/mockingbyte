# Mockingbyte

**Draw Apple ][ user interfaces, the way the hardware actually allows.**

Mockingbyte is a tiny, **single-file, zero-dependency** browser tool for mocking up Apple ][
screens — a 40-column Lo-Res block grid with the authentic 16-colour palette, the real
character-ROM font, true 4:3 non-square-pixel aspect, mixed-mode text lines, uppercase-only
text, and PNG/JSON export.

Standalone — open it in any browser, no install, works offline, and you can hand the whole
folder to anyone.

**Try it:** https://nmcmil.github.io/appleiiuidesigner/

## Use it

Open `index.html` in a browser. That's it. (Double-click the file, or serve the folder with
any static server.)

## What it models (the "rules")

- **40 columns** wide — the Apple ][ text/Lo-Res width.
- **True 4:3 screen** with **non-square pixels**: the design is drawn at the native
  280×192 framebuffer and stretched to a 4:3 frame, exactly like an Apple ][ on an NTSC
  monitor — so a Lo-Res block reads as ~1.6:1 and text is slightly taller than wide, the
  way it really looked.
- **The real font**: text is rendered from the actual Apple ][ character ROM (341-0036),
  pixel for pixel — not a lookalike system font.
- **Lo-Res blocks**: 40 × 48 chunky blocks, each in one of **16 true colours** (the palette
  swatches are numbered 0–15 = the colour you'd `POKE`).
- **Three modes:**
  - **Mixed Lo-Res** *(default, what the game uses)* — 40 × 40 colour blocks on top, with
    **4 lines of hardware text** at the bottom. This 3-mode split is a real hardware limit:
    you can't put a hardware-text line in the middle of the graphics area.
  - **Full Lo-Res** — all 40 × 48 blocks, no text.
  - **Full Text** — 40 × 24 characters, no colour (for menu / shop / library screens).
- **Text anywhere — the honest way.** Type in the bottom 4 lines and you get crisp **hardware
  text**. Type in the graphics area and the same ROM glyph is stamped as colour **blocks**
  ("block text") — exactly how the game draws rank/suit on the cards, and the only way to put
  text in the graphics region on a real Apple ][. Block text is chunkier and uses your
  selected colour; the contrast between the two makes the hardware limit obvious.
- **Text is UPPERCASE only** (the ][+ character ROM has no lowercase). Suits are letters
  **S H D C**, ten is **T**. Inverse (black-on-white) is supported for highlights.

## Designs

Use the **Design** bar at the top to keep multiple mockups:

- **New** — a fresh felt screen.
- **Clone** — duplicate the current design (for revisions / variants).
- **Rename**, **Delete**, and a dropdown to **switch** between them.

Everything auto-saves to the browser, so your set of designs is there next time.

## Controls

- **Paint** — pick a colour, click/drag to fill blocks.
- **Rect** — drag a filled rectangle.
- **Fill** — flood-fill a region.
- **Erase** — paint black.
- **Text** — click any cell, then type. In the bottom 4 lines it's hardware text; in the
  graphics area it's block text in the current colour. `← ↑ → ↓` move, `Enter` newline,
  `Backspace` delete, `Esc` finish.
- **Select** — drag a box of blocks, then **drag inside it to move** the blocks. Copy / Cut /
  Paste / Delete buttons or `⌘/Ctrl+C/X/V` and `Del`. Paste lands at the selection (or where
  the mouse is). Great for cloning a card, nudging a row, rearranging a layout.
- **Block font** dropdown — graphics-area text comes in **Small (3×5)**, **Standard (5×7,
  the real ROM glyph)**, and **Large (10×14, doubled)**. Hardware text in the bottom 4 lines
  is always the one fixed ROM font (that's all a stock ][ has).
- **Inverse** checkbox — type highlighted (black-on-white) hardware characters.

- **Undo / Redo** — step backward and forward through your edits (every paint stroke, rect,
  fill, text keystroke, selection move/paste, felt/clear, and mode change is one step). Buttons
  in the Edit panel or `⌘/Ctrl+Z` to undo, `⌘/Ctrl+Shift+Z` (or `Ctrl+Y`) to redo. History is
  per design.

Keyboard: **P** paint · **R** rect · **F** fill · **E** erase · **T** text · **V** select ·
**Esc** clear cursor/selection · **⌘/Ctrl+Z / ⇧Z** undo/redo. The status bar under the canvas
shows the active tool, colour, font, and cursor position.
- **Fill felt** — flood the graphics area with table-green (handy starting point).
- Your work **autosaves** to the browser; **Save/Open JSON** to keep or share a file.

## Export / share

- **Save PNG** — a clean image of the mockup (no grid).
- **Save JSON** / **Copy JSON** — the exact design as data. This is the best thing to paste
  back when sharing a layout: it round-trips perfectly via **Open JSON**, and it lists the
  text lines and the block-colour grid in a form that's easy to read and to translate into
  `gr_*` / `cputsxy` calls.

The readable text box at the bottom always shows your text lines as `row| content`.

## JSON format

```json
{
  "tool": "mockingbyte", "version": 2,
  "mode": "mixed", "cols": 40, "blockRows": 48,
  "palette": [{ "i": 0, "name": "BLACK", "hex": "#000000" }, ...],
  "gfx":  [[ /* 40 colour indices */ ], ... 48 rows ],
  "text": [ { "row": 20, "chars": "ANTE 1/8 ...", "inverse": "0000..." }, ... ]
}
```

## License / distribution

Self-contained and free to copy. No build step, no network calls, no tracking — just one
HTML file.
