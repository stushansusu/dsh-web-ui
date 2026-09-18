# Hairline (素线)

English | [中文](README.zh.md)

A dark-only dsh skin drawn like a drafting sheet: every line in the interface is
**drawn, not filled** — 1px ink hairlines, frosted-glass panels, and a single
continuous ink stroke for the water. The backdrop is a looping water-surface
video.

## What it is

- **Pure assets**: `skin.json` (v2 manifest) + `skin.css` (full
  `--dsw-alias-*` token remap) + `patches.css` (L3 free selectors) + one
  looping MP4. No package.json, no build step, no hooks.
- **Dark-only by construction**: the palette is declared on both `:root` and
  `body[data-ds-dark-theme]`, every L3 rule drops its light/dark branch, and
  `color-scheme: dark` is pinned so native widgets follow. The market renderer
  injects the skin without running the loader's `:root` → `body` token clone,
  so the palette is declared on `body` as well.
- **Measured under both host themes, not asserted.** The two committed previews
  are captures of the market simulator under a **light host**
  (`preview.html?skin=hairline&theme=light`, i.e. `body[data-ds-dark-theme]`
  absent) and a dark host, at 1440×900. Isolating the moving backdrop — the
  `<video>` hidden, the scrim kept — the two host themes render **0 differing
  pixels** (max channel delta 0 over 1,296,000 px). The shipped pair still
  differs as files only because the backdrop is a running loop and the two
  captures land on different water frames.
- **Three module-CSS light branches are pinned**: `Deliverables`, `JsonTree`
  and `GuideBody` branch on `body[data-ds-dark-theme]` and read
  `--dsw-static-*` instead of tokens, so a light scheme paints them with stock
  light values. They are re-pinned through stable hooks
  (`[data-presented-files-row]`, `div:has([data-json-root-row])`,
  `[data-sidebar-right-guide] > span[aria-hidden='true']`).

## Frosted glass

Five surfaces are frosted, and **every blur sits on an empty pseudo-element**,
never on the container itself: the sidebar, the details pane, the composer card,
the session header and the conversation column each carry their own
`::before` overlay. A `backdrop-filter` on the container would make it the
containing block for `position: fixed` descendants — and the settings dialog is
rendered inside the sidebar subtree. Measured: **0 elements** in the page carry a
`backdrop-filter`; all five layers read their blur back from pseudo-elements.

## Workspaces as an index table

The sidebar's Workspaces block is laid out like a drawing index. A workspace is a
title tag — a 3px ink rule, a mono name, a hairline box; the open one takes a
paper fill and a solid ink edge. Its sessions hang from a continuous 1px vertical
rail (the rows are contiguous, so the per-row edge shadows join into one line),
and each row's trailing value sits on a hairline. The current session lights up a
3px ink segment on that rail.

## Backdrop

One continuous ink stroke is *not* used — the empty-session screen draws nothing
at all; the footage is the picture. The video is a 6.33 s seamless loop at
1920×1080 / 30 fps: the source was 7.035 s, and its last 0.7 s is crossfaded into
its first 0.7 s, so the wrap point is two consecutive frames of the original
(measured seam delta 0.56 against a 1.80 mean inter-frame delta). Glass opacity is
deliberately thin — the panels are readable because the backdrop behind them is
blurred, not because they are opaque.

The footage is **third-party, sourced from the internet**, and it is the one file
here that is not the author's own work. `NOTICE` records what is known about it
(source, unidentified rights holder) and the basis on which it is included;
`LICENSE` (CC0 1.0) covers the skin's own code and stylesheets only, not the
video.

## Palette

- Ground: deep pool `#071310`
- Ink: near-white `#e9f2ed` (on a dark ground the hairlines are the *bright* thing)
- Accent: water jade `#6fd3b0` — lines and text only, never a fill
- Attention: `#e0685c` — the only red in the skin, reserved for approvals

## Preview

`preview/light.jpg` is a render under a **light host system** and
`preview/dark.jpg` under a dark one — two independent captures of the market
simulator at 1440×900, not one file copied twice. They look the same because the
skin is dark-only by design; the pixel measurement above is what backs that
claim. The backdrop is a running video loop, so the two captures sit on
different frames of the water.
