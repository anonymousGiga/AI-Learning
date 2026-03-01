# ZeroClaw + GLM-4.6 配置指南

本指南介绍如何安装 ZeroClaw 并配置使用智谱 GLM-4.6 模型。

## 前置要求

- Linux 系统（本指南在 Ubuntu/Debian 环境下测试）
- 智谱 AI API Key（从 [智谱AI开放平台](https://open.bigmodel.cn/) 获取）

## 安装步骤

### 1. 安装 Rust 工具链

ZeroClaw 使用 Rust 编写，需要先安装 Rust。推荐使用官方安装脚本：

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装过程中会提示确认安装，输入 `1` 并回车继续。

安装完成后，重新加载环境变量：

```bash
source $HOME/.cargo/env
```

验证 Rust 是否安装成功：

```bash
cargo --version
rustc --version
```

### 2. 克隆仓库

```bash
git clone https://github.com/zeroclaw-labs/zeroclaw.git
cd zeroclaw
```

### 3. 运行引导脚本

```bash
./bootstrap.sh
```

脚本会询问一系列配置问题。如果不确定，可以一路按回车使用默认值，稍后再手动修改配置文件。

### 4. 构建安装

```bash
cargo install --path .
```

安装完成后，可以使用 `zeroclaw --help` 验证安装是否成功。

## 配置 GLM-4.6

ZeroClaw 的配置文件位于 `~/.zeroclaw/config.toml`。

### 配置模型提供商

编辑配置文件，添加或修改 `[model_providers]` 部分：

```toml
[model_providers.glm]
name = "anthropic-custom:https://open.bigmodel.cn/api/anthropic"
api_key = "YOUR_API_KEY_HERE"
default_model = "glm-4.6"
```

**字段说明：**

| 字段 | 说明 |
|------|------|
| `glm` | 配置名称（可自定义） |
| `name` | 提供商类型，`anthropic-custom:` 前缀表示自定义 Anthropic 兼容端点 |
| `api_key` | 你的智谱 API Key |
| `default_model` | 默认模型名称 |

### 设置默认提供商

确保配置文件顶部的 `default_provider` 指向你配置的提供商名称：

```toml
default_provider = "glm"
```

## 验证配置

运行 ZeroClaw 交互模式：

```bash
zeroclaw agent
```

进入交互模式后，发送测试消息：

```
> 你好，请介绍一下你自己
```

如果配置正确，GLM-4.6 模型会响应你的消息。

## 完整配置示例

```toml
default_provider = "glm"
default_temperature = 0.7
model_routes = []
embedding_routes = []

[model_providers.glm]
name = "anthropic-custom:https://open.bigmodel.cn/api/anthropic"
api_key = "YOUR_API_KEY_HERE"
default_model = "glm-4.6"

[observability]
backend = "none"

[autonomy]
level = "supervised"
workspace_only = true

# ... 其他配置项 ...
```

## 常见问题

### Q: 为什么使用 `anthropic-custom:` 前缀？

A: ZeroClaw 使用此格式来识别自定义的 Anthropic API 兼容端点。智谱的此端点遵循 Anthropic 的消息格式。

### Q: 可以配置多个模型提供商吗？

A: 可以，在 `[model_providers]` 下添加多个配置，例如 `[model_providers.glm]`、`[model_providers.openai]` 等。

### Q: 如何查看日志排查问题？

A: ZeroClaw 会输出详细的日志信息，包括连接状态、错误原因等，便于排查配置问题。

## 参考资源

- [ZeroClaw GitHub](https://github.com/zeroclaw-labs/zeroclaw)
- [智谱AI开放平台](https://open.bigmodel.cn/)
- [ZeroClaw 完整文档](../README.md)
