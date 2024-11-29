# 安装 uv

## 安装方法

可以通过独立安装程序或您选择的包管理工具安装 uv。

### 独立安装程序

uv 提供独立安装程序，方便下载和安装：

=== "macOS 和 Linux"

    使用 `curl` 下载脚本并通过 `sh` 执行：
    
    ```console
    $ curl -LsSf https://astral.sh/uv/install.sh | sh
    ```
    
    如果您的系统没有安装 `curl`，可以使用 `wget`：
    
    ```console
    $ wget -qO- https://astral.sh/uv/install.sh | sh
    ```
    
    指定特定版本时，在 URL 中包含版本号：
    
    ```console
    $ curl -LsSf https://astral.sh/uv/0.5.5/install.sh | sh
    ```

=== "Windows"

    使用 `irm` 下载脚本并通过 `iex` 执行：
    
    ```console
    $ powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```
    
    更改 [执行策略](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4#powershell-execution-policies) 后即可运行来自互联网的脚本。
    
    指定特定版本时，在 URL 中包含版本号：
    
    ```console
    $ powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.5.5/install.ps1 | iex"
    ```

!!! tip

    安装脚本可在使用前查看内容：

    === "macOS 和 Linux"

        ```console
        $ curl -LsSf https://astral.sh/uv/install.sh | less
        ```

    === "Windows"

        ```console
        $ powershell -c "irm https://astral.sh/uv/install.ps1 | more"
        ```

    或者，您可以直接从 [GitHub](#github-releases) 下载安装程序或二进制文件。

有关自定义 uv 安装的详细信息，请参阅 [安装程序配置文档](../configuration/installer.md)。

### PyPI

为方便起见，uv 已发布至 [PyPI](https://pypi.org/project/uv/)。

从 PyPI 安装时，建议将 uv 安装到隔离环境中，例如使用 `pipx`：

```console
$ pipx install uv
```

当然，也可以直接使用 `pip`：

```console
$ pip install uv
```

!!! note

    uv 提供了许多平台的预构建分发包（wheels）。如果某个平台没有可用的 wheel，uv 将从源码构建，这需要 Rust 工具链。  
    有关从源码构建 uv 的详细信息，请参阅 [贡献者设置指南](https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#setup)。  

### Cargo

可以通过 Cargo 安装 uv，但由于依赖未发布的 crates，必须从 Git 构建，而不是 [crates.io](https://crates.io)。

```console
$ cargo install --git https://github.com/astral-sh/uv uv
```

### Homebrew

uv 已包含在 Homebrew 的核心包中。

```console
$ brew install uv
```

### Winget

可以通过 [winget](https://winstall.app/apps/astral-sh.uv) 安装 uv。

```console
$ winget install --id=astral-sh.uv -e
```

### Docker

uv 提供了一个 Docker 镜像，托管在  
[`ghcr.io/astral-sh/uv`](https://github.com/astral-sh/uv/pkgs/container/uv)。

请参阅我们的 [Docker 集成指南](../guides/integration/docker.md) 了解更多详细信息。

### GitHub Releases

uv 的发布文件可以直接从 [GitHub Releases](https://github.com/astral-sh/uv/releases) 下载。

每个发布页面都包含所有支持平台的二进制文件，以及通过 `github.com` 而非 `astral.sh` 使用独立安装程序的说明。

---

## 更新 uv

如果通过独立安装程序安装 uv，可以按需自我更新：

```console
$ uv self update
```

!!! tip

    更新 uv 时会重新运行安装程序，并可能修改您的 shell 配置文件。  
    如果想禁用此行为，可设置环境变量 `INSTALLER_NO_MODIFY_PATH=1`。

使用其他安装方法时，自动更新功能会被禁用。请使用相应包管理工具的升级方法，例如使用 `pip`：

```console
$ pip install --upgrade uv
```

---

## Shell 自动补全

要为 uv 命令启用 shell 自动补全，请运行以下命令之一：

=== "Linux 和 macOS"

    ```bash
    # 确定您的 shell 类型（例如 `echo $SHELL`），然后运行以下之一：
    echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc
    echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
    echo 'uv generate-shell-completion fish | source' >> ~/.config/fish/config.fish
    echo 'eval (uv generate-shell-completion elvish | slurp)' >> ~/.elvish/rc.elv
    ```

=== "Windows"

    ```powershell
    Add-Content -Path $PROFILE -Value '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'
    ```

---

要为 uvx 启用 shell 自动补全，请运行以下命令之一：

=== "Linux 和 macOS"

    ```bash
    # 确定您的 shell 类型（例如 `echo $SHELL`），然后运行以下之一：
    echo 'eval "$(uvx --generate-shell-completion bash)"' >> ~/.bashrc
    echo 'eval "$(uvx --generate-shell-completion zsh)"' >> ~/.zshrc
    echo 'uvx --generate-shell-completion fish | source' >> ~/.config/fish/config.fish
    echo 'eval (uvx --generate-shell-completion elvish | slurp)' >> ~/.elvish/rc.elv
    ```

=== "Windows"

    ```powershell
    Add-Content -Path $PROFILE -Value '(& uvx --generate-shell-completion powershell) | Out-String | Invoke-Expression'
    ```

最后，重新启动 shell 或重新加载 shell 配置文件即可。

## 卸载

如果需要从系统中移除 uv，只需删除 `uv` 和 `uvx` 二进制文件：

=== "macOS 和 Linux"

    ```console
    $ rm ~/.local/bin/uv ~/.local/bin/uvx
    ```

=== "Windows"

    ```powershell
    $ rm $HOME\.local\bin\uv.exe
    $ rm $HOME\.local\bin\uvx.exe
    ```

!!! tip

    在移除二进制文件之前，您可能需要清理 uv 存储的数据：

    ```console
    $ uv cache clean
    $ rm -r "$(uv python dir)"
    $ rm -r "$(uv tool dir)"
    ```

!!! note

    在 0.5.0 版本之前，uv 是安装在 `~/.cargo/bin` 中的。可以从该路径中移除二进制文件以完成卸载。  
    从旧版本升级时不会自动删除 `~/.cargo/bin` 中的二进制文件。

---

## 下一步

查看 [初始步骤](./first-steps.md) 或直接跳转到 [指南](../guides/index.md) 开始使用 uv。
