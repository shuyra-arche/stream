# Provenance

## Source

- Canonical repository: <https://github.com/anreonyr/sqware.git>
- Frozen public HEAD: `68e7afb7416f8008d5ed39cbc1daa53aa2b5d087`
- Public source branch: `codex/shuyra-ipc-extraction` (PR38)
- Local extraction branch: `codex/shuyra-stream-extraction` (not published)
- Source worktree was clean at the frozen HEAD before and after this snapshot.

`crates/stream` has one source commit,
`35b3d73e13f695c935665efa6d79b12265af98e7` (author
`anreonyr <anreonyr@gmail.com>`, 2026-10-10), which added both files. The frozen
tip does not modify them. Attribution is taken from the real Git history, not
from Cargo metadata; Cargo declares no `authors`. Full history remains in the
original repository.

No license, licence, copying, or notice file exists among the tracked files at
frozen commit `68e7afb7416f8008d5ed39cbc1daa53aa2b5d087` or in the extracted
`crates/stream`, and the `stream` package metadata declares no license. No
license text or permission is added, removed, or inferred here.

## Two-file mapping

| source | provider | source_sha256 | provider_sha256 | state |
|---|---|---|---|---|
| `crates/stream/Cargo.toml` | `Cargo.toml` | `a4d4f25f1ed4ef10c6c51ffcd36771869fe3b5bc7b5525fda5a3a39b3ec027e3` | `ce0936cdbc339240c21be9d1f9982bca15363c6ec38c97d7291de92408647683` | adapted |
| `crates/stream/src/lib.rs` | `src/lib.rs` | `e661d726ee704c74e6f0bb532c54a5d8380aecfbe9fdece64a0613aa8b535711` | `e661d726ee704c74e6f0bb532c54a5d8380aecfbe9fdece64a0613aa8b535711` | same |

The only Cargo adaptation makes the inherited `version`/`edition` explicit and
adds an independent `[workspace]` with `resolver = "3"`. `src/lib.rs` is
byte-identical: all types, `MAGIC`, `VERSION`, capacities, `repr(C, align(64))`
header layout, atomic orderings, partial progress, EOF drain, broken-pipe and
pending behavior, assertions, and the concurrent 1 MiB test are retained. The
three inline tests were not disabled.

## Lock

The source root lock has sha256
`8a422bdc85ad1f0a85af5cd627783d240a61acefdd4cb6c98cbaada480666f57`; it was not
copied. The candidate has a newly generated one-package `Cargo.lock` with
sha256 `95dde6f084297803c266a45c47a5ca3fb834e6365eb26a0f293ec25e0f77fb21`
before and after the eight verification commands. The copied `.gitignore` does
not ignore this lock, so the root lock is present for tracking.

## Verification

Run from the candidate with
`CARGO_TARGET_DIR=/home/anreonyr/Develop/shuyra/target/shared-build/pr16-provider`.
Raw commands, logs, and exit files are under `evidence/pr16/provider` in the
workspace.

| # | command | exit | result |
|---|---|---|---|
| 1 | `cargo build --lib --target x86_64-unknown-linux-gnu --locked` | 0 | host library built |
| 2 | `cargo build --lib --target riscv64gc-unknown-none-elf --locked` | 0 | RV64 debug library built |
| 3 | `cargo build --lib --release --target riscv64gc-unknown-none-elf --locked` | 0 | RV64 release library built |
| 4 | `cargo test --target x86_64-unknown-linux-gnu --locked` | 0 | 3 passed, 0 failed |
| 5 | `cargo build --lib --target x86_64-unknown-linux-gnu --locked --offline` | 0 | host library built offline |
| 6 | `cargo build --lib --target riscv64gc-unknown-none-elf --locked --offline` | 0 | RV64 debug offline |
| 7 | `cargo build --lib --release --target riscv64gc-unknown-none-elf --locked --offline` | 0 | RV64 release offline |
| 8 | `cargo test --target x86_64-unknown-linux-gnu --locked --offline` | 0 | 3 passed, 0 failed |

An initial invocation accidentally ran from the workspace root rather than the
candidate. All eight commands exited 101 with `could not find Cargo.toml`; the
raw commands, logs, and exits are preserved in `attempt-cwd` and are not gate
results. The candidate rerun above is the actual verification.

## Boundary

This is a local provider snapshot only. No public clone, repository creation,
commit, push, consumer edit, QEMU run, or Stage 3/4 completion is claimed. The
consumer phase remains deferred until the provider has a public full SHA and the
root passes an independent public clean-clone gate.
