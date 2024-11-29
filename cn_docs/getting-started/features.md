# 功能概览

uv 提供了丰富的功能，涵盖了从安装 Python 和运行简单脚本，到支持多 Python 版本和多平台的大型项目开发的各个方面。

uv 的功能可以分为多个模块，您可以根据需要单独或组合使用这些模块。

---

## Python 版本管理

安装和管理 Python 版本。

- `uv python install`：安装 Python 版本。
- `uv python list`：查看可用的 Python 版本。
- `uv python find`：查找已安装的 Python 版本。
- `uv python pin`：为当前项目指定使用的 Python 版本。
- `uv python uninstall`：卸载 Python 版本。

查看[安装 Python 指南](../guides/install-python.md)以快速入门。

---

## 脚本

运行独立的 Python 脚本（如 `example.py`）。

- `uv run`：运行脚本。
- `uv add --script`：为脚本添加依赖。
- `uv remove --script`：从脚本中移除依赖。

查看[运行脚本指南](../guides/scripts.md)以快速入门。

---

## 项目

创建并管理包含 `pyproject.toml` 的 Python 项目。

- `uv init`：创建一个新项目。
- `uv add`：向项目添加依赖。
- `uv remove`：从项目中移除依赖。
- `uv sync`：同步项目依赖到环境。
- `uv lock`：为项目依赖创建锁文件。
- `uv run`：在项目环境中运行命令。
- `uv tree`：查看项目的依赖树。
- `uv build`：构建项目的分发包。
- `uv publish`：将项目发布到包索引。

查看[项目指南](../guides/projects.md)以快速入门。

---

## 工具

运行和安装发布到 Python 包索引的工具（如 `ruff` 或 `black`）。

- `uvx` / `uv tool run`：在临时环境中运行工具。
- `uv tool install`：全局安装工具。
- `uv tool uninstall`：卸载工具。
- `uv tool list`：列出已安装的工具。
- `uv tool update-shell`：更新 shell，使工具可执行文件生效。

查看[工具指南](../guides/tools.md)以快速入门。

---

## pip 接口

手动管理环境和包，适用于需要精细控制的传统工作流或场景。

### 创建虚拟环境（替代 `venv` 和 `virtualenv`）

- `uv venv`：创建一个新的虚拟环境。

查看[使用环境的文档](../pip/environments.md)了解详情。

### 管理环境中的包（替代 [`pip`](https://github.com/pypa/pip) 和 [`pipdeptree`](https://github.com/tox-dev/pipdeptree)）

- `uv pip install`：安装包到当前环境。
- `uv pip show`：显示已安装包的详细信息。
- `uv pip freeze`：列出已安装包及其版本。
- `uv pip check`：检查当前环境中的包是否兼容。
- `uv pip list`：列出已安装的包。
- `uv pip uninstall`：卸载包。
- `uv pip tree`：查看环境的依赖树。

查看[管理包的文档](../pip/packages.md)了解详情。

### 锁定环境中的包（替代 [`pip-tools`](https://github.com/jazzband/pip-tools)）

- `uv pip compile`：将需求文件编译为锁文件。
- `uv pip sync`：根据锁文件同步环境。

查看[锁定环境的文档](../pip/compile.md)了解详情。

!!! important

    这些命令并不完全等同于它们所基于的工具的接口和行为。越是偏离常见的工作流，越可能遇到差异。详情请查阅 [pip 兼容性指南](../pip/compatibility.md)。

---

## 实用工具

管理和查看 uv 的状态，如缓存、存储目录，或执行自更新：

- `uv cache clean`：清理缓存条目。
- `uv cache prune`：清理过期的缓存条目。
- `uv cache dir`：显示 uv 缓存目录路径。
- `uv tool dir`：显示 uv 工具目录路径。
- `uv python dir`：显示 uv 安装的 Python 版本路径。
- `uv self update`：将 uv 更新到最新版本。

---

## 下一步

阅读[指南](../guides/index.md)以了解每个功能的入门内容，查阅[概念页面](../concepts/index.md)以深入了解 uv 的功能，或在遇到问题时学习如何[获取帮助](./help.md)。