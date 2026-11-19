---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改进用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在启用团队其他成员的审查之前，先审查 GitHub Copilot 为添加文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对用于提交代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来处理任务。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

首先在 GitHub.com 上通过标准 PR 界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，它为代码添加了文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器调用。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以利用这段时间休息或回顾目前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些规则来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像在 PR 中提及其他团队成员一样，通过评论或代码行内评论提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，无法提供一条适用于所有情况的固定提示。可以要求 Copilot 完成以下任一更新：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中提出文档更改请求，并像提及用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望添加到代码库中的文档改进。
2. 选择 **View Session**，查看 Copilot 如何执行工作。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会显示新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认结果符合要求后，可以将 PR 从草稿转换为可审查状态，再合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 创建的 PR，它用于实现游戏详情页中的相关游戏功能。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更完善的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，你可以通过迭代方式向 Copilot 云智能体请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中通过任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在重新加载页面后仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

当你可以将真实待办任务交给云智能体并转而处理其他工作时，它最有价值。可以在自己的存储库中创建更多议题并分配给 Copilot，养成这种工作习惯。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习路径。在这些课程中，你：

- **检查了此存储库自带的自定义说明文件**，以便稍后观察它们对云智能体输出的影响。
- **向 Copilot 云智能体分配了议题**，并查看它如何设置环境、制定计划和异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进了云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体练习路径已完成。其他练习路径可以补充你在此处学到的内容：

- 🖥️ **[VS Code 练习路径](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体连接。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
