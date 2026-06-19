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

本教程的使命，就是解决以上所有痛点。我们将通过一个名为 `Claude Code Router` 的开源项目，将 `claude-code` 的请求“路由”到像 **DeepSeek** 这样更具性价比、访问更便捷的大模型上。

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

正如我们开头提到的，直接使用官方 Claude Code 的主要障碍是成本和访问。而我们的解决方案是 **[Claude Code Router](https://github.com/musistudio/claude-code-router)**。


![https://github.com/musistudio/claude-code-router](https://files.mdnice.com/user/43439/48150452-df46-4504-8cbd-c922f4a5cd7d.png)


**[Claude Code Router](https://github.com/musistudio/claude-code-router)** 是一个巧妙的**本地代理服务**。它的工作原理如下：

1.  你在终端运行 `claude-code`。
2.  `claude-code` 以为它在和 Anthropic 的官方服务器通信。
3.  `Claude Code Router` 在中间拦截了这个请求。
4.  它将请求转换格式，发送给你配置好的、兼容 OpenAI API 格式的模型服务（例如 DeepSeek, 硅基流动, Ollama 等）。
5.  收到模型返回的结果后，再转换成 `claude-code` 能理解的格式传回去。

整个过程对 `claude-code` 本身是透明的，但我们却实现了底层驱动模型的替换，完美解决了成本和访问问题。

### 方案二：免配置一键使用（2026 年推荐）

CCR 方案功能强大，但依赖 Node.js 和 npm，手写 JSON 配置文件对新人和学生来说成功率不高。

如今有了更简单的选择：**[Claude Code 中文版启动器](https://www.claudezip.cn?utm_source=github&utm_medium=article&utm_campaign=claude-code-qidongqi)** 内置了可视化的 BYOK（自带密钥）功能。你不再需要安装任何命令行工具，只需三步：

1. 打开启动器，进入密钥管理面板
2. 填入 DeepSeek API Key
3. 打开开关，一键启用

DeepSeek v4 Pro 模型已发布，推理能力大幅提升。结合启动器的可视化 BYOK，**无需安装、无需配置 JSON、无需搭建中转服务**，就能在 Claude Code 中以 Claude Opus 价格约二十分之一的成本使用顶级编程助手。

![在启动器的密钥管理面板中填入 API Key，一键启用 DeepSeek V4 模型](https://github.com/tiztaz/claude-code-qidongqi/raw/main/images/settings_api_keys.png)

> 📖 详细教程：[免配置，在 Claude Code 中直接使用 Deepseek v4 模型](https://github.com/tiztaz/claude-code-qidongqi/blob/main/bring-your-own-deepseek-key.md)

## 三、三步轻松上手

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

这是最简单、成本最低的配置。适用于日常的大部分编程任务。DeepSeek 的 `deepseek-reasoner` 模型在代码和推理任务上表现出色。

```json
{
    "log": true,
    "Providers": [
        {
            "name": "deepseek",
            "api_base_url": "https://api.deepseek.com",
            "api_key": "sk-xxx",
            "models": ["deepseek-reasoner", "deepseek-chat"],
            "transformer": {
                "use": ["deepseek"]
            }
        }
    ],
    "Router": {
        "default": "deepseek,deepseek-chat",
        "background": "deepseek,deepseek-chat",
        "think": "deepseek,deepseek-reasoner",
        "longContext": "deepseek,deepseek-chat"
    }
}

```

*   `background`: 用于处理背景任务，对智能要求不高，`deepseek-chat` 足够。
*   `think`: 用于执行核心的思考和规划任务，我们使用最强的 `deepseek-reasoner`。
*   `longContext`: 当上下文长度较长时使用。

### 方案二：混合搭配（DeepSeek + Qwen 长文本）

DeepSeek 的上下文窗口有限（约 64k）。当你需要处理非常大的文件或项目时，可能会超出限制。这时，我们可以引入支持更长上下文的模型，比如硅基流动（SiliconFlow）提供的 128k 上下文的 Qwen 模型。

```json
{
    "log": true,
    "Providers": [
        {
            "name": "deepseek",
            "api_base_url": "https://api.deepseek.com",
            "api_key": "sk-xxx",
            "models": ["deepseek-reasoner", "deepseek-chat"],
            "transformer": {
                "use": ["deepseek"],
                "deepseek-chat": {
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
        "default": "deepseek,deepseek-chat",
        "background": "deepseek,deepseek-chat",
        "think": "deepseek,deepseek-reasoner",
        "longContext": "siliconflow,Qwen/Qwen3-32B"
    }
}

```

> 在这个配置中，日常任务走 64k 的 DeepSeek，一旦遇到需要处理超长上下文的场景，会自动切换到 128k的 Qwen 模型。

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
            "models": ["deepseek-reasoner", "deepseek-chat"],
            "transformer": {
                "use": ["deepseek"],
                "deepseek-chat": {
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
        "default": "deepseek,deepseek-chat",
        "background": "deepseek,deepseek-chat",
        "think": "deepseek,deepseek-reasoner",
        "longContext": "gemini,gemini-2.5-pro-preview-06-05"
    }
}

```
如果不知道怎么用，可以使用ai写config(将上述代码粘贴给ai,说明自己的要求）

### 动态切换模型

在 `claude-code` 会话中，你随时可以使用 `/model` 命令动态切换当前使用的模型。

```
/model deepseek,deepseek-reasoner
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
