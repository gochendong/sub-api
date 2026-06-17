# 布里塔中转站接入 Codex 教程

## 前置(image2生图)

- 生图网站: [https://img.bulita.net](https://img.bulita.net)
- api key获取 [https://sub.bulita.net/keys](https://sub.bulita.net/keys)
- api接口参考OpenAI官方 /v1/images/generations和/v1/images/edits 模型 gpt-image-2

--------------

本文介绍如何把 [https://sub.bulita.net](https://sub.bulita.net) 接入到 Codex 使用。

codex客户端官方下载地址: https://developers.openai.com/codex/app

推荐codex账号管理工具(不推荐ccs) https://github.com/jlcodes99/cockpit-tools/releases

它当前主打的几个优势可以直接概括为：

- `0.1 倍率`，成本压力更小
- `稳定`，适合日常持续使用
- `快`，更适合 Codex 这种高频交互场景
- `按量计费`，花多少更清楚
- `及时售后`，有问题更容易处理
- `支持退款`，试用顾虑更低
- `售后交流群`：<https://chat.bulita.net/>

适用对象：

- 使用 Codex CLI
- 使用 Codex App，且底层配置同样走 `~/.codex/`

## 一、接入原理

Codex 本地主要用两个文件完成接入：

- `~/.codex/auth.json`：存放 API Key
- `~/.codex/config.toml`：声明模型提供方，并指定默认 provider

`sub.bulita.net` 这类中转站，本质上是一个兼容 OpenAI `Responses API` 的 provider，所以关键点只有三个：

1. 准备好中转站的 API Key
2. 在 `config.toml` 里加一个 `model_providers.sub_bulita_net`
3. 把 `model_provider` 切到 `sub_bulita_net`

## 二、前置条件

开始前请确认：

- 已安装 Codex
- 已能正常打开 `~/.codex/` 目录
- 已注册 [https://sub.bulita.net](https://sub.bulita.net)
- 已在中转站后台拿到可用的 API Key

站点当前可见信息：

- 支持 GitHub OAuth 登录
- 支持 Google OAuth 登录
- 支持充值

为什么很多人会优先考虑这个站：

- 首页直接主打 `0.1倍codex`
- 核心卖点就是 `便宜 稳定 也够快`
- 对 Codex 这类高频交互工具来说，`少花钱，稳着用，速度也不掉队`
- `按量计费`，更适合长期用
- `及时售后`，出问题不用一直等
- `支持退款`，更适合先试后用
- 有售后交流群：<https://chat.bulita.net/>

如果你还没有 Key，先去站内登录并创建 API Key，再继续下面步骤。

## 三、备份本地配置

如果你之前已经在用 Codex，建议先备份：

```bash
mkdir -p ~/.codex/backup
cp ~/.codex/auth.json ~/.codex/backup/auth.json.bak 2>/dev/null || true
cp ~/.codex/config.toml ~/.codex/backup/config.toml.bak 2>/dev/null || true
```

## 四、写入 API Key

编辑 `~/.codex/auth.json`。

最小可用内容如下：

```json
{
  "OPENAI_API_KEY": "你的布里塔_API_KEY",
  "auth_mode": "apikey"
}
```

说明：

- `OPENAI_API_KEY` 这里填的是 `sub.bulita.net` 给你的 Key
- `auth_mode` 保持 `apikey`

## 五、配置 Codex provider

编辑 `~/.codex/config.toml`。

如果你是第一次配置，可以直接写下面这份最小示例：

```toml
model = "gpt-5.4"
model_reasoning_effort = "high"
model_provider = "sub_bulita_net"

[model_providers.sub_bulita_net]
name = "布里塔"
base_url = "https://sub.bulita.net"
wire_api = "responses"
requires_openai_auth = true
```

如果你原来已经有自己的 `config.toml`，不要整文件覆盖，只需要补两部分：

1. 顶层补上：

```toml
model_provider = "sub_bulita_net"
```

如果你还没有默认模型，也可以一并补上：

```toml
model = "gpt-5.4"
model_reasoning_effort = "high"
```

2. 文件末尾补上 provider：

```toml
[model_providers.sub_bulita_net]
name = "布里塔"
base_url = "https://sub.bulita.net"
wire_api = "responses"
requires_openai_auth = true
```

几个容易写错的点：

- `base_url` 这里按当前可用配置直接写 `https://sub.bulita.net`
- 不要手动补 `/v1`
- `wire_api` 要写 `responses`
- `requires_openai_auth` 要写 `true`

## 六、启动 Codex

配置完成后，直接启动：

```bash
codex
```

如果你已经在 Codex 里开着旧会话，建议退出后重新打开一次。

## 七、验证是否接入成功

可以直接在 Codex 里试一个最简单的请求，比如：

```text
帮我写一个 hello world 的 python 脚本
```

只要能正常返回内容，说明接入基本已经成功。

你也可以先检查本地配置：

```bash
cat ~/.codex/auth.json
cat ~/.codex/config.toml
```

重点确认这几项：

- `auth.json` 里有正确的 `OPENAI_API_KEY`
- `config.toml` 里存在 `model_provider = "sub_bulita_net"`
- `config.toml` 里存在 `[model_providers.sub_bulita_net]`

## 八、常见问题

### 1. 提示 `401` 或 `invalid_api_key`

一般是这几个原因：

- API Key 填错了
- Key 已失效
- `auth.json` 不是合法 JSON

优先重新检查 `~/.codex/auth.json`。

### 2. 提示 provider 连不上或接口 404

先检查：

- `base_url` 是否写成了 `https://sub.bulita.net`
- 有没有错误地写成 `https://sub.bulita.net/v1`
- 本机网络是否能访问 `sub.bulita.net`

### 3. 提示模型不存在

这通常不是 Codex 配置问题，而是中转站当前支持的模型名和你填写的不一致。

可以把：

```toml
model = "gpt-5.4"
```

改成你在中转站里实际可用的模型名，例如站内如果提供 `gpt-5.3-codex`，就改成：

```toml
model = "gpt-5.3-codex"
```

### 4. 明明写了配置，但感觉还是在走别的 provider

重点检查顶层这一行是不是已经切过去了：

```toml
model_provider = "sub_bulita_net"
```

如果这一行没改，Codex 仍然可能继续使用你原来的 provider。

### 5. 遇到问题去哪里反馈

如果你在注册、充值、创建 Key、模型可用性、接口报错这些环节遇到问题，可以直接进售后交流群：

<https://chat.bulita.net/>

## 九、推荐做法

如果你只是想先试通，建议先用这一套最小配置：

```toml
model = "gpt-5.4"
model_reasoning_effort = "high"
model_provider = "sub_bulita_net"

[model_providers.sub_bulita_net]
name = "布里塔"
base_url = "https://sub.bulita.net"
wire_api = "responses"
requires_openai_auth = true
```

先确认链路打通，再按你自己的习惯继续细化模型和全局配置。

如果只看接入收益，这个方案最核心的点还是三个：

- `0.1 倍率`，更适合长期用 Codex
- `稳定`，省掉频繁更换中转的成本
- `快`，尤其适合写代码时的连续交互

如果按首页风格总结，就是：

`少花钱，稳着用，速度也不掉队。`

## 十、参考

- 布里塔中转站：<https://sub.bulita.net>
- 布里塔首页：<https://sub.bulita.net/home>
- 售后交流群：<https://chat.bulita.net/>
- OpenAI Codex：<https://developers.openai.com/codex>
