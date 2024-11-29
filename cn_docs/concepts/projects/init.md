# 创建项目

uv 支持使用 `uv init` 创建项目。

在创建项目时，uv 提供了两种基本模板： [**应用程序**](#applications) 和 [**库**](#libraries)。默认情况下，uv 会创建一个应用程序项目。可以使用 `--lib` 标志创建一个库项目。

## 目标目录

uv 会在工作目录中创建项目，或者通过提供名称在目标目录中创建项目，例如：`uv init foo`。如果目标目录中已经存在项目（即存在 `pyproject.toml` 文件），uv 会报错并退出。

## 应用程序 {: #applications}

应用程序项目适用于 Web 服务器、脚本和命令行接口。

应用程序是 `uv init` 的默认目标，但也可以使用 `--app` 标志指定：

```console
$ uv init example-app
```

该项目包括一个 `pyproject.toml` 文件，一个示例文件（`hello.py`），一个 README 文件，以及一个 Python 版本锁定文件（`.python-version`）。

```console
$ tree example-app
example-app
├── .python-version
├── README.md
├── hello.py
└── pyproject.toml
```

`pyproject.toml` 包含基本的元数据。它不包括构建系统，因此不是一个 [包](./config.md#project-packaging)，也不会安装到环境中：

```toml title="pyproject.toml"
[project]
name = "example-app"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []
```

示例文件定义了一个 `main` 函数和一些标准模板：

```python title="hello.py"
def main():
    print("Hello from example-app!")


if __name__ == "__main__":
    main()
```

可以使用 `uv run` 来执行 Python 文件：

```console
$ uv run hello.py
Hello from example-project!
```

## 打包应用程序 {: #packaged-applications}

许多使用场景需要一个 [包](./config.md#project-packaging)。例如，如果你正在创建一个将发布到 PyPI 的命令行接口，或者你想在专用目录中定义测试。

可以使用 `--package` 标志来创建一个打包应用程序：

```console
$ uv init --package example-pkg
```

源代码会被移动到一个 `src` 目录下，并包含一个模块目录和 `__init__.py` 文件：

```console
$ tree example-pkg
example-pkg
├── .python-version
├── README.md
├── pyproject.toml
└── src
    └── example_packaged_app
        └── __init__.py
```

一个 [构建系统](./config.md#build-systems) 被定义，这样项目将被安装到环境中：

```toml title="pyproject.toml" hl_lines="12-14"
[project]
name = "example-pkg"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[project.scripts]
example-pkg = "example_packaged_app:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

!!! tip

    可以使用 `--build-backend` 选项请求使用不同的构建系统。

包含了一个 [命令](./config.md#entry-points) 定义：

```toml title="pyproject.toml" hl_lines="9 10"
[project]
name = "example-pkg"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[project.scripts]
example-pkg = "example_packaged_app:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

可以使用 `uv run` 来执行该命令：

```console
$ uv run --directory example-pkg example-pkg
Hello from example-pkg!
```

## 库 {: #libraries}

库提供了供其他项目使用的函数和对象。库通常用于构建和分发，例如，通过将其上传到 PyPI。

可以使用 `--lib` 标志来创建一个库项目：

```console
$ uv init --lib example-lib
```

!!! note

    使用 `--lib` 相当于 `--package`。库项目总是需要打包的项目。

与 [打包应用程序](#packaged-applications) 相同，库使用 `src` 结构。还包括一个 `py.typed` 标记，以指示给使用者，库的类型信息是可读取的：

```console
$ tree example-lib
example-lib
├── .python-version
├── README.md
├── pyproject.toml
└── src
    └── example_lib
        ├── py.typed
        └── __init__.py
```

!!! note

    `src` 结构在开发库时特别有价值。它确保库代码与项目根目录中的任何 `python` 调用隔离开来，并且分发的库代码与其他项目源代码良好分离。

定义了一个 [构建系统](./config.md#build-systems)，这样项目就可以安装到环境中：

```toml title="pyproject.toml" hl_lines="12-14"
[project]
name = "example-lib"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

!!! tip

    你可以使用 `--build-backend` 选项选择不同的构建后端模板，支持 `hatchling`、`flit-core`、`pdm-backend`、`setuptools`、`maturin` 或 `scikit-build-core`。如果你想创建一个 [带扩展模块的库](#projects-with-extension-modules)，则需要使用其他后端。

创建的模块定义了一个简单的 API 函数：

```python title="__init__.py"
def hello() -> str:
    return "Hello from example-lib!"
```

你可以使用 `uv run` 导入并执行它：

```console
$ uv run --directory example-lib python -c "import example_lib; print(example_lib.hello())"
Hello from example-lib!
```

## 带扩展模块的项目 {: #projects-with-extension-modules}

大多数 Python 项目是“纯 Python”，意味着它们不定义如 C、C++、FORTRAN 或 Rust 等其他语言的模块。然而，带有扩展模块的项目通常用于性能敏感的代码。

创建带有扩展模块的项目需要选择一个替代的构建系统。uv 支持使用以下构建系统来创建带扩展模块的项目：

- [`maturin`](https://www.maturin.rs) 用于 Rust 项目
- [`scikit-build-core`](https://github.com/scikit-build/scikit-build-core) 用于 C、C++、FORTRAN、Cython 项目

使用 `--build-backend` 标志指定构建系统：

```console
$ uv init --build-backend maturin example-ext
```

!!! note

    使用 `--build-backend` 相当于 `--package`。

该项目除了典型的 Python 项目文件外，还包含一个 `Cargo.toml` 和一个 `lib.rs` 文件：

```console
$ tree example-ext
example-ext
├── .python-version
├── Cargo.toml
├── README.md
├── pyproject.toml
└── src
    ├── lib.rs
    └── example_ext
        ├── __init__.py
        └── _core.pyi
```

!!! note

    如果使用 `scikit-build-core`，你将看到 CMake 配置文件和 `main.cpp` 文件。

Rust 库定义了一个简单的函数：

```rust title="src/lib.rs"
use pyo3::prelude::*;

#[pyfunction]
fn hello_from_bin() -> String {
    "Hello from example-ext!".to_string()
}

#[pymodule]
fn _core(m: &Bound<'_, PyModule>) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(hello_from_bin, m)?)?;
    Ok(())
}
```

Python 模块导入并使用该函数：

```python title="src/example_ext/__init__.py"
from example_ext._core import hello_from_bin

def main() -> None:
    print(hello_from_bin())
```

可以使用 `uv run` 执行该命令：

```console
$ uv run --directory example-ext example-ext
Hello from example-ext!
```

!!! important

    如果更改了 `lib.rs` 或 `main.cpp` 中的扩展代码，需要运行 `--reinstall` 以重新构建它们。
