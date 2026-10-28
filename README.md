# 言简意明 · clarity-zh

为 Codex 提供简体中文默认输出规范，涵盖回复、文档和 Git 提交消息，减少多余的防御性措辞、重复免责与过度限定，保留必要的事实边界及代码、命令和技术标识符原文。

本项目是一个纯指令型 Skill，完整规则位于 [SKILL.md](./SKILL.md)，无需安装运行时依赖。

## 用途

- **中文输出**：默认使用简体中文撰写回复、进度说明和面向中文读者的文档；用户指定其他语言时遵从要求。
- **直接表达**：先给出结论或下一步，减少预先辩解、没有实际信息的对比、重复免责声明和限定词堆叠。
- **保留事实边界**：保留必要的条件、归因、不确定性及验证状态，准确说明信息缺口和实际风险。
- **保护技术原文**：保留代码、命令、日志、路径、API、配置键和技术标识符，保持 JSON、YAML 等格式有效。
- **统一提交消息**：保留 `feat`、`fix`、`docs` 等英文类型前缀，使用简体中文撰写变更描述和正文。

### 表达示例

| 场景 | 调整前 | 调整后 |
| --- | --- | --- |
| 自我辩护与过度限定 | 不要误会，我不是在否定这个方案，只是也许可能存在兼容性问题。 | 这个方案可能存在兼容性问题。 |
| 验证状态说明 | 测试已通过，但需要强调，这并不意味着真实设备一定正常。 | 测试已通过，真实设备上的表现尚未验证。 |
| Git 提交消息 | `feat: add CSV export` | `feat: 添加 CSV 导出功能` |

示例中的设备验证表述适用于确实未做设备验证的情况。规则按语义和上下文判断，不机械禁止某个词或句式。

## 安装

### 用户级安装

安装到用户级目录后，可在不同仓库中使用。以下命令需要本机已安装 Git；已有同名安装时，请使用后文的更新方法。

**Windows（PowerShell）**

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills" | Out-Null
git clone https://github.com/TimeseekerKK/clarity-zh.git "$env:USERPROFILE\.agents\skills\clarity-zh"
```

**macOS / Linux**

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/TimeseekerKK/clarity-zh.git "$HOME/.agents/skills/clarity-zh"
```

安装后，技能文件应位于 `~/.agents/skills/clarity-zh/SKILL.md`。也可以手动创建该目录，只将本仓库的 `SKILL.md` 放入其中。

Codex 会自动检测新增技能；如果没有出现，重启 Codex。目录位置与发现机制见 [OpenAI 技能文档](https://learn.chatgpt.com/docs/build-skills)。

### 项目级安装

只想在某个项目中使用时，将 `SKILL.md` 放到该项目的 `.agents/skills/clarity-zh/SKILL.md`。团队可将它纳入版本控制，与项目一起分发。

同一环境中建议只保留一份同名技能，避免选择时出现重复项。

## 使用方法

### 显式调用

在 Codex 提示词中写出技能名，并附上实际任务，例如：

```text
$clarity-zh 解释这段报错，并给出排查步骤。
```

```text
$clarity-zh 为这次修改编写 Git 提交消息。
```

也可以从客户端的技能选择器中选择 `clarity-zh`；Codex CLI 和 IDE 扩展支持输入 `$` 选择技能。

### 自动选用与持续默认生效

任务匹配技能描述时，Codex 可以自动选用本技能。技能正文在被选用后加载，因此用户级安装表示跨项目可用，不保证每轮回复都会加载。调用机制见 [OpenAI 技能文档](https://learn.chatgpt.com/docs/build-skills)。

如果希望每次任务都带上这些表达偏好，可将需要持续生效的规则写入 Codex 全局 `AGENTS.md`，默认位置为 `~/.codex/AGENTS.md`；设置了 `CODEX_HOME` 时使用该目录。保留已有规则并合并新增内容；若存在非空的 `AGENTS.override.md`，该层会优先读取它。具体机制见 [OpenAI AGENTS.md 文档](https://learn.chatgpt.com/docs/agent-configuration/agents-md)。

本技能只约束语言与表达方式，不改变任务目标、权限边界和项目规范，也不会因被加载而自动改写已有文件。

## 更新

通过上述 Git 克隆方式安装后，可拉取最新规则。

**Windows（PowerShell）**

```powershell
git -C "$env:USERPROFILE\.agents\skills\clarity-zh" pull --ff-only
```

**macOS / Linux**

```bash
git -C "$HOME/.agents/skills/clarity-zh" pull --ff-only
```

手动复制安装时，重新下载并替换对应的 `SKILL.md`；项目级安装时更新项目内的文件。更新后若未被识别，重启 Codex。
