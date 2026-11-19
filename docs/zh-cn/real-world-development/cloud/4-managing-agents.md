---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体会异步且自主地工作，但仍需要监控这些任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的智能体任务、了解它们执行的操作，甚至引导正在运行的会话。

本课将：

- 探索智能体页面并监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

团队在分配高对比度模式任务后，意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，将这项功能也纳入工作范围很合理。你将引导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 访问 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 为修改游戏生成 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时查看右侧显示的拉取请求 (PR) 窗格。在此可以查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

你已查看正在进行的任务。现在要求 Copilot 在实现高对比度模式时，也加入浅色模式切换功能。

1. 选择表示添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。出现类似下方的消息时，就表示它已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会在完成收到消息时正在执行的具体步骤后，将新请求纳入工作流程。

这需要几分钟，可以利用这段时间暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以在此跟踪所有正在进行和已完成的任务、审查正在执行的工作，也可以重定向正在运行的任务以调整范围或补充指导。

你探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在运行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
