# teamleader

`teamleader` 是一个面向 Codex 的 `leader` 技能仓库，用来把模型约束为“组长/负责人”角色：先收敛目标、制定原则和计划、分配任务给子 agent、持续巡检推进，再做验收交付。

当前版本：`0.1.4`

## 仓库内容

- [SKILL.md](/E:/teamleader/SKILL.md): 技能主说明
- [agents/openai.yaml](/E:/teamleader/agents/openai.yaml): 供 Codex 识别的技能元数据
- [VERSION](/E:/teamleader/VERSION): 当前版本号
- [CHANGELOG.md](/E:/teamleader/CHANGELOG.md): 版本变更记录

这不是一个 Web、后端或桌面应用项目，没有独立的启动入口。

## 安装

把仓库内容复制到本地 Codex skills 目录下的 `leader` 文件夹。

Windows 示例：

```powershell
New-Item -ItemType Directory -Force C:\Users\user\.codex\skills\leader | Out-Null
Copy-Item E:\teamleader\SKILL.md C:\Users\user\.codex\skills\leader\SKILL.md -Force
New-Item -ItemType Directory -Force C:\Users\user\.codex\skills\leader\agents | Out-Null
Copy-Item E:\teamleader\agents\openai.yaml C:\Users\user\.codex\skills\leader\agents\openai.yaml -Force
```

如果当前环境里已经有同名 `leader` skill，建议先备份再覆盖。

## 版本说明

`0.1.4` 是在 `0.1.3` 基础上的小版本增强，重点是优化“子 agent 默认模型选择”：

- 简单任务创建子 agent 时，必须优先使用 `gpt-5.3-codex-spark` 提升吞吐。
- 特别复杂任务才允许继承组长模型，以保证推理质量。

`0.1.3` 是在 `0.1.2` 基础上的小版本增强，重点是把“不要停住”从口号改成状态机硬约束：

- 目标未完成前，每次唤醒都必须执行一个明确推进动作。
- `DONT_NOTIFY` 只代表不打扰用户，不能代表没有内部推进。
- 所有子 agent 完成后，主线程必须立即复核、集成、验证或续派。

`0.1.2` 是在 `0.1.1` 基础上的小版本增强，重点是把 heartbeat 提示词从“检查型”改成“推进型”：

- 禁止把 heartbeat 主要写成“检查、识别、汇总、建议下一步”。
- 要求 heartbeat 发现完成态、等待命令或新结果时，立即续派、复核、集成或宣布阻塞。
- 强化“继续干活，快速干活”在 heartbeat 中的主命令地位。

`0.1.1` 是在 `0.1.0` 基础上的小版本增强，重点是把心跳检测从“只强调开启”补成“开启 + 关闭”的完整闭环：

- 项目开始前必须设定并确认心跳已生效。
- 项目完成、终止、取消或被替代后，必须主动关闭心跳。
- 增加关闭条件、`已关闭` 状态和交付前关闭自检，减少无效的每分钟提醒。

`0.1.0` 是初始小版本封装发布，重点是把已有 skill 整理成更稳定、更容易复用的发布形态：

- 增加版本号文件，便于后续继续迭代。
- 增加变更记录，便于区分强化内容。
- 强化“心跳检测机制”相关约束，降低组长漏写巡检机制的概率。
- 补齐仓库级安装和验证文档。

## 验证

最小验证方式：

1. 确认 `C:\Users\user\.codex\skills\leader\SKILL.md` 存在。
2. 确认 `C:\Users\user\.codex\skills\leader\agents\openai.yaml` 能被 `utf-8` 正常读取。
3. 用 YAML 解析工具检查 `openai.yaml` 是否可解析。
4. 重开一个 Codex 会话，确认技能列表里出现 `leader`。

示例检查：

```powershell
Get-ChildItem C:\Users\user\.codex\skills\leader -Recurse
@'
from pathlib import Path
import yaml

skill = Path(r"C:\Users\user\.codex\skills\leader\SKILL.md").read_text(encoding="utf-8")
meta = yaml.safe_load(Path(r"C:\Users\user\.codex\skills\leader\agents\openai.yaml").read_text(encoding="utf-8"))
print(skill[:80])
print(meta)
'@ | python -
```

## 使用

当任务需要“负责人/组长模式”时，在 Codex 中明确提到 `leader` skill，或者描述出以下意图：

- 先收敛需求再执行
- 制定顶层原则和阶段计划
- 把执行性工作拆给子 agent
- 持续调度并做阶段验收

## 当前仓库状态

当前仓库提供的是技能定义，不包含自动安装脚本、示例任务或集成测试。如果后续要增强体验，建议补充：

- 自动安装脚本
- 示例对话
- 版本说明或变更记录
