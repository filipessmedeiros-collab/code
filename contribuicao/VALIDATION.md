# Validation

Environment: headless Linux x86_64, Rust 1.99.1. Source pins and local override details are in
README.md. No dependency declarations or lockfiles in either product repository were changed to run the local integration.

| Check | Result |
| --- | --- |
| `craft-ui`: full package tests with `serde` | 84 passed, 0 failed, 0 ignored |
| `craft-ui`: Clippy, all targets, `-D warnings` | Passed |
| `craft-ui`: wasm32 check with `serde` | Passed |
| PhotoCraft: full `photocraft-ui-egui` package tests | 3,198 passed, 0 failed, 13 ignored |
| PhotoCraft: focused docking regression suite | 31 passed, 0 failed |
| PhotoCraft: real Brushes panel visual regression | Passed; before/after PNGs inspected |
| PhotoCraft: architecture (`cargo xtask layers`) | Passed: 31 crates, no violations |
| Both workspaces: `cargo fmt --check` | Passed |
| Both patches: forward application to clean base files | Passed |
| PhotoCraft: full `cargo xtask wasm` | Passed, including `photocraft-codecs --features heif` |
| PhotoCraft: Clippy, all targets, `-D warnings` | Passed |

The 13 ignored tests are marked as such by the project; no tests were disabled or assertions
weakened for this change. The full application package run includes 3,085 passing unit tests and
113 passing integration tests. Its doctest runner reported zero doctests.
The wasm check emitted three dead-code warnings from unchanged `photocraft-engine` code.

The shared-library visual workflow exercises all eight handles in dark/light themes, 640/1000
point windows and 1x/2x scales. It grows and shrinks panels and checks that content and widget
identity are retained. Its 64 growth captures were retained; representative light/dark images
were inspected. The PhotoCraft test separately checks all eight cursor directions and gestures,
valid layout membership, and workspace restoration.

Validation used offscreen rendering and real egui pointer events. An interactive desktop launch,
release installer build, and publication to the original repositories were not performed.
The local path override is required until the shared-library fix is merged and PhotoCraft pins
the resulting upstream commit.

To stay within the cloud machine's disk capacity, debug-only sections were stripped from
generated integration/example executables. Executable code, source files and assertions were
unchanged.

## Evidence follow-up

The first real-panel capture demonstrated the existing lower-right handle. It was insufficient
as evidence of newly supported regions. The follow-up changes only the capture/regression
code; the production fix is unchanged.

The real PhotoCraft test now grows the panel by 96 points from every edge and corner, checks
cursor output on hover and at every drag step, and saves nine frames plus before/after images
per region. Left/top/top-left demos are highlighted. The complete focused docking suite was
rerun: 31 passed, zero failures. Clippy all targets with `-D warnings` was rerun successfully.
The full-package figures above are the original validation run, preceding these test-only
capture changes. No unrelated full suite was repeated for this evidence update.

GIFs are encoded from the application frames using FFmpeg, with endpoint pauses and a shared
palette. The cursor-aware follow-up below changes how the test renderer paints the cursor. Each GIF has 19 frames at
1280×800 and lasts 3.8 seconds.

## Cursor-aware capture follow-up

The default egui_kittest renderer always paints a generic triangular pointer, regardless of
CursorIcon. A separate explicit local copy of the published egui_kittest 0.36.2 package is
patched only for evidence rendering (`03-test-renderer-cursor.patch`). It selects horizontal,
vertical and diagonal software arrows from the application's actual CursorIcon and leaves
ordinary pointers unchanged. This is not native Windows cursor artwork. Production uses the
existing native cursor path, mapped by winit on Windows to IDC_SIZEWE / IDC_SIZENS / diagonals.
The verified registry package was not modified in place, and product dependencies and lockfiles
remain unchanged. Cursor direction and resize geometry continue to be checked by the same
regression assertions, independently of the rendered cursor.

Cursor-aware rerun: all 31 docking tests passed with the optional renderer override. PhotoCraft
Clippy (`--all-targets -- -D warnings`) also passed with that override. Horizontal and vertical
hover images and diagonal frames were inspected. Selecting the overridden egui_kittest package
directly for Clippy triggered a Cargo feature-resolution panic; selecting the integrating
PhotoCraft package instead completed successfully with the full active feature graph.

## PR #189 review follow-up

Reserve a 10-point floating resize border (bounded by the frame dimensions) and inset tab/header/body widgets. Keep movement and saved geometry on the outer frame. Increase the owned-interaction capacity to cover all floating handles and prior/current-frame identities. Regression tests cover non-overlap, real scrollbar dragging and cancellation beyond the former interaction cap.

Updated validation: full shared workspace tests 544 passed, 0 failed, 4 ignored; craft-ui with serde 92 passed, 0 failed, 0 ignored; full workspace Clippy with -D warnings and formatting passed. PhotoCraft docking integration: 31 passed, 0 failed. The old GIFs demonstrate the initial revision; they predate the reserved border. Windows CI and manual testing reported earlier apply to the initial revision; Windows must be rerun for this update.
