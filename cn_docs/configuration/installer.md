# 配置 uv 安装器

## 更改安装路径

默认情况下，uv 会安装到 `~/.local/bin`。如果设置了 `XDG_BIN_HOME`，则会使用该路径代替。类似地，如果设置了 `XDG_DATA_HOME`，目标目录将推断为 `XDG_DATA_HOME/../bin`。

要更改安装路径，可以使用 `UV_INSTALL_DIR`：

=== "macOS 和 Linux"

    ```console
    $ curl -LsSf https://astral.sh/uv/install.sh | env UV_INSTALL_DIR="/custom/path" sh
    ```

=== "Windows"

    ```powershell
    $env:UV_INSTALL_DIR = "C:\Custom\Path" powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```

## 禁用 Shell 配置修改

安装程序还可能更新你的 shell 配置文件，以确保 uv 二进制文件在 `PATH` 中。如果希望禁用此行为，可以使用 `INSTALLER_NO_MODIFY_PATH`。例如：

```console
$ curl -LsSf https://astral.sh/uv/install.sh | env INSTALLER_NO_MODIFY_PATH=1 sh
```

如果使用 `INSTALLER_NO_MODIFY_PATH` 安装，之后的操作（如 `uv self update`）将不会修改你的 shell 配置文件。

## 非托管安装

在短暂环境（如 CI）中，可以使用 `UV_UNMANAGED_INSTALL` 将 uv 安装到特定路径，并防止安装程序修改 shell 配置文件或环境变量：

```console
$ curl -LsSf https://astral.sh/uv/install.sh | env UV_UNMANAGED_INSTALL="/custom/path" sh
```

使用 `UV_UNMANAGED_INSTALL` 还会禁用自我更新（通过 `uv self update`）。

## 传递选项给安装脚本

建议使用环境变量，因为它们在各个平台间是一致的。然而，也可以直接传递选项给安装脚本。例如，要查看可用的选项：

```console
$ curl -LsSf https://astral.sh/uv/install.sh | sh -s -- --help
```
