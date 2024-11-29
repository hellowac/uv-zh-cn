# 获取帮助

## 帮助菜单

可以使用 `--help` 标志查看某个命令的帮助菜单，例如，对于 `uv`：

```console
$ uv --help
```

要查看特定命令的帮助菜单，例如，对于 `uv init`：

```console
$ uv init --help
```

使用 `--help` 标志时，uv 会显示一个简洁的帮助菜单。要查看更详细的帮助菜单，可以使用 `uv help`：

```console
$ uv help
```

要查看特定命令的详细帮助菜单，例如，对于 `uv init`：

```console
$ uv help init
```

使用详细帮助菜单时，uv 会尝试使用 `less` 或 `more` 来分页输出，避免一次性显示所有内容。要退出分页器，请按 `q`。

## 查看版本

在寻求帮助时，了解您正在使用的 uv 版本非常重要——有时候问题可能已经在更新的版本中解决。

查看已安装的版本：

```console
$ uv version
```

以下命令也有效：

```console
$ uv --version      # 与 `uv version` 输出相同
$ uv -V             # 不包括构建提交和日期
$ uv pip --version  # 可与子命令一起使用
```

## 在 GitHub 上报告问题

[GitHub 问题追踪器](https://github.com/astral-sh/uv/issues)是报告 bug 和请求功能的好地方。在报告问题之前，请先搜索相似的问题，因为其他人可能已经遇到过相同的问题。

## 在 Discord 上聊天

Astral 有一个 [Discord 服务器](https://discord.com/invite/astral-sh)，这是一个提问、了解更多关于 uv 的信息以及与其他社区成员互动的好地方。