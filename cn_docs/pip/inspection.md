# 检查环境

## 列出已安装的包

列出环境中所有已安装的包：

```console
$ uv pip list
```

以 JSON 格式列出包：

```console
$ uv pip list --format json
```

以 `requirements.txt` 格式列出环境中所有包：

```console
$ uv pip freeze
```

## 检查包信息

查看已安装包的信息，例如查看 `numpy` 的信息：

```console
$ uv pip show numpy
```

也可以一次检查多个包。

## 验证环境

如果在多个步骤中安装了包，可能会将具有冲突要求的包安装到同一个环境中。

检查环境中是否有冲突或缺失的依赖项：

```console
$ uv pip check
```
