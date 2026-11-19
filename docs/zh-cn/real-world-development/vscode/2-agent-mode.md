---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发人员的工作流程一般包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再进行更新……如此反复。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。下面了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但它们针对两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的桌游众筹平台，可供用户列出游戏并查看详情。它是一个单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中，交互功能由使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端窗口。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的专用隧道，此时 Web 服务器正在 Codespace 中运行。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。不同练习所需的模式和模型可能不同；需要时会在相应步骤中说明。

网站的初始实现可以正常运行，但我们希望通过添加新功能进一步改进它。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题；请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并不偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列在说明中。这样做是希望代码建议相对一致，以提供良好的体验。不过，大型语言模型的输出具有概率性，因此你收到的建议可能与课程中的示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用其他 MCP 命令，但仍应能完成任务。

3. 使用以下提示，请 Copilot 查看存储库待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 查看说明文件

让代理生成代码前，先查看将用于向 Copilot 提供上下文的说明文件。我们将使用[用户界面 (UI) 说明][ui-instructions]，其中记录了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这样可以复用内容、将复杂说明拆分成较小且易于管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行以下三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并更改前还必须运行并通过测试。Copilot 代理模式可以为你完成这些任务。现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍选中 **Agent**。

   ![Chat 视图中的智能体选择器屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项；在工作进行期间无需选择它们。

7. 按 Copilot 的提示选择 **Continue** 以运行测试。

   ![Copilot Chat 窗格中的对话框屏幕截图，提示用户确认是否要运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直至完成任务且未检测到错误。

   ![已完成的 Copilot 代理模式 Chat 会话屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你学会了：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 - 实现分页

随着游戏列表不断增长，需要添加分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。制定方案时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 编写提示时，为 Copilot 提供实现所需的指导。
- 你可能需要与 GitHub Copilot 迭代，通过追加请求和上下文来调整结果。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
