# Claude Code 终极平替指南 (claude-code-deepseek-quickstart)

[![GitHub stars](https://img.shields.io/github/stars/yzfly/claude-code-deepseek-quickstart.svg)](https://github.com/yzfly/claude-code-deepseek-quickstart/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**一份专为国内开发者打造的指南，让你在 60 分钟内，以极低成本、无障碍地用上顶级的 AI 编程神器 Claude Code。**

<p align="center">
  <img src="https://files.mdnice.com/user/43439/d9396ec9-9e25-4126-819f-9b590bbd7574.png" alt="Project Banner" width="800"/>
</p>

## 项目背景

你是否对 Anthropic 出品的 `claude-code` 这款强大的 AI 编程工具心动不已？它能像一个经验丰富的工程师一样，在你的终端里与你协作，帮你读懂代码、实现功能、修复 Bug，甚至处理复杂的 Git 操作。

但现实的门槛可能让你望而却步：
*   **高昂的费用**：Claude 官方模型的 API 调用价格不菲。
*   **访问限制**：需要海外信用卡和“特殊”网络环境。
*   **封号风险**：账号策略不明，随时可能失去访问权限。

本教程的使命，就是解决以上所有痛点。核心思路是让 `claude-code` 使用像 **DeepSeek** 这样更具性价比、访问更便捷的大模型：既可以走 DeepSeek 官方的 Anthropic 兼容接口**直连**（推荐），也可以通过开源项目 `Claude Code Router` 灵活**路由**多家模型。

**最终目标**：让你在 60 分钟内，以极低的成本，在自己的电脑上流畅地用上这款顶级的 AI 编程神器。

![](https://files.mdnice.com/user/43439/8be5b2d5-367e-4633-8008-45b449531e01.png)
---

## 一、Claude Code 是什么？
![](https://files.mdnice.com/user/43439/d699b72e-08eb-4057-afb6-71462c067ae3.png)

在开始之前，我们先花 5 分钟快速了解 `claude-code` 到底是什么。

简单来说，`Claude Code` 是一个运行在**命令行终端**的**代码智能体（Agentic Coding Assistant）**。它不是一个简单的代码补全工具，而是一个可以与你的开发环境深度交互的“数字同事”。

它的核心能力包括：

*   **感知上下文**：能自动读取你项目中的文件，理解代码结构。
*   **执行命令**：可以直接调用你终端里的 `bash` 命令，比如 `npm run build`, `git log`, `ls -l` 等。
*   **文件操作**：可以创建、编辑、删除你项目中的文件。
*   **长期记忆**：通过一个名为 `CLAUDE.md` 的特殊文件，你可以告诉它项目的关键信息（如常用命令、代码规范、核心文件等），让它在整个会话中“记住”这些设定。
*   **可扩展性**：支持自定义命令、连接外部工具（如 MCP），潜力巨大。

官方文档虽然全面，但对于入门者来说过于冗长。你只需要记住：**它是一个拥有你电脑部分权限、能和你一起编程的 AI 助手。**

## 二、核心痛点与“平替”方案

正如我们开头提到的，直接使用官方 Claude Code 的主要障碍是成本和访问。现在有两条路线可选：

### 方案一：DeepSeek 官方直连（推荐，零依赖）

> 🆕 **2026 更新**：DeepSeek 官方已支持 [Anthropic API 兼容格式](https://api-docs.deepseek.com/guides/anthropic_api)，Claude Code 可以**直接连接 DeepSeek**，不再需要任何中间代理。这是目前最简单、最稳定的方案。

只需安装官方 Claude Code，然后设置两个环境变量：

```shell
npm install -g @anthropic-ai/claude-code

export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_API_KEY=你的_DeepSeek_API_Key

claude
```

模型会自动映射：Claude Code 请求 Opus 系列时由 `deepseek-v4-pro` 响应，请求 Sonnet / Haiku 系列时由 `deepseek-v4-flash` 响应；也可以用 `ANTHROPIC_MODEL=deepseek-v4-pro` 显式指定。两个模型均支持 **1M 上下文**与思考模式（`low / high / max` 三档推理强度）。

**价格（2026-08-16 起，每百万 token，美元；低谷时段为高峰的一半）**

| | V4-Flash | V4-Pro |
|---|---|---|
| 输入・缓存命中 | $0.014 / $0.007 | $0.044 / $0.022 |
| 输入・缓存未命中 | $0.44 / $0.22 | $1.32 / $0.66 |
| 输出 | $1.32 / $0.66 | $3.96 / $1.98 |

高峰时段为 UTC 01:00–04:00 与 06:00–10:00（周一至周五，约合北京时间 9–12 点、14–18 点），其余时间与周末为低谷价。Claude Code 会反复发送系统提示与工具定义，DeepSeek 的硬盘缓存命中率极高，**实际花费通常只有标价的 10%–20%**，比 Claude 官方模型便宜一到两个数量级。最新价格以 [官方价格页](https://api-docs.deepseek.com/quick_start/pricing) 为准。

也可以把配置写进 `~/.claude/settings.json`，不污染 shell 环境：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_API_KEY": "你的_DeepSeek_API_Key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_SMALL_FAST_MODEL": "deepseek-v4-flash"
  }
}
```

> 已知限制：DeepSeek 的 Anthropic 兼容接口不支持 document 内容块与 MCP 工具透传（Claude Code 自己管理的本地 MCP 不受影响）。Skills、Hooks、子 Agent 等功能均可正常使用。

把环境变量写进 `~/.bashrc` 或 `~/.zshrc` 即可长期生效。**如果你只想用 DeepSeek，看到这里就可以直接开始用了**；想组合多家模型（如 Gemini、Qwen）再看方案二。

### 方案零：DeepSeek 官方 Agent 框架 deepseek-harness（dsh）

> 🆕 2026-08-13，DeepSeek 开源了自家的 Agent 框架 **[deepseek-harness（dsh）](https://github.com/deepseek-ai/deepseek-harness)**（MIT，两周破 19 万 star）。它不是 Claude Code 的"壳"，而是一个"一切皆插件"的 harness：内置 MCP 客户端、兼容 Claude Code / Codex 的 Hook 协议与 Agent Skills，自带 Web UI 与 headless 模式。如果你本来就只打算用 DeepSeek，dsh 是零成本、无需任何兼容层的原生选择。

```shell
npx @deepseek-ai/dsh web     # 启动 Web UI，默认 http://127.0.0.1:3080，在 Providers 页填入 DeepSeek API Key
```

目前处于开发者预览阶段，接口会有破坏性变更；技能 / 插件精选见 [awesome-dsh-skills](https://github.com/yzfly/awesome-dsh-skills)，详细介绍见 [DeepSeek 生态指南](https://github.com/EmbraceAGI/awesome-chatgpt-zh/blob/main/docs/DeepSeek.md#deepseek-harness官方-agent-框架)。

### 方案二：Claude Code Router（多模型灵活路由）

如果你想按任务类型路由不同厂商的模型，可以使用 **[Claude Code Router](https://github.com/musistudio/claude-code-router)**。


![https://github.com/musistudio/claude-code-router](https://files.mdnice.com/user/43439/48150452-df46-4504-8cbd-c922f4a5cd7d.png)


**[Claude Code Router](https://github.com/musistudio/claude-code-router)** 是一个巧妙的**本地代理服务**。它的工作原理如下：

1.  你在终端运行 `claude-code`。
2.  `claude-code` 以为它在和 Anthropic 的官方服务器通信。
3.  `Claude Code Router` 在中间拦截了这个请求。
4.  它将请求转换格式，发送给你配置好的、兼容 OpenAI API 格式的模型服务（例如 DeepSeek, 硅基流动, Ollama 等）。
5.  收到模型返回的结果后，再转换成 `claude-code` 能理解的格式传回去。

整个过程对 `claude-code` 本身是透明的，但我们却实现了底层驱动模型的替换，完美解决了成本和访问问题。

## 三、三步轻松上手（方案二：Claude Code Router）

> 使用方案一（官方直连）的读者可以跳过本节和下一节。

让我们开始动手吧！整个过程不超过 10 分钟。

### 第 1 步：安装 Claude Code

打开你的终端，运行以下命令来全局安装 `claude-code`。

```shell
npm install -g @anthropic-ai/claude-code
```

### 第 2 步：安装 Claude Code Router

接着，安装我们的“魔法路由”和它依赖的 `tiktoken`。

```shell
npm install -g tiktoken
npm install -g @musistudio/claude-code-router
```

### 第 3 步：启动！

现在，不要用官方的 `claude` 命令，而是使用 `Claude Code Router` 提供的命令：

```shell
ccr code
```

如果一切顺利，你将看到如下的欢迎界面。恭喜你，`claude-code` 已经成功在你的本地运行起来了！

![Claude Code 成功运行界面](https://files.mdnice.com/user/43439/12e555c2-f037-4bfd-9541-9e657a9bbf7a.png)

> **小提示：** 首次运行时，你可以根据提示运行 `/init` 来创建一个 `CLAUDE.md` 配置文件，或者运行 `/terminal-setup` 来更好地集成终端。

默认情况下，`Claude Code Router` 已经配置好了 DeepSeek 作为后端，需要在配置文件中添加 API Key，下面是如何配置的教程。

---

## 四、进阶配置：打造你的专属模型路由

如果你想获得更强大的性能或更灵活的组合，就需要了解它的配置文件。

配置文件位于你的用户主目录下的 `~/.claude-code-router/config.json`。

下面我们提供几种推荐的配置方案，你可以根据自己的需求选择一个，并**记得将 `sk-xxx` 替换成你自己的 API Key**。

### 方案一：极致性价比（纯 DeepSeek）

这是最简单、成本最低的配置。适用于日常的大部分编程任务。DeepSeek 的 `deepseek-v4-pro` 模型在代码和推理任务上表现出色。

> ⚠️ 旧模型名 `deepseek-chat` 和 `deepseek-reasoner` 已于 2026 年 7 月 24 日正式停用，请使用 `deepseek-v4-flash`（日常任务）和 `deepseek-v4-pro`（复杂推理）。

```json
{
    "log": true,
    "Providers": [
        {
            "name": "deepseek",
            "api_base_url": "https://api.deepseek.com",
            "api_key": "sk-xxx",
            "models": ["deepseek-v4-pro", "deepseek-v4-flash"],
            "transformer": {
                "use": ["deepseek"]
            }
        }
    ],
    "Router": {
        "default": "deepseek,deepseek-v4-flash",
        "background": "deepseek,deepseek-v4-flash",
        "think": "deepseek,deepseek-v4-pro",
        "longContext": "deepseek,deepseek-v4-flash"
    }
}

```

*   `background`: 用于处理背景任务，对智能要求不高，`deepseek-v4-flash` 足够。
*   `think`: 用于执行核心的思考和规划任务，我们使用最强的 `deepseek-v4-pro`。
*   `longContext`: 当上下文长度较长时使用。

### 方案二：混合搭配（DeepSeek + Qwen 长文本）

DeepSeek V4 系列已支持 1M 上下文，长上下文不再是瓶颈。不过如果你想在不同任务上使用不同厂商的模型（比如某些任务偏好 Qwen 的表现，或想利用硅基流动的赠送额度），依然可以混合搭配。

```json
{
    "log": true,
    "Providers": [
        {
            "name": "deepseek",
            "api_base_url": "https://api.deepseek.com",
            "api_key": "sk-xxx",
            "models": ["deepseek-v4-pro", "deepseek-v4-flash"],
            "transformer": {
                "use": ["deepseek"],
                "deepseek-v4-flash": {
                    "use": ["tooluse"]
                }
            }
        },
        {
            "name": "siliconflow",
            "api_base_url": "https://api.siliconflow.cn/v1",
            "api_key": "sk-xxx",
            "models": ["Qwen/Qwen3-32B", "Qwen/Qwen3-235B-A22B", "Qwen/Qwen3-8B"],
            "transformer": {
                "use": ["qwen"]
            }
        }
    ],
    "Router": {
        "default": "deepseek,deepseek-v4-flash",
        "background": "deepseek,deepseek-v4-flash",
        "think": "deepseek,deepseek-v4-pro",
        "longContext": "siliconflow,Qwen/Qwen3-32B"
    }
}

```

> 在这个配置中，日常任务走 DeepSeek，超长上下文场景会自动切换到 Qwen 模型。你也可以按自己的偏好调整各路由指向的模型。

### 方案三：顶配之选（集成 Gemini 百万上下文）

如果你追求极致性能，希望处理百万级别的超长上下文（例如，分析整个代码库），可以考虑将 Google 的 Gemini 模型接入。

由于 Gemini 的 API 格式与 OpenAI 不同，我们需要一个额外的转换层。这里推荐使用开源项目 `openai-gemini` 配合 Cloudflare Workers 实现。

1.  **部署 Gemini 代理**：
    *   访问 [openai-gemini](https://github.com/PublicAffairs/openai-gemini) 项目。
    *   点击 "Deploy on Cloudflare" 按钮，按照指引完成部署。你需要填入你的 Cloudflare 账户 ID 和一个创建好的 API Token。
    *   部署成功后，为你的 Worker 添加一个自定义域名，例如 `my-gemini-proxy.com`。

2.  **测试代理**：
    *   在终端运行以下命令，替换你的域名和 Gemini API Key。
    ```shell
    curl -H "Authorization: Bearer [你的 Gemini API KEY]" https://[你的域名]/v1/models
    ```
    *   如果成功返回模型列表，说明你的代理已经就绪。

    ![Gemini 成功部署](https://files.mdnice.com/user/43439/45f27ce0-cd23-4d49-a1c1-5899ab2abbb4.png)

3.  **更新 `config.json`**：
    将你的 Gemini 代理添加到 `Providers` 列表，并设置为 `longContext` 的处理器。

```json
{
    "log": true,
    "Providers": [
        {
            "name": "deepseek",
            "api_base_url": "https://api.deepseek.com",
            "api_key": "sk-xxx",
            "models": ["deepseek-v4-pro", "deepseek-v4-flash"],
            "transformer": {
                "use": ["deepseek"],
                "deepseek-v4-flash": {
                    "use": ["tooluse"]
                }
            }
        },
        {
            "name": "gemini",
            "api_base_url": "https://xxx/v1",
            "api_key": "xxx",
            "models": ["gemini-2.5-pro-preview-06-05", "gemini-2.5-flash-preview-05-20"],
            "transformer": {
                "use": ["gemini"]
            }
        }
    ],
    "Router": {
        "default": "deepseek,deepseek-v4-flash",
        "background": "deepseek,deepseek-v4-flash",
        "think": "deepseek,deepseek-v4-pro",
        "longContext": "gemini,gemini-2.5-pro-preview-06-05"
    }
}

```
如果不知道怎么用，可以使用ai写config(将上述代码粘贴给ai,说明自己的要求）

### 动态切换模型

在 `claude-code` 会话中，你随时可以使用 `/model` 命令动态切换当前使用的模型。

```
/model deepseek,deepseek-v4-pro
```
或者
```
/model siliconflow,Qwen/Qwen3-32B
```

---

## 五、Claude Code 实战技巧

![](https://files.mdnice.com/user/43439/8be5b2d5-367e-4633-8008-45b449531e01.png)

工具已经就绪，现在分享一些让你事半功倍的实战技巧。

*   **明确你的指令**：把它当成一个初级工程师，指令越清晰、越具体，效果越好。
    *   **不好**: `给 foo.py 加测试。`
    *   **好**: `为 foo.py 里的 'calculate_discount' 函数写一个单元测试，需要覆盖用户未登录和折扣码过期的两种边缘情况。不要使用 mock。`

*   **善用 `@` 引用文件**：输入 `@` 会弹出文件列表，可以快速将文件内容加入上下文，让它分析或修改。

*   **用 `#` 帮助它记忆**：当你发现一个常用命令或重要信息时，可以用 `#` 开头告诉它，它会自动存入 `CLAUDE.md`。
    *   `# 启动服务的命令是 npm run dev`

*   **“四步工作流”**：处理复杂任务时，遵循这个模式能极大提高成功率。
    1.  **探索 (Explore)**: 让它先读取相关文件，理解现状。`请阅读 src/utils/auth.ts 和 src/components/Login.vue，不要写任何代码。`
    2.  **规划 (Plan)**: 让它制定一个行动计划。`现在，请给出一个重构登录逻辑的详细步骤计划。`
    3.  **编码 (Code)**: 在你确认计划后，让它开始实施。`计划看起来不错，请开始根据计划修改代码。`
    4.  **提交 (Commit)**: 完成后，让它帮你写 commit message 并提交。`代码工作正常，请帮我提交这些改动。`

*   **给它“看”图片**：你可以直接将 UI 设计稿、错误截图等图片拖拽或粘贴到终端里，让它根据视觉信息来写代码或 Debug。

*   **随时打断和纠正**：
    *   按 `Esc` 键可以随时打断它的当前操作。
    *   连按两次 `Esc` 可以回到上一步，修改你的指令，从一个不同的方向重新尝试。

---

## 总结

通过本教程，你已经成功地绕过了使用 `claude-code` 的所有障碍，并搭建了一套适合自己的、高性价比的 AI 编程工作流。现在，你拥有了一个强大的 AI 编程伙伴，它将极大地提升你的开发效率。

大胆地去探索和尝试吧！把它用到你的日常工作中，无论是写新功能、重构老代码、学习一个新框架，还是编写文档，它都能成为你的得力助手。


## 🙏 致谢

本指南的核心得以实现，离不开以下优秀项目：

*   [**@anthropic-ai/claude-code**](https://docs.anthropic.com/en/claude-code)：官方出品的强大 AI 编程智能体。
*   [**@musistudio/claude-code-router**](https://github.com/musistudio/claude-code-router)：实现了模型路由的关键项目，是本指南的基石。
*   [**openai-gemini**](https://github.com/PublicAffairs/openai-gemini)：提供了便捷的 Gemini-to-OpenAI 转换方案。
*   [**deepseek-harness**](https://github.com/deepseek-ai/deepseek-harness)：DeepSeek 官方 Agent 框架。

感谢所有为 AI 和开源社区做出贡献的开发者！

## 🤝 贡献指南

我们欢迎任何形式的贡献！

*   发现文档错误？请[提交一个 Issue](https://github.com/yzfly/claude-code-deepseek-quickstart/issues/new)。
*   有更好的模型配置方案或使用技巧？欢迎[提交 Pull Request](https://github.com/yzfly/claude-code-deepseek-quickstart/pulls)！
*   对项目有任何建议或问题，都可以在 [Issues](https://github.com/yzfly/claude-code-deepseek-quickstart/issues) 区进行讨论。

## 📜 License

本项目采用 [MIT License](LICENSE)。

---

<p align="center">
  Crafted with ❤️ by <a href="https://github.com/yzfly">云中江树 (yzfly)</a>
</p>
