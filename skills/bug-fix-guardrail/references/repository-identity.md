# Bug 记录仓库标识规范

## 读取来源

在 Git 仓库中：

1. 使用 `git rev-parse --show-toplevel` 获取当前工作树对应的 Git 根目录。
2. 使用 `git config --local --get-regexp '^remote\..*\.url$'` 读取本地 Git 配置的远程 URL。该命令可正确处理 `.git` 目录和 worktree 使用的 `.git` 文件。
3. 优先选择 remote 名为 `origin` 的 URL；同一 remote 有多个 URL 时，全部规范化后取字典序最小值。没有 `origin` 时，在所有 remote URL 中取规范化结果字典序最小值。

不要直接把 `.git` 文件中的 `gitdir: ...` 当成远程地址。远程地址通常位于实际 Git 配置的 `[remote "..."]` 节点中。

## 规范化规则

将 URL 转换为稳定的 `主机[:非默认端口]/仓库路径` 文本：

- 去除首尾空白、查询参数和片段；
- 对 `git@host:owner/repo.git` 形式转换为 `host/owner/repo.git`；
- 对 `https://user:token@host/owner/repo.git`、`ssh://user@host/owner/repo.git` 等形式去除协议、用户名和凭证；
- 主机名转小写；保留非默认端口；路径统一使用 `/`，去除重复的末尾 `/`；
- 仅去除路径末尾的 `.git` 后缀，保留仓库路径其他大小写和内容；
- 若 URL 是本地路径、格式无法识别或包含无法安全脱敏的内容，则该 URL 不参与候选，继续选择其他远程 URL。

对规范化文本的 UTF-8 字节计算完整 SHA-256，使用小写十六进制表示：

```text
remote-<64位小写十六进制哈希>
```

例如，以下地址应得到同一个规范化值：

```text
https://github.com/acme/payments.git
git@github.com:acme/payments.git
ssh://developer@github.com/acme/payments
```

## 无远程 URL 的兜底

若当前 Git 仓库没有可用远程 URL，或当前目录不是 Git 仓库：

- Git 仓库使用 `git rev-parse --show-toplevel` 的规范化绝对路径；
- 非 Git 目录使用当前工作区的规范化绝对路径；
- Windows 路径统一盘符大小写、斜杠和大小写规则，其他系统使用规范化绝对路径；
- 对路径文本计算 SHA-256，目录名使用 `path-<64位小写十六进制哈希>`。

## 旧目录兼容

旧版本目录名是把 Git 根目录或工作区绝对路径中的特殊字符替换为 `_` 的路径字符串。切换到远程 URL 标识后：

- 新目录不存在时，可先读取当前根目录对应的旧目录；
- 只有旧记录本身明确写有相同的规范化远程标识时，才可将其按 Bug 编号增量复制到新目录；
- 合并时保留历史内容和原编号；不同旧目录出现相同编号但内容不同的记录时，必须分配新唯一编号，并在新记录中同时注明原始编号和来源目录，不能以“信息更完整”为由丢弃另一条记录；
- 不删除、重命名或覆盖旧目录；无法确认归属时保留旧目录并报告待人工迁移。
