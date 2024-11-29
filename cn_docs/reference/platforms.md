# 平台支持

uv 对以下平台提供一级支持：

- macOS（Apple Silicon）
- macOS（x86_64）
- Linux（x86_64）
- Windows（x86_64）

uv 会持续在其一级平台上进行构建、测试和开发。受 Rust 项目启发，一级平台可以被视为 [“保证可用”](https://doc.rust-lang.org/beta/rustc/platform-support.html)。

uv 对以下平台提供二级支持 [“保证可以构建”](https://doc.rust-lang.org/beta/rustc/platform-support.html)：

- Linux（PPC64）
- Linux（PPC64LE）
- Linux（aarch64）
- Linux（armv7）
- Linux（i686）
- Linux（s390x）

uv 会为其一级和二级平台发布预构建的 wheel 文件至 [PyPI](https://pypi.org/project/uv/)。然而，虽然二级平台会持续构建，但它们并未持续进行测试或开发，因此实际稳定性可能会有所不同。

除了一级和二级平台，uv 已知可以在 i686 Windows 上构建，但已知 _不能_ 在 aarch64 Windows 上构建，然而目前并未将这两个平台视为已支持平台。最低支持的 Windows 版本为 Windows 10 和 Windows Server 2016，遵循 [Rust 自身的一级支持](https://blog.rust-lang.org/2024/02/26/Windows-7.html)。

uv 支持并已在以下 Python 版本上进行测试：3.8、3.9、3.10、3.11、3.12 和 3.13。