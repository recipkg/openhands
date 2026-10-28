# 覆盖与证据

- [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c) 指定提交 `17af119c331a87e881417e4afe95f83dd3c5cb2c`；仓库声明 MIT。当前上游 README 将该仓库定义为 Agent Canvas，SDK/Agent Server 和 automation 是独立仓库。
- 已阅读 README、主会话创建与事件同步源码，以及上游模拟模型的端到端测试设计；见 `CONVERSATION-SOURCE.md`。尚未运行 Canvas、浏览器、实际模型、自动化服务或独立重建。
- 界面详细流程、API、持久化、权限、ACP、自动化、多用户和部署边界均未完整提炼；`agent` 依赖本身也是草案。禁止把结构检查当功能验证。
- `agent` 固定到 SDK 配方 v0.1.5，该配方已实测单个自定义工具的本机会话、确认及恢复，文件编辑器与 subprocess 终端，以及服务端认证、正常重启、会话 WebSocket 序号恢复与实时消息的局部行为；这些不是 Canvas 浏览器、Canvas 到服务端互操作或完整 SDK 的验收结果。
