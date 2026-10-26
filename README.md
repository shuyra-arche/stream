# stream

`no_std`, dependency-free safe SPSC ring snapshot extracted from
[`anreonyr/sqware`](https://github.com/anreonyr/sqware.git) at commit
`68e7afb7416f8008d5ed39cbc1daa53aa2b5d087` (public source branch
`codex/shuyra-ipc-extraction`, PR38). The local extraction branch is
`codex/shuyra-stream-extraction`; the full history remains in the original
repository.

## Contents

- `src/lib.rs` — the 279-line production file, byte-identical to
  `crates/stream/src/lib.rs` at the source commit. Its three inline library
  tests remain enabled.
- `Cargo.toml` — explicit `stream` package, version 0.1.0, edition 2024, with an
  independent `[workspace]` and `resolver = "3"`. No dependencies,
  dev-dependencies, build script, package rename, or extra unit file.
- `Cargo.lock` — fresh initial lock for this one-package workspace, not copied
  from the monorepo.
- `rust-toolchain.toml` and `.gitignore` — copied from the source root.

The caller retains storage, mapping, endpoint lifetime, wakeup, scheduling, and
retry. This provider is only the SPSC header/ring mechanism: a library, not a
service or a fake endpoint.

## Verification

```sh
cargo build --lib --target x86_64-unknown-linux-gnu --locked
cargo build --lib --target riscv64gc-unknown-none-elf --locked
cargo build --lib --release --target riscv64gc-unknown-none-elf --locked
cargo test --target x86_64-unknown-linux-gnu --locked
```

The same four commands were repeated with `--locked --offline` after the cache
was prepared. All eight exited 0; both host test runs reported 3 passed. See
`PROVENANCE.md` for the source mapping, lock facts, and deferred consumer gate.
