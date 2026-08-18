# superpowers-dsh-obra

## 注意事项原仓库地址
本仓库fork与 https://github.com/LayneChai/superpowers-dsh 
由于本人不想省略superpowers而fork


### 最简单：一条命令

不需要先全局安装 `dsh`，在任意目录执行：

```sh
npx @deepseek-ai/dsh plugin --profile web add github:lyf2nb/superpowers-dsh-obra
```

装完后重启 `npx @deepseek-ai/dsh web`（或 `dsh web`），刷新浏览器即可。

### 最推荐：直接让 DeepSeek Harness 帮你安装

打开 DeepSeek Harness（Web 界面），新建对话，把下面这句话发给它：

```
帮我安装这个链接里边的插件：https://github.com/lyf2nb/superpowers-dsh-obra
```

Agent 会自动完成安装（`dsh plugin --profile web add` → 重启 profile →
验证技能注册），无需你手动敲任何命令。装完后你可以在对话里让它运行
`dsh --profile web --dump-config`，确认输出里有 `superpowers-dsh` 行。

### 卸载

```sh
dsh plugin --profile web remove superpowers-dsh-obra
# 卸载后同样需要重启 profile
```

## 技能列表

| 技能 | 用途 |
| --- | --- |
| `superpowers-using-superpowers` | 如何查找和使用技能；入口技能 |
| `superpowers-brainstorming` | 通过协作对话把想法变成设计 |
| `superpowers-writing-plans` | 根据规格编写全面的实施计划 |
| `superpowers-executing-plans` | 按书面计划执行，带评审检查点 |
| `superpowers-subagent-driven-development` | 每个任务派发全新子代理并评审 |
| `superpowers-dispatching-parallel-agents` | 把独立工作扇出到并行代理 |
| `superpowers-systematic-debugging` | 先找根因的调试纪律 |
| `superpowers-test-driven-development` | RED-GREEN-REFACTOR 实施循环 |
| `superpowers-verification-before-completion` | 声称成功前先拿出证据 |
| `superpowers-requesting-code-review` | 合并前获得严格评审 |
| `superpowers-receiving-code-review` | 核实反馈，而不是盲目照做 |
| `superpowers-finishing-a-development-branch` | 安全地整合已完成的工作 |
| `superpowers-using-git-worktrees` | 功能开发的隔离工作区 |
| `superpowers-writing-skills` | 以 TDD 方式编写并验证新技能 |

## 注意事项原仓库地址
本仓库fork与 https://github.com/LayneChai/superpowers-dsh 
由于本人不想省略superpowers而fork
## 许可证

MIT。技能内容改编自
[obra/superpowers](https://github.com/obra/superpowers)（MIT），© Jesse Vincent
及贡献者。
