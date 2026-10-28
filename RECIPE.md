# OpenHands Agent Canvas 配方（提取中）

```toml
format = "recipkg/1"

[package]
name = "openhands"
version = "0.1.10"
kind = "software"
description = "面向软件开发 Agent 的自托管控制界面配方草案。"
entry = "BUILD_PROMPT.md"

[dependencies.agent]
git = "https://github.com/recipkg/openhands-agent-sdk.git"
rev = "07f17dc1c75acca310991499d221697968ebdf6a"

[dependencies.design]
git = "https://github.com/recipkg/design-directions.git"
rev = "6fdba350d1effa2d50dc0185095bb91ca7a699ee"
```

来源：[OpenHands/OpenHands](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c)，仓库声明 MIT；此配方及 Agent SDK 依赖均为未完成草案，见 `COVERAGE.md`。
