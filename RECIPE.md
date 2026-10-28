# OpenHands Agent Canvas 配方（提取中）

```toml
format = "recipkg/1"

[package]
name = "openhands"
version = "0.1.0"
kind = "software"
description = "面向软件开发 Agent 的自托管控制界面配方草案。"
entry = "BUILD_PROMPT.md"

[dependencies.agent]
git = "https://github.com/recipkg/openhands-agent-sdk.git"
rev = "8c8218e03c8d4fcf58451a9ae987ad86e83064d3"

[dependencies.design]
git = "https://github.com/recipkg/design-directions.git"
rev = "f86b3eca87356a073274248eb48b6a5b6756db59"
```

来源：[OpenHands/OpenHands](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c)，仓库声明 MIT；此配方及 Agent SDK 依赖均为未完成草案，见 `COVERAGE.md`。
