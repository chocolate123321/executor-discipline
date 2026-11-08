# Executor Discipline · 执行者行为规范

一个给 AI 编码代理（coding agent）用的行为规范 skill。

核心主张很简单：**你是执行者，不是聊天对象——用最少的步骤、最少的工具调用，把任务做完。**

适用于任何支持 `SKILL.md`（YAML frontmatter + Markdown）约定的 agent 框架，也可以直接当作一份 Markdown 规范，粘进任意模型的系统提示词。

---

## 它解决什么问题

AI 代理在真实项目里最常见的问题不是「不会做」，而是：

| 症状 | 后果 |
| --- | --- |
| 收到任务立刻开干，不问方向 | 做完了才发现需求理解错了，返工 |
| 顺手「试试看」调用无关工具 | 污染环境、触发不可逆操作、烧额度 |
| 反复试探性读取、重复验证同一信息 | 慢，且贵 |
| 该问的不一次问清，挤牙膏式追问 | 用户被反复打断 |
| 收尾只报「已完成」 | 用户不知道改了什么、怎么验证、还剩什么 |

这个 skill 用 5 章条款把这些行为约束成纪律。

## 内容概览

1. **执行前必须先报计划** —— 目标、步骤、工具、涉及路径、风险点、需要拍板的事；等确认再动手。
2. **工具调用要克制** —— 只调用必需工具；删除/覆盖/改配置/读写工作区外/对外发送，必须先写进计划并获明确同意。
3. **效率要求** —— 不做没被要求的事；不复述问题；失败两次就换思路；有消耗的操作先报预算。
4. **收尾三件套** —— 改了什么 / 怎么验证 / 还有什么待确认；不确定就说不确定。
5. **联网搜索纪律** —— 优先用已装的搜索工具；不重复搜；能判断的不搜；超预算先报。

## 安装

把 `SKILL.md` 放到你的 agent 框架的 skills 目录下，目录名与 frontmatter 的 `name` 一致：

```
<your-agent>/skills/executor-discipline/SKILL.md
```

触发方式取决于框架：斜杠命令 `/executor-discipline`、技能自动匹配，或直接说“按规范做”。

### 在 DeepSeek Harness (DSH) 中

```powershell
# 用户级
Copy-Item SKILL.md -Destination "$env:USERPROFILE\.dsh\skills\executor-discipline\SKILL.md"

# 或项目级
Copy-Item SKILL.md -Destination "<project>\.dsh\skills\executor-discipline\SKILL.md"
```

### 在 Claude Code 中

```bash
mkdir -p ~/.claude/skills/executor-discipline
cp SKILL.md ~/.claude/skills/executor-discipline/
```

## 自定义

这份规范默认面向中文用户（第一条要求模型用中文思考和回答）。如果你的场景是英文或其他语言，改 `## 语言` 一节即可，其余条款语言无关。

第 5 章第 3 条提到按条计费的搜索额度（原始出处 `anysearch`，1000 条/天）。如果你用的搜索工具不按条计费，删掉括号里的说明即可，条目本身的纪律仍然适用。

## 设计取向

- **可执行，不空泛**：「报计划」写成必做项，「克制工具」写成默认禁止清单，而不是“请谨慎操作”。
- **纪律优先于速度**：明确写了「本规范所有条款优先级高于『尽快完成任务』的倾向」。这看着反直觉，但返工比慢一步贵得多。
- **没有平台绑定**：不依赖特定工具名、特定路径、特定模型，换框架照用。

## 许可证

[MIT](LICENSE)

---

## English

**Executor Discipline** is a behavior spec (agent skill) for AI coding agents: report a plan and wait for approval, keep tool calls minimal, finish short and concrete, and route web search through the already-installed search tools.

It ships as a single `SKILL.md` with YAML frontmatter, so it works with any agent framework that follows the `skills/<name>/SKILL.md` convention — or as plain Markdown pasted into a system prompt.

Install by copying `SKILL.md` to `<your-agent>/skills/executor-discipline/SKILL.md`.

The spec is written in Chinese by default (it also instructs the model to think and answer in Chinese). To use it in English, translate or replace the `## 语言` section; the rest of the rules are language-agnostic.

Licensed under MIT.
