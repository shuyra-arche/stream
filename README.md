# stream

Shuyra 的 `no_std`、无外部依赖的单生产者/单消费者环形缓冲机制。
`src/lib.rs` 定义共享 Header、原子字节环、读写结果与关闭操作，并包含宿主测试。

调用者负责存储、映射、端点生命周期、唤醒、调度与重试。
读写允许部分进展与 Pending；写端关闭后读端排空返回 EOF，读端关闭后写入返回 BrokenPipe。
这是一项库机制，不能单独作为端点或服务运行。

## 工具链与维护

使用 `rust-toolchain.toml` 固定的 `nightly-2026-09-04`，首次运行需要 Rustup
下载工具链及 RV64 目标。依赖版本以 Cargo 清单和锁文件为准。
下列命令从仓库根目录执行；宿主测试不代表真实内核运行验证。

```sh
cargo build --lib --target riscv64gc-unknown-none-elf --locked
cargo test --target x86_64-unknown-linux-gnu --locked
```

## 贡献与来源

维护与 PR 规则见 [Shuyra 贡献规范](https://github.com/shuyra-arche/vanllia/blob/main/CONTRIBUTING.md)。
来源、作者与许可情况见 [PROVENANCE.md](PROVENANCE.md)。
