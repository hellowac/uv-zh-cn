# pip 接口

uv 提供了一个替代方案，可以替代常用的 `pip`、`pip-tools` 和 `virtualenv` 命令。这些命令直接与虚拟环境交互，与 uv 的主要接口不同，后者会自动管理虚拟环境。`uv pip` 接口将 uv 的速度和功能暴露给高级用户和尚未准备好过渡到 uv 的项目，供它们继续使用 `pip` 和 `pip-tools`。

以下部分讨论了使用 `uv pip` 的基础知识：

- [创建和使用环境](./environments.md)
- [安装和管理包](./packages.md)
- [检查环境和包](./inspection.md)
- [声明包依赖](./dependencies.md)
- [锁定和同步环境](./compile.md)

请注意，这些命令并不 _完全_ 实现它们所基于工具的接口和行为。越是偏离常见工作流程，遇到差异的可能性就越大。有关详细信息，请参阅 [pip 兼容性指南](./compatibility.md)。

!!! important

    uv 不依赖也不调用 pip。将其命名为 pip 接口，是为了突出其提供与 pip 接口匹配的底层命令的专用目的，并将其与 uv 的其他命令区分开来，后者在更高抽象层次上运行。