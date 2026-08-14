<div align="center">

[English](README.en.md) | **简体中文**

</div>

<div align="center">

![superpowers-dsh](static/home.png)

</div>

# superpowers-dsh

为 **DeepSeek Harness (DSH)** 打造的 Superpowers 插件包：把
[obra/superpowers](https://github.com/obra/superpowers) 的核心技能
（Claude-Code 技能库：TDD、调试、规划、协作模式）移植到 DSH 的 Cordis
插件架构上。

插件会向 `ctx.skills` 注册表的 **host 层** 注册一个技能提供者，因此每个
agent preset 的作用域链都会合并这些技能。技能正文随包分发
（`skills/<name>/SKILL.md`），通过 `import.meta.url` 定位——这是包的
组装事实，不需要任何用户配置。

## 在 DeepSeek Harness 中安装与使用

这是 DeepSeek Harness 的**插件包**。安装后会把下面的 14 个技能注册进
host 技能注册表，你 profile 里的每个 agent 会话都能在技能目录中看到它们，
并可用 `skill` 工具加载。

### 前置条件

- 已安装 DeepSeek Harness，并且有 **pnpm** —— `dsh plugin` 命令内部调用
  pnpm（用 `pnpm --version` 检查；没有的话到 https://pnpm.io 安装）
- `dsh` 命令行。它随 Harness 一起提供，通常以 `npx @deepseek-ai/dsh web`
  方式启动，所以只在该命令的进程内可用。要么把它装成全局命令，要么在下面
  所有命令前加 `npx`：

  ```sh
  # 方式一：全局安装 dsh，永久可用（推荐）
  npm install -g @deepseek-ai/dsh
  dsh --version

  # 方式二：不安装，所有命令用 npx 形式
  npx @deepseek-ai/dsh --version
  ```

  下文所有 `dsh ...` 示例都可以等价写成 `npx @deepseek-ai/dsh ...`。

### 最推荐：直接让 DeepSeek Harness 帮你安装

打开 DeepSeek Harness（Web 界面），新建对话，把下面这句话发给它：

```
帮我安装这个链接里边的插件：https://github.com/LayneChai/superpowers-dsh
```

Agent 会自动完成安装（`dsh plugin --profile web add` → 重启 profile →
验证技能注册），无需你手动敲任何命令。装完后你可以在对话里让它运行
`dsh --profile web --dump-config`，确认输出里有 `superpowers-dsh` 行。

### 从 npm 安装（推荐，一条命令）

包已发布到 npm，名为 `superpowers-dsh`（国内会自动同步到 npmmirror 镜像）：

```sh
dsh plugin --profile web add superpowers-dsh
```

> 必须用 `dsh plugin` 形式——直接 `npm install superpowers-dsh` 只会把包当
> 普通库装到当前目录，**不会**注册进任何 DeepSeek Harness profile，技能
> 永远不会被加载。

### 从 GitHub 安装

```sh
# 任意目录下执行
dsh plugin --profile web add https://github.com/LayneChai/superpowers-dsh.git
```

### 从 tarball 或本地文件夹安装

```sh
# tarball（例如 Release 里的 superpowers-dsh-0.1.0.tgz）
dsh plugin --profile web add C:\路径\to\superpowers-dsh-0.1.0.tgz

# 或解压后的插件文件夹（pnpm 以链接方式安装，改完重启即生效）
dsh plugin --profile web add C:\路径\to\superpowers-dsh
```

### 重启并验证

bundle 层在 profile 启动时挂载，所以需要**重启 profile**（停掉后重新运行
`dsh web` / `npx @deepseek-ai/dsh web`，再刷新浏览器）。确认层已组合：

```sh
dsh --profile web --dump-config     # 必须出现 `superpowers-dsh` 行
```

之后技能会出现在 agent 技能目录中（`using-superpowers` 是入口技能），
可以用 `skill` 工具加载。

安装成功后的界面如下：

![安装成功截图](static/install-success.png)

### 在其他 profile（headless / tui / 自定义）中使用

把 `--profile` 指向你实际运行的 profile：

```sh
dsh plugin --profile headless add superpowers-dsh
dsh --profile headless --dump-config
```

### 卸载

```sh
dsh plugin --profile web remove superpowers-dsh
# 卸载后同样需要重启 profile
```

### 注意事项

- 国内用户可以先把 npm 镜像设为 npmmirror，让 `dsh plugin add
  superpowers-dsh` 更快：`npm config set registry https://registry.npmmirror.com`
- 从文件夹或 `file:` 规格安装的插件是链接安装（不是复制）：修改该文件夹后，
  下次重启 profile 生效
- 使用者**不需要 npm 账号，也不需要 2FA**——安装只是普通的包下载

## 技能列表

| 技能 | 用途 |
| --- | --- |
| `using-superpowers` | 如何查找和使用技能；入口技能 |
| `brainstorming` | 通过协作对话把想法变成设计 |
| `writing-plans` | 根据规格编写全面的实施计划 |
| `executing-plans` | 按书面计划执行，带评审检查点 |
| `subagent-driven-development` | 每个任务派发全新子代理并评审 |
| `dispatching-parallel-agents` | 把独立工作扇出到并行代理 |
| `systematic-debugging` | 先找根因的调试纪律 |
| `test-driven-development` | RED-GREEN-REFACTOR 实施循环 |
| `verification-before-completion` | 声称成功前先拿出证据 |
| `requesting-code-review` | 合并前获得严格评审 |
| `receiving-code-review` | 核实反馈，而不是盲目照做 |
| `finishing-a-development-branch` | 安全地整合已完成的工作 |
| `using-git-worktrees` | 功能开发的隔离工作区 |
| `writing-skills` | 以 TDD 方式编写并验证新技能 |

## 工作原理

- **Bundle 层** —— `cordis.patch.yml` 在 dsh-base 层之上插入一行
  （`- id: superpowers-dsh, name: superpowers-dsh`）。后面的层（profile 的
  `cordis.patch.yml`、`--patch` 叠加）仍可按 id 定位这一行。
- **提供者** —— `lib/index.js` 调用 `ctx.skills.registerProvider(...)`，
  注册一个提供者：
  - `list()` 扫描包内 `skills/` 目录，把每个 `<name>/SKILL.md` 作为候选，
    从 YAML frontmatter 解析出 `name`、`description`、`whenToUse`。
  - `get()` 按需读取候选技能正文，返回完整技能定义，`resourceBase` 指向
    技能所在目录，使相对引用（脚本、提示模板）可以正确解析。
- **零运行时依赖** —— 插件只使用 Node 内置模块，消费注入的 `ctx.skills`
  服务接口。

## 移植说明（对比上游 obra/superpowers）

- 去掉了命名空间前缀：`superpowers:brainstorming` → `brainstorming`
  （DSH 技能用裸名称寻址）。
- `using-superpowers` 现在介绍 DSH 的 `skill` 工具，并指向
  `skills/using-superpowers/references/dsh-tools.md`——完整的
  Claude-Code → DSH 工具映射（`pwsh`、`subagent`、`workflow`、`goal` ...）。
- 子代理引用映射到 DSH 的 `subagent` / `subagent_fork` 工具。
- `brainstorming` 的视觉伴侣补充了 Windows 说明：Node 服务
  （`scripts/server.cjs`）全平台可跑；`.sh` 辅助脚本仅限 bash。

## 添加自己的技能

往包里放一个新的 `skills/<kebab-name>/SKILL.md` 即可——它必须以 YAML
frontmatter 开头（`name` + `description`，可选 `whenToUse`）。无需改任何
代码：`list()` 会自动发现它。

## 许可证

MIT。技能内容改编自
[obra/superpowers](https://github.com/obra/superpowers)（MIT），© Jesse Vincent
及贡献者。
