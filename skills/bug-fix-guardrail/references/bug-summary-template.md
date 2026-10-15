# Bug 标题与摘要索引模板

将以下结构追加到用户级 `bug-summaries.md`，不要放入项目目录。每个 Bug 只保留一个摘要条目；调整时更新该条目，不重复创建。

```md
# Bug 标题与摘要索引

## BUG-YYYYMMDD-NNN：简短标题

- 摘要：用一至两句话概括现象、触发条件和影响
- 仓库唯一标识：remote-<64位哈希> / path-<64位哈希>
- 关键词：模块名、类名、方法名、接口、表名、异常、业务字段
- 模块/文件：受影响模块或关键文件
- 状态：已修复 / 部分修复 / 待验证 / 未解决
- 发现日期：YYYY-MM-DD
- 最近更新：YYYY-MM-DD
- 详细记录：[bug-fixes.md#bug-yyyymmdd-nnn-简短标题](bug-fixes.md#bug-yyyymmdd-nnn-简短标题)
```
