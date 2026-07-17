# 布里塔中转站接入 Codex 教程

## 前置(image2生图)

- 生图网站: [https://img.bulita.net](https://img.bulita.net)
- api key获取 [https://sub.bulita.net/keys](https://sub.bulita.net/keys)
- api接口参考OpenAI官方 /v1/images/generations和/v1/images/edits 模型 gpt-image-2

## 使用官方Codex / 插件

codex客户端官方下载地址: https://developers.openai.com/codex/app

codex账号开源管理工具 https://github.com/jlcodes99/cockpit-tools/releases

### 其他客户端选择

如果不想使用官方Codex, 建议使用更方便的Zcode客户端: https://zcode.z.ai/cn

1. 设置
2. 模型设置
3. 添加供应商
4. 填入 base_url 和 api_key, API格式选择Responses, 填入你想调用的模型比如 gpt-5.6-sol 
5. 返回工作区

## 常见问题

### 1. 提示 `401` 或 `invalid_api_key`

- API Key 填错了

### 2. 提示模型不存在

这通常不是 Codex 配置问题，而是中转站当前支持的模型名和你填写的不一致。

## 参考

- 布里塔中转站：<https://sub.bulita.net>
- 售后交流群：<https://hi.bulita.net/>
