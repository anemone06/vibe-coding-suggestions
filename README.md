

# Vibe Coding 终极指南 V1.2
**原文链接：** https://github.com/EnzeD/vibe-coding
**作者：** [Nicolas Zullo, https://x.com/NicolasZu](https://x.com/NicolasZu)  
**创建日期：** 2025 年 3 月 12 日  
**最后更新：** 2025 年 10 月 6 日  

---

## 快速入门
开始 Vibe Coding，你只需要以下两种工具之一：  
- **Claude Sonnet 4.5**，在 Claude Code 中使用
- **gpt-5-codex (high)**，在 Codex CLI 中使用

本指南适用于 CLI 版本（在终端中使用）和 VSCode 扩展版本（Codex 和 Claude Code 都有相应的扩展，且界面更新）。

*（注：虽然本指南的早期版本使用了 **Grok 3**，随后过渡到了 **Gemini 2.5 Pro**。而现在我们使用的是 **Claude 4.5** 或 **gpt-5-codex (high)**）*

*（注 2：如果你想使用 Cursor，请参考本指南的 [1.1 版本](https://github.com/EnzeD/vibe-coding/tree/1.1.1)，但我们认为它的功能不如 Codex CLI 或 Claude Code 强大）*

正确的配置是成功的关键。如果你认真想开发一个功能齐全且视觉精美的游戏（或应用），请花时间打好坚实的基础。

**核心原则：** *规划胜过一切。* 不要让 AI 自主进行规划，否则你的代码库会变成一团乱麻，无法维护。

---

## 环境搭建

### 1. 游戏设计文档 (GDD)
- 拿出的你的游戏创意，让 **GPT-5** 或 **Sonnet 4.5** 创建一个简单的 Markdown 格式的**游戏设计文档**：`game-design-document.md`。  
- 审查并完善该文档，确保它符合你的愿景。文档可以很基础——其目的是让 AI 了解游戏的结构和意图。不要过度设计，因为我们稍后会进行迭代。

### 2. 技术栈与 `CLAUDE.md` / `Agents.md`
- 让 **GPT-5** 或 **Sonnet 4.5** 为你的游戏推荐最佳技术栈（例如：用于 3D 多人游戏的 ThreeJS 和 WebSocket）。将其保存为 `tech-stack.md`。
  - 要求它提出*最简单但最稳健的方案*。  
- 在终端中打开 **Claude Code** 或 **Codex CLI**，并使用 `/init` 命令。它会引用你目前创建的两个 .md 文件。这将创建一套规则，引导 LLM 正确工作。 
- **至关重要：检查生成的规则。** 确保它们强调**模块化**（多文件），并反对**单体化**（一个巨大的文件）。你可能需要手动调整或添加规则，并检查它们的触发时机。
  - **重要提示：** 某些规则对于维持上下文至关重要，应设置为 **"Always" (始终执行)** 规则。这能确保 AI 在编写任何代码前*始终*参考这些规则。考虑添加如下规则并标记为 "Always"：
    > ```
    > # 重要：
    > # 在编写任何代码前，务必阅读 memory-bank/@architecture.md。包含完整的数据库架构。
    > # 在编写任何代码前，务必阅读 memory-bank/@game-design-document.md。
    > # 在添加主要功能或完成里程碑后，更新 memory-bank/@architecture.md。
    > ```
  - 示例：确保其他（非 "Always"）规则引导 AI 遵循你所选技术栈的最佳实践（如网络通信、状态管理等）。
  - *如果你想要游戏尽可能优化、代码尽可能整洁，这套规则设置是强制性的。*

### 3. 实施计划 (Implementation Plan)
- 向 **GPT-5** 或 **Sonnet 4.5** 提供：  
  - 游戏设计文档 (`game-design-document.md`)
  - 技术栈建议 (`tech-stack.md`)
- 让它创建一个详细的 Markdown 格式 **实施计划** (`.md`)，这是一套给 AI 开发者的分步骤指令。  
  - 步骤应当小而具体。  
  - 每个步骤必须包含一个测试，以验证实现是否正确。  
  - 计划中不要包含代码——只需清晰、具体的指令。  
  - 专注于*基础游戏*，而不是全量功能（细节稍后补充）。  

### 4. 记忆库 (Memory Bank)
- 为你的项目创建一个新文件夹，然后在 VSCode 中打开它。
- 在项目文件夹内，创建一个名为 `memory-bank` 的子文件夹。  
- 将以下文件添加到 `memory-bank` 中：  
  - `game-design-document.md`  
  - `tech-stack.md`  
  - `implementation-plan.md`  
  - `progress.md`（创建一个空文件，用于追踪已完成的步骤）  
  - `architecture.md`（创建一个空文件，用于记录文件用途）

---

## 编写基础游戏 (Vibe Coding the Base Game)
现在有趣的部分开始了！

### 确保一切清晰
- 在 VSCode 扩展中打开 **Codex** 或 **Claude Code**，或者在项目终端启动 Claude Code 或 Codex CLI。 
- 提示词：阅读 `/memory-bank` 中的所有文档，`implementation-plan.md` 是否清晰？为了让你 100% 明确任务，你还有什么问题？
- 它通常会问 9-10 个问题。回答它们并提示它相应地编辑 `implementation-plan.md`，使其更加完善。

### 你的第一个实现提示词
- 在 VSCode 扩展或终端中打开工具。  
- 提示词：阅读 `/memory-bank` 中的所有文档，执行实施计划的第 1 步。我会运行测试。在我验证测试通过前，不要开始第 2 步。一旦我通过验证，请打开 `progress.md` 并记录你为后续开发所做的工作。然后将任何架构见解添加到 `architecture.md` 中，解释每个文件的作用。
- **务必**先以 "Ask" 模式或 "Plan Mode"（Claude Code 中使用 `shift+tab`）开始，当你满意后，再允许 AI 执行该步骤。

- **极致体验 (Extreme vibe)：** 安装 [Superwhisper](https://superwhisper.com)，通过语音随性地与 Claude 或 GPT-5 交流，而不是打字。  

### 工作流
- 完成第 1 步后：  
- 将更改提交 (Commit) 到 Git（如果不熟悉，请向 AI 寻求帮助）。  
- 开启新对话（使用 `/new` 或 `/clear`）。  
- 提示词：现在查看 memory-bank 中的所有文件，阅读 progress.md 了解之前的工作，然后开始执行第 2 步。在我验证测试通过前，不要开始第 3 步。
- 重复此过程，直到完成整个 `implementation-plan.md`。  

---

## 完善细节
恭喜，你已经构建了基础游戏！它可能还比较简陋且缺乏细节，但现在你可以开始实验和优化了。  
- 想要雾效、后期处理、特效或声音？想要更酷的飞机/赛车/城堡？想要绚丽的天空？
- 对于每个主要功能，创建一个新的 `feature-implementation.md` 文件，包含简短的步骤和测试。  
- 逐步实施并测试。  

---

## 修复 Bug 和解决卡壳
- 如果某个提示词失败或导致游戏崩溃：  
- 在 Claude Code 中使用 `/rewind` 并精简你的提示词直到成功。如果使用 GPT-5，可以频繁提交 git 并在需要时重置 (reset)。
- 针对错误：  
    - **如果是 JavaScript：** 打开控制台 (`F12`)，复制错误信息，并将其粘贴到 VSCode 中，同时提供视觉错误截图。  
    - **懒人选项：** 安装 [BrowserTools](https://browsertools.agentdesk.ai/installation) 来跳过手动复制/截图。  
- 如果卡住了：  
    - 回退到上一个 Git 提交 (`git reset`)，尝试使用新的提示词。  
- 如果*真的*彻底卡住了：  
    - 使用 [RepoPrompt](https://repoprompt.com/) 或 [uithub](https://uithub.com/) 将你的整个代码库合并为一个文件，并请求 **GPT-5 或 Claude** 的协助。  

---

## Claude Code & Codex 技巧
- **在终端运行：** 在 VSCode 的集成终端中运行任一工具，这样无需离开工作区即可查看 diff (差异) 并提供额外上下文。
- **Claude Code `/rewind`：** 如果某次迭代偏离了目标，使用此命令将项目回滚到之前的状态。
- **自定义 Claude Code 命令：** 创建像 `/explain $arguments` 这样的助手命令，触发诸如“深入研究代码并理解 $arguments 的工作原理。一旦你理解了，告诉我，我会给你具体的任务”之类的提示词，让模型在修改代码前获取丰富的上下文。
- **清理上下文：** 经常使用 `/clear` 清理上下文，或者如果你仍需要之前的对话背景，使用 `/compact`。
- **节省时间（风险自负）：** 使用 `claude --dangerously-skip-permissions` 或 `codex --yolo` 启动，这样工具在执行操作时不会询问确认。

## 其他提示
- **小改动：** 使用 GPT-5 (medium)
- **优秀的营销文案：** 使用 Opus 4.1
- **生成优秀的精灵图 (2D 图像)：** 使用 ChatGPT 和 Nano Banana
- **生成音乐：** 使用 Suno
- **生成音效：** 使用 ElevenLabs
- **生成视频：** 使用 Sora 2
- **优化提示词输出：** 
    - 添加：“思考时间越长越好，我不着急。重要的是你精准地遵循我的要求并完美执行。如果我不够精确，请向我提问。” 
    - 对于 Claude Code，使用特定词汇触发更深层的推理：`think` < `think hard` < `think harder` < `ultrathink`。

---

## 常见问题解答
**问：我是在做一个 App 而不是游戏，工作流是一样的吗？**  
**答：** 绝大部分是一样的！你可以用 PRD（产品需求文档）代替 GDD。你也可以使用 v0、Lovable 或 Bolt.new 等优秀工具先制作原型，然后将代码移至 GitHub，再克隆到本地，通过本指南在 VSCode 或终端中继续开发。

**问：你在空战游戏里的那架飞机太棒了，但我没法通过一个提示词复现它！**  
**答：** 这不是一个提示词做出来的——它是通过约 30 个提示词，在特定的 `plane-implementation.md` 文件引导下完成的。使用尖锐且具体的提示词，例如“在机翼上切割出副翼的空间”，而不是像“做一架飞机”这样模糊的指令。

**问：为什么 Claude Code 或 Codex CLI 现在比 Cursor 更好？**  
**答：** 这完全取决于个人喜好。我们强调的是，Claude Code 在驱动 Claude Sonnet 4.5 方面更出色，而 Codex CLI 在驱动 GPT-5 方面比 Cursor 更好。让它们运行在终端中可以解锁更多的开发流：可以在任何 IDE 中工作，可以通过 SSH 进入远程服务器等。还有强大的自定义选项，如自定义命令、子代理 (sub-agents) 和钩子 (hooks)，随着时间推移，这些将提升开发的质量和速度。最后，即使你只订阅了基础版的 Claude 或 ChatGPT 方案，也足以开始使用。

**问：我不知道如何为我的多人游戏搭建服务器。**  
**答：** 问你的 AI。

---
