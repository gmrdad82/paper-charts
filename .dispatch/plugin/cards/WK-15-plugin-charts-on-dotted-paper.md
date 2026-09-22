# WK-15 — Plugin: Charts on dotted paper

Wave: 1 · Run: single · Depends on: none · Complexity: ***
Branch: split-wk-15 · Base: main

## What
Fenced data blocks in an Idea's body become a chart drawn on the dotted ruling in the `panel` slot.

## How
1. The ruling hint in the desk.
2. The plugin crate: block format, `src/parse.rs`, `src/scale.rs`, `src/draw.rs`.
3. Golden primitive tests for a line and a bar block; captures measured.

## Guards
- Do not touch: the host, the slots, the tree renderer.
- Gate: the project's gate
- Accept: parser tests cover every rule and both failure cases.
