# Telegram 消息监控程序

[English](./README_EN.md)

## 项目简介 
本项目是一个基于 Python 的 Telegram 消息监控程序，利用 `Telethon` 库与 Telegram API 进行交互。  
程序在更新后，支持以下主要特性：  
- 监听指定对话的消息，匹配关键词、正则表达式或文件后缀名。
- 自动转发匹配消息、发送邮件通知、记录匹配消息到本地文件。
- 定时发送消息，可选择随机延时与发送后删除。
- 对于带有 Inline 按钮和图片的消息，可自动将图片上传到 AI 模型进行识别，从 AI 的回应中选择并点击按钮。
- 如 AI 模型调用失败，会在10秒后重试一次，若仍失败则放弃本次处理。
- 对于正则匹配结果，可将匹配的文本内容转发到指定对话（支持延时与发送后删除）。
- 全量监控与用户过滤功能，可根据用户ID、用户名或昵称选择性监听消息。
- 支持用户自己配置代理ip

## 功能特性

- **关键词监控**：  
  根据完全匹配、关键词匹配或正则表达式匹配规则，对指定对话中的消息进行监控。  
  可选择自动转发、邮件通知、记录到本地文件，以及对正则匹配结果自动发送到指定对话。

- **文件后缀名监控**：  
  对指定文件类型（后缀名）的文件消息进行监控，同样可自动转发、邮件通知。

- **全量监控**：  
  可对指定频道、群组或对话进行全量监控，监听所有消息。

- **用户过滤**：  
  可根据用户ID、用户名或昵称只监控特定用户的消息。

- **定时消息**：  
  使用 Cron 表达式添加定时消息任务，可指定随机延时和发送后删除。

- **按钮与图片处理**：  
  当检测到图片与 Inline 按钮时，将图片上传给 AI 模型进行识别，根据模型回答自动点击相应按钮。  
  若第一次模型调用失败，则10秒后重试一次，若仍失败则放弃上传并删除本地图片文件。

- **日志记录**：  
  使用日志文件记录程序运行过程、错误信息。

## 环境要求

- Python 3.7 及以上
- 安装依赖包（Telethon、opanai、pytz、apscheduler等）
- 需要 Telegram `api_id` 和 `api_hash`
- 需要提供 SMTP 邮箱信息（如需邮件通知）
- 需要 One/New API 的 `api_key` 和 `base_url` 用于调用 AI 模型（如需图片识别）

## 安装指南

1. 克隆或下载本项目代码：
```bash
   git clone https://github.com/djksps1/telegram-monitor.git
 
```

1. 安装依赖：
```bash
   pip install -r requirements.txt

```
2. 获取 Telegram API 凭证（`api_id` 和 `api_hash`）。
3. 配置 `ONE/NEW API_KEY` 和 `ONE/NEW API_BASE_URL` 来使用 AI 模型服务。
4. 配置 SMTP 邮箱信息（如需邮件通知）。
## 使用说明

1. **运行程序**：
```bash
   python monitor.py

```
2. **登录 Telegram**：程序首次运行需要输入 `api_id` 和 `api_hash`，然后输入您的 Telegram 手机号和验证码进行登录。如果启用了两步验证，还需输入密码。
3. **设置监控参数**：程序启动后会显示可用命令列表，可根据需求添加、修改监控关键词、文件后缀名监控、全量监控、定时任务等。
4. **启动监控**：配置完成后，输入 `start` 即可开始监控。
## 可用命令列表

- `list`：列出所有对话
- `addkeyword` / `modifykeyword` / `removekeyword` / `showkeywords`：管理关键词监控
- `addext` / `modifyext` / `removeext` / `showext`：管理文件后缀名监控
- `addall` / `modifyall` / `removeall` / `showall`：管理全量监控配置
- `schedule` / `modifyschedule` / `removeschedule` / `showschedule`：管理定时消息任务
- `addbutton` / `modifybutton` / `removebutton` / `showbuttons`：管理按钮关键词监控
- `addimagelistener` / `removeimagelistener` / `showimagelistener`：管理针对图片+按钮消息的监听对话ID
- `start` / `stop`：启动或停止监控
- `exit`：退出程序
## 功能细节

- **关键词监控**：可指定匹配类型（exact、partial、regex），并为 regex 类型配置匹配结果发送目标对话、延时和自动删除选项。
- **文件后缀名监控**：当检测到指定后缀名文件时自动转发、邮件通知。
- **全量监控**：可对指定频道或群组监听所有消息，并配置自动转发、邮件通知和用户过滤。
- **按钮关键词监控**：当检测到带有 Inline 按钮的消息时，可自动点击包含指定关键词的按钮。
- **图片与 AI 模型处理**：当检测到带图片与按钮的消息，将图片上传给 AI 模型获取回答。若失败则10秒后重试一次，仍失败则放弃。成功后根据返回内容自动点击对应按钮。完成后立即删除本地图片文件。
- **定时消息**：使用 Cron 表达式指定定时消息发送时间，可配置随机延时与发送后删除。
## 日志记录

- 日志文件：`telegram_monitor.log`
- 记录程序运行状态、错误信息与关键事件。
## 注意事项

- 请遵守 Telegram 使用条款，避免滥用导致账号受限。
- 妥善保管您的 `api_id`、`api_hash` 和登录会话文件，防止账号信息泄露。
- 若需要邮件通知，请正确配置 SMTP 信息和邮箱授权码。
- 配置 One/NEW API 的 `api_key` 与 `base_url` 用于模型调用。
## 常见问题

1. **无法连接 Telegram**：检查网络与代理配置，确保能访问 Telegram。
2. **邮件发送失败**：确保 SMTP 配置正确、邮箱授权码已设置。
3. **AI 模型调用失败**：程序会自动重试一次，若仍失败则放弃本次处理并删除图片。
4. **定时任务不执行**：检查 Cron 表达式与时区配置，确保 `scheduler` 已启动。
## 贡献与支持

欢迎提出建议和改进。如有问题或想要新增功能，可提交 issue 或 pull request。

## 许可证

本项目采用 MIT 许可证。
