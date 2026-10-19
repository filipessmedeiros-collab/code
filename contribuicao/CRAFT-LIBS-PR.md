# Allow floating dock panels to resize from every edge and corner

Floating dock panels currently expose only a lower-right resize grip. For example, dragging the left or top border of PhotoCraft's floating Brushes panel cannot resize it.

Add four edge handles and four corner handles to the shared docking surface. Each handle emits the corresponding horizontal, vertical or diagonal cursor during hover and drag. Resizing anchors the opposite edge, respects panel size limits and screen bounds, and preserves the existing lower-right handle ID, keyboard interaction and cancellation behavior. Ignore pointer motion on the initial press frame to avoid a jump when hover movement and press arrive together.

Regression tests exercise all eight regions, cursor direction, size constraints, opposite-edge anchoring, cancellation and layout persistence. No dependency or public API changes are required.

## Validation

- Linux: `cargo fmt --check`, workspace Clippy with warnings denied, and workspace tests: 82 passed, zero failures, zero ignored (default features).
- Previous feature-enabled `craft-ui` suite: 84 passed.
- PhotoCraft integration on Linux: 31 docking tests passed. Original full UI package run: 3,198 passed, 13 ignored.
- Contributor reports the Windows workflow completed successfully and manually verified resizing and cursor behavior in the resulting Windows executable.
- [Windows workflow](https://github.com/filipessmedeiros-collab/code/actions/workflows/windows-resize.yml)

## Demonstrations

These captures use real PhotoCraft panel state and pointer events with a software test cursor driven by CursorIcon; they are not Windows screen recordings.

![Left edge](https://raw.githubusercontent.com/filipessmedeiros-collab/code/main/imagens/borda-esquerda-cursor.gif)
![Top edge](https://raw.githubusercontent.com/filipessmedeiros-collab/code/main/imagens/borda-superior-cursor.gif)
![Top-left corner](https://raw.githubusercontent.com/filipessmedeiros-collab/code/main/imagens/canto-superior-esquerdo-cursor.gif)

PhotoCraft will need a separate dependency revision update and its application regression test after this change is merged. The optional screenshot-renderer patch is not part of this contribution.

The shared craftrules repository is not public; please advise if additional applicable standards require adjustments.
