# task-clarifier

一个用于“任务开工检查”的 Agent Skill。

它不会为了形式完整而机械追问，而是先检查真正可能改变结果的缺口，再选择四种处理方式之一：

- 信息充分：直接完成任务，不输出内部检查过程；
- 存在低风险、可撤销的缺口：说明必要假设后继续；
- 缺口会改变结论：只询问最关键的问题；
- 关键依据或审批无法取得：只暂停受影响的部分。

## 适合什么场景

- 接到一个信息可能不完整、约束可能冲突的任务；
- 在执行前判断“直接做、带假设做、先确认，还是暂停局部”；
- 需要保护 JSON、表格、字数限制、只输出正文等严格交付格式；
- 希望把一套反复使用的任务澄清方法沉淀为可维护规则。

它不用于普通的润色、校对或内容评价，除非这些任务本身存在会改变结果的关键缺口。

## 目录结构

```text
task-clarifier-skill/
├─ SKILL.md                         # 触发条件、核心规则与处理边界
├─ agents/
│  └─ openai.yaml                  # OpenAI 产品可选元数据
├─ references/
│  └─ decision-examples.md         # 10 个边界案例
└─ dist/
   └─ task-clarifier-0.3.0.zip     # 可下载的安装包
```

## 核心检查框架

执行任务前先检查四件事：

1. 输入依据是否足够；
2. 执行约束是否明确且相互兼容；
3. 完成标准是否可以判断；
4. 异常情况和权限边界是否需要处理。

Skill 的完整规则、澄清预算、严格格式保护和权限边界请查看 [`SKILL.md`](./SKILL.md)。

## 使用方式

### 方式一：下载压缩包

下载 [`dist/task-clarifier-0.3.0.zip`](./dist/task-clarifier-0.3.0.zip)，解压后把 `task-clarifier` 文件夹放入产品支持的 Skills 目录。

常见位置示例：

- Codex用户级目录：$HOME/.agents/skills/task-clarifier/
- Windows示例：%USERPROFILE%\.agents\skills\task-clarifier\
- 项目级目录：项目根目录/.agents/skills/task-clarifier/
- Claude Code（项目级）：`.claude/skills/task-clarifier/`

不同产品的发现、安装和调用方式可能不同，请以对应产品的官方说明为准。

### 方式二：直接复制目录

也可以下载本仓库，把 `SKILL.md`、`agents/` 和 `references/` 按原有结构复制到一个名为 `task-clarifier` 的 Skill 目录中。

安装后，可以在任务中主动点名使用，例如：

```text
请使用 $task-clarifier，先判断下面的任务能否直接开始，再按判断结果处理：
……
```

在支持自动匹配的产品中，产品也可能根据 Skill 的名称和描述自动调用；自动触发效果取决于具体产品。

## 测试边界

当前版本为 `0.3.0`。它已通过结构校验和有限案例检查，覆盖直接继续、带假设继续、请求确认、局部暂停以及严格格式保护等边界，但尚不能据此证明它在不同产品中的自动触发和重复执行都已稳定。

## 参考资料

- [OpenAI：Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Claude Code：Extend Claude with skills](https://code.claude.com/docs/en/skills)
- [Agent Skills 开放规范](https://agentskills.io/specification)

## 许可证

本项目采用 [MIT License](./LICENSE)。
你可以使用、复制、修改和分发本项目，但须保留原版权声明和许可证声明。