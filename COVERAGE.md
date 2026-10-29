# 覆盖与证据

- [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c) 指定提交 `17af119c331a87e881417e4afe95f83dd3c5cb2c`；仓库声明 MIT。当前上游 README 将该仓库定义为 Agent Canvas，SDK/Agent Server 和 automation 是独立仓库。
- 已阅读 README、主会话创建与事件同步源码，以及上游模拟模型的端到端测试设计；见 `CONVERSATION-SOURCE.md`。在真实浏览器运行 npm Canvas v1.34.0 发布包、本机模拟模型和固定上游 Agent Server，观察本地会话与重进历史，见 `BROWSER-OBSERVED.md`。该发布包的构建源码与配方固定提交不同。另构建固定 Canvas 提交，在无头浏览器实测认证错误、一次终端工具结果与同会话恢复，见 `AUTH-ERROR-BROWSER-OBSERVED.md`；未运行实际模型、自动化服务或独立重建。
- 界面详细流程、API、持久化、权限、ACP、自动化、多用户和部署边界均未完整提炼；`agent` 依赖本身也是草案。禁止把结构检查当功能验证。
- `agent` 固定到 SDK 配方 v0.1.10，该配方已实测单个自定义工具的本机会话、确认及恢复，文件编辑器与 subprocess 终端，服务端和 WebSocket 局部行为，以及项目技能发现和模拟模型驱动的调用样本。另实测模拟模型认证失败后事件与本机会话恢复；Canvas 固定源码已观察一条终端工具执行后认证失败和恢复路径；技能调用仍未通过 Canvas 界面验收。浏览器样本不代表完整互操作或完整 SDK 的验收结果。
