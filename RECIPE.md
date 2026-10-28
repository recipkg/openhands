# OpenHands Agent Canvas 配方（提取中）

```toml
format = "recipkg/1"

[package]
name = "openhands"
version = "0.1.7"
kind = "software"
description = "面向软件开发 Agent 的自托管控制界面配方草案。"
entry = "BUILD_PROMPT.md"

[dependencies.agent]
git = "https://github.com/recipkg/openhands-agent-sdk.git"
rev = "ec6c76ef23cced085ce38ee4840dd50c6a778961"

[dependencies.design]
git = "https://github.com/recipkg/design-directions.git"
rev = "2546cea48637585ec2e83b297b12b08548f9d15f"
```

来源：[OpenHands/OpenHands](https://github.com/OpenHands/OpenHands/tree/17af119c331a87e881417e4afe95f83dd3c5cb2c)，仓库声明 MIT；此配方及 Agent SDK 依赖均为未完成草案，见 `COVERAGE.md`。
