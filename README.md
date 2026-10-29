# iDock CLI

通过终端或 AI Agent 使用 iDock：查询录音、读取转写和会议纪要、搜索个人 RAG 知识库、下载音频、导出文档，以及导入本地音频并跟踪转写任务。

| 项目 | 名称 |
| --- | --- |
| npm 包 | [`@gdpisen/idock-cli`](https://www.npmjs.com/package/@gdpisen/idock-cli) |
| 终端命令 | `idock` |
| Agent Skill | `idock-cli` |
| 运行环境 | Node.js 22 或以上，Windows / macOS / Linux |
| 登录方式 | 用户个人 AccessKey |

## 直接让 AI 帮你安装

把下面这段提示词发送给具有终端执行能力的 Trae、Codex 或其它 AI Agent：

```text
请帮我安装 iDock CLI（idock），项目地址：
https://github.com/mu-qiao/idock-cli

请先阅读 README 中的安装和 Agent Skill 配置说明，在当前 Agent 实际
执行命令的环境安装 @gdpisen/idock-cli，并配置当前 AI 的 idock-cli Skill。
已有同名 Skill 时先比较内容，保护个人定制。

安装完成后检查登录状态。如果尚未登录，引导我在自己的交互终端执行
idock auth login，隐藏输入个人 AccessKey，不要让我把 Key 发到聊天中。
如果你的工具没有交互终端，先完成安装，再告诉我手动登录命令，
等我登录后继续验证。已有有效登录时直接复用。

最后检查 CLI 版本、Skill 是否加载、idock auth status 和 idock doctor，
报告实际结果；需要用户继续操作的步骤请明确列出。
本次只做安装与验证，不上传音频、重置或撤销 Key。
```

AI 需要具备终端工具才能执行安装。CLI 和 Skill 应安装在它实际执行命令的环境；远程开发、WSL、容器中的安装和登录与本机分别管理。

## 1. 安装 CLI

先检查 Node.js 版本，再安装公开 npm 包：

```sh
node --version
npm install -g @gdpisen/idock-cli --registry=https://registry.npmjs.org/
idock --version
idock --help
```

服务器地址由发布者配置在包内，普通用户无需输入服务器地址或用户 ID。本仓库提供安装和使用说明，CLI 及完整 Skill 文件通过 npm 包分发，安装时无需克隆服务端仓库。

Windows PowerShell 如果拦截 `npm.ps1` 或 `idock.ps1`，分别使用 `npm.cmd` 和 `idock.cmd`，无需修改系统执行策略。例如：

```powershell
npm.cmd install -g @gdpisen/idock-cli --registry=https://registry.npmjs.org/
idock.cmd --version
```

如果以前安装过旧的无 scope 包 `idock-cli`，遇到同名命令冲突时，可先执行 `npm uninstall -g idock-cli`，再安装 `@gdpisen/idock-cli`。不需要用 `--force` 覆盖其它包的命令入口。

## 2. 配置 Agent Skill

Skill 随 npm 包提供。先用 `npm root -g` 找到全局模块目录，包路径为该目录下的 `@gdpisen/idock-cli`，安装脚本为 `scripts/install-skill.mjs`。

### Trae

在 Trae 打开的项目目录中执行。

Windows PowerShell：

```powershell
$idockPackage = Join-Path ((npm.cmd root -g).Trim()) '@gdpisen/idock-cli'
node (Join-Path $idockPackage 'scripts/install-skill.mjs') --target .\.trae\skills
```

macOS / Linux：

```sh
node "$(npm root -g)/@gdpisen/idock-cli/scripts/install-skill.mjs" --target ./.trae/skills
```

在 Trae 的“设置 → 技能与命令”中确认项目技能 `idock-cli` 被识别且启用。新增技能未出现时，检查打开的项目目录并重新加载窗口。使用具有终端执行能力的 Agent。宿主设置见 [Trae 官方技能文档](https://docs.trae.com.cn/ide/skills)。

### Codex

Windows PowerShell：

```powershell
$idockPackage = Join-Path ((npm.cmd root -g).Trim()) '@gdpisen/idock-cli'
node (Join-Path $idockPackage 'scripts/install-skill.mjs')
```

macOS / Linux：

```sh
node "$(npm root -g)/@gdpisen/idock-cli/scripts/install-skill.mjs"
```

默认安装到当前用户的 `~/.agents/skills/idock-cli`。新增技能未出现时重启 Codex，再通过技能列表或显式提及 `idock-cli` 确认加载。

### 其它 Agent

按宿主文档确定技能父目录，使用 `--target` 指定。安装器会创建 `idock-cli` 子目录，保留以下三个文件：

```text
<技能父目录>/idock-cli/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── workflows.md
```

`SKILL.md` 是调用指引，`references/workflows.md` 是它引用的工作流，`agents/openai.yaml` 提供 Codex 界面元数据。不要只复制 Skill 正文而遗漏引用文件。

安装器默认保护已有不同内容。安装结果中的文件路径和文件复制成功，只表示 Skill 已落盘，还需由宿主确认实际发现并加载。Skill 本身不会安装 CLI、创建账号或授予额外数据权限。

## 3. 使用个人 AccessKey 登录

先通过 iDock 提供的领取入口或管理员取得自己的 AccessKey，再在自己的交互终端执行：

```sh
idock auth login
idock auth status
idock doctor
```

`auth login` 隐藏输入 Key，并向服务端验证；验证成功后保存到当前环境的用户配置中。不要把 Key 发给 AI 聊天，不要把个人登录配置提交到 GitHub 或打包到 Skill 中。

当前采用 AccessKey 登录，**不会打开浏览器或提供二维码授权**。AI 的命令工具没有交互终端时，由用户在同一环境手动执行 `idock auth login`，完成后再让 AI 检查 `idock auth status` 和 `idock doctor`。

Key 无效、过期或撤销时，取得有效 Key 后重新登录。权限不足时需要申请相应权限；重新安装 CLI 或 Skill 不会扩大 Key 权限。

## 4. 开始使用

在 AI 对话中输入：

```text
使用 idock-cli 技能，列出我最近一周的 5 条录音，展示标题和时间。

使用 idock-cli 技能，从我的 RAG 知识库搜索“上次产品会议确定了哪些行动项”，附上来源。

使用 idock-cli 技能，把我选中的这条录音纪要导出成 Word，保存到 ./exports。
```

直接使用终端：

```sh
idock recording list --time 7d --limit 5
idock search "上次产品会议确定了哪些行动项" --limit 5
idock flash note list --time today
idock flash todo list --time 7d
```

先从列表取得实际录音 ID，再读取或导出：

```sh
idock recording get <录音ID> --section summary
idock recording download --id <录音ID> --source original --out ./downloads
idock recording export --id <录音ID> --type summary --file-format word --out ./exports
idock recording export --id <录音ID> --type transcription --file-format txt --out ./exports
```

完整命令帮助和机器可读契约：

```sh
idock --help
idock recording --help
idock help recording import
idock schema
```

`search` 只搜索用户 RAG 向量库。列表按返回的 `next_cursor` 续页；读取完整转写优先使用导出命令。每个用户的 CLI API 共用每秒一次限额，CLI 已内置有限重试，不需要 Agent 紧密循环重试。

导入音频并触发转写需要额外的 `recording:import` 权限和用户明确授权。请先查看 `idock recording import --help`，确认文件、真实时长及执行计划。

## 升级

```sh
npm install -g @gdpisen/idock-cli@latest --registry=https://registry.npmjs.org/
```

升级 CLI 后，重新运行相应宿主的 Skill 安装命令。目标已有不同内容时先比较并保留个人定制；确定使用发行文件替换后，再给安装器添加 `--force`。已有登录状态有效时可以继续使用。

安装包包含更完整的 `README.md`、`SKILL.md` 和 `references/workflows.md`，可供 Agent 按需读取。
