# WK-15 — Plugin: Charts on dotted paper

Branch: split-wk-15 (in both repos) · Worktree: ~/Projects/.worktrees/paper-charts/split-wk-15 and ~/Projects/.worktrees/pito-work/split-wk-15

## What
The book (sections 1, 6): paper carries three rulings, and the dotted areas exist for charts, "as a plugin should be made for adding chart feature" (his words, 2026-09-15). Charts is the first-party plugin that proves a plugin with its own data: fenced data blocks in a Page body become a chart drawn on the dotted ruling in the Page's `panel` slot. The desk knows nothing about charts;

## How
1. **The ruling hint (pito-work).** Read WK-07's panel ground seam (the file the runbook of WK-07 names); if a `panel` tree can already ask for `dotted`, nothing to do;
2. **The plugin crate.** `pito-plugins/plugins/charts/` from the template: `plugin.toml` (id `charts`, name "Charts", version `0.1.0`, host `work`, capabilities: `core:read` — it reads the open Page's body through the world; nothing else), `Cargo.toml`, `src/lib.rs` exporting the `panel` slot.
3. **The block format.** ` ```chart ` fence; first lines `type: line` or `type: bar`, `x: <column>`, `y: <column>[, <column>…]` (up to four series), then CSV rows with a header row;
4. **The parser** (`src/parse.rs`): fence detection over the body (a fence inside a fenced code block of another language is not a chart), header, CSV (quoted fields with commas allowed), typed columns. Unit tests: one per rule above, plus the two failure cases.
5. **Scales and layout** (`src/scale.rs`): linear scales for numbers, an ordinal scale for labels, a day scale for dates; nice ticks (1-2-5 steps);
6. **Primitives** (`src/draw.rs`): axes as paths, ticks as short paths, labels as `text`, a line series as one path per series, a bar series as rects with WK-06's bar gap; colours come from the desk's palette through the host (the tree names palette roles — `ink`, `accent`, `work_lit_1..3` as `src/desk/theme.rs:176-182` names them today — never a colour literal in the plugin).
7. **The slot.** `panel` returns a `column` of one canvas per chart block, each with the `dotted` ruling hint and a caption row (the block's index and type). No block: the plugin returns an empty tree and the host draws no panel.

## Guards
- Do not touch: - The host, the slots and the tree renderer (`pito-work/src/plugins/*`): WK-13's, except the ruling hint of step 1. - The paper surface's numbers: WK-06's and WK-07's;
- Gate: the project's gate
- Accept: The parser tests cover every rule of step 3 and both failure cases (test names in the runbook).
- Accept: The golden primitive tests hold for a line block and a bar block (step 8).
- Accept: US-1's captures measure the dot pitch, the bar gap and the label size against WK-06's numbers (three numbers in the runbook).
- Accept: A block over the caps renders the cap message, never a panic (a test and US-2's capture).
