# Canvas 认证失败、工具记录与恢复：固定源码浏览器观察

使用 [OpenHands/OpenHands 固定提交 `17af119c331a87e881417e4afe95f83dd3c5cb2c`](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c) 的 Canvas 源码，Node.js 26.12.1、npm 12.2.0，执行 `npm ci --ignore-scripts --no-audit --no-fund` 和 `npm run build` 后，以无头 Chromium 打开构建结果。Agent Server 是 `agent` 配方记录的 SDK 固定提交 `46b1fa5cb91ada17e6ec938c7e8624a4fa19fa98`，Python 3.13.16，依照其 `uv.lock` 安装。测试使用独立临时 HOME、工作区、会话目录、回环代理和 OpenAI 兼容模拟模型；模型、会话密钥和错误消息均为虚构。浏览器拦截非回环请求并拒绝遥测；自动化服务不可用，相关请求返回 404。这不是操作系统沙箱。

## 工具执行后失败并恢复

1. 保存使用模拟模型的配置，在一次性工作区发送任务。模拟模型先请求终端工具执行 `printf 'executed\n' >> fixture-marker.txt`，随后返回 HTTP 401 和模拟认证错误。实际文件写入一行 `executed`；界面展示执行过的命令，并显示 `Error` 状态。
2. 重载同一会话后，错误状态、原消息和已执行命令仍在；展开工具项可见命令和“执行完成、无输出”的观察结果。当前页面的无头浏览器可访问性快照没有展示模拟上游错误原文或更详细的认证解释；未检查所有图标、面板和辅助技术，不能推断其他入口一定没有细节。
3. 将模拟模型切回正常响应，发送一条新的后续用户消息。界面状态变为完成，原来的工具项与结果和新消息仍在，服务端会话状态为 `finished`，测试文件仍只有一行。模拟模型未再次发出该工具调用；这不能证明重复请求去重、恰好一次执行或崩溃恢复。
4. 受测试会话密钥保护的服务端事件查询包含两条同代码 `LLMAuthenticationError`：一条为密钥无效或过期的友好提示，另一条包含模拟上游错误标记 `fixture-invalid-credential`。恢复后两条仍可查询。模拟字符串并非真实密钥；此样本不是所有错误路径或日志的秘密泄露审计。

另有一次未先执行工具的 401 测试：页面显示错误、重载后原消息和状态保留，发送后续消息并恢复模拟模型后会话完成。保存模型配置时界面会调用服务端验证；模拟端点先返回 401 时编辑器保持打开，切回成功才保存。此处没有完整验证配置错误的文字呈现。

手动测试夹具在 [standard-library-verification](https://github.com/recipkg/standard-library-verification)：以实际构建的 Canvas 目录及 SDK 环境启动，先保存模拟模型配置，再按其 README 切换 `model-mode`。该测试是上游固定源码的一条可观察路径，不是独立重建验收；真实模型、异常进程终止、其他故障、移动端、键盘无障碍和自动化服务均未测试。之前 npm 发布包的不同提交样本仍见 `BROWSER-OBSERVED.md`。
