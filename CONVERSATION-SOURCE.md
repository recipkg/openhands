# Canvas 会话主流程：源码提取

以下来自 [OpenHands Agent Canvas 固定提交 `17af119c331a87e881417e4afe95f83dd3c5cb2c`](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c) 的前端源码和测试设计，**未运行 Canvas 或浏览器**。关联服务端实测在 `agent` 依赖的 `SESSION-SOCKET-OBSERVED.md`；两者合起来也不证明跨仓库兼容。云端、代理和 ACP 仍须分别实测。

## 启动与会话入口

- 上游 [模拟模型端到端测试设计](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/tests/e2e/mock-llm/conversations/mock-llm-conversation.spec.ts) 描述：设置并启用模型配置，首页输入任务并提交，创建会话后进入会话页，看到用户消息、Agent 回复和侧边栏中的会话；离开后从侧边栏打开同一会话时历史仍可见。该测试设计还核对工具执行与请求中的 worktree 设置，但这里未执行测试，不将其当作运行结果。
- [创建服务源码](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/api/conversation-service/agent-server-conversation-service.api.ts) 区分本地与云端：本地路径需要 Agent profile，解析工作区并提交包含初始用户消息的创建请求；云端先提交启动任务，实际会话可能尚未就绪。已有会话重新打开不应再次发送初始消息。
- [输入提交逻辑](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/hooks/chat/use-chat-submission.ts) 用去除首尾空白后的结果判断是否为空；无附件且内容为空时不提交，有附件可提交空文本。提交回调收到原始文本，随后清空输入框；这只是组件逻辑，不证明附件上传、失败恢复或服务端接收成功。

## 历史与实时事件

- [历史加载源码](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/hooks/query/use-conversation-history.ts) 首次读取事件计数，再以时间倒序请求最近 50 条并反转为显示顺序；向上滚动加载较早页面。本地以计数推导初始序号游标，云端考虑历史副本延迟而回退一页；源码指出云端落后超过一页仍可能有缺口。计数不可用时使用全量重放作为回退。
- [会话连接源码](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/contexts/conversation-websocket-context.tsx) 等首次历史加载结束再连接 `/sockets/session/{id}`；连接使用 `after_seq`，接收持久帧时推进恢复游标，临时状态及流式进度不充当持久游标。事件按 ID 去重；切换会话时清理先前会话事件和游标。源码部分旧注释仍提到时间戳，实际连接参数以代码中的 `after_seq` 为准。
- 序号不必按到达顺序递增：[游标源码](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/utils/session-seq-cursor.ts) 跟踪连续已见位置，避免简单使用最大值而跳过缺口。其对永久缺失序号的处理与限制需单独验收；不能据此声称任意故障下都无遗漏。

## 消息交付和断线

- [连接源码](https://github.com/OpenHands/OpenHands/blob/17af119c331a87e881417e4afe95f83dd3c5cb2c/src/contexts/conversation-websocket-context.tsx) 在 socket 已打开时发送用户消息；未打开但运行时可达时通过 HTTP 发到该运行时并请求运行，之后靠事件重放显示。云端运行时还没有地址时，会暂存消息等待连接；发送错误会向界面报告。计划模式没有就绪的目标会话时不回退发送给主 Agent。
- 用户消息在收到对应事件前可以显示为待发送；源码用历史或实时事件消除匹配的临时消息，避免重复展示。临时状态、完整发送确认、上传、离线缓存、重试幂等和进程关闭后的未发送数据都还需要浏览器实测，不假设已持久保存或恰好送达一次。
