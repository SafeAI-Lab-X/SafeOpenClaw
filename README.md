# Safe OpenClaw: Windows/macOS x 飞书 / 钉钉 / QQ

[![OpenClaw](https://img.shields.io/badge/OpenClaw-Compatible-blue.svg)](https://github.com/openclaw/openclaw)[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)[![Language](https://img.shields.io/badge/Language-English%20%7C%20%E4%B8%AD%E6%96%87-success)](https://chatgpt.com/c/69b15db3-177c-8321-ad15-7a235e7cba78#)

------

# 🧠 项目简介

**Safe OpenClaw** 提供一套 **极简但有效的安全指导方案**，用于降低 **OpenClaw 智能体在真实环境运行时的安全风险**。

不同于传统需要 **插件 / 沙箱 / 复杂策略系统** 的安全方案，本项目探索一种更加简单、可审计的方式：

> **使用 Markdown 文档直接定义智能体安全规则。**

**OpenClaw** 可以 **直接读取这些安全指南并自动部署安全策略**，从而显著降低用户的配置成本。

该方案主要用于防御以下 **Agent 特有安全风险**：

- Prompt Injection（提示词注入）
- 破坏性系统操作
- Skill / 插件供应链投毒
- 敏感信息外泄
- 权限滥用
- ...

------

# 🎯 适用场景与核心原则

该方案适用于以下 OpenClaw 运行环境：

### 系统环境

- Windows
- macOS *（TODO：待补充文档）*

### 协作平台

- 飞书（Feishu, Lark）
- 钉钉（DingTalk） *（TODO：待补充文档）*
- 微信 / 企业微信 *（TODO：待补充文档）*

### 典型使用方式

- OpenClaw 作为 **自动化助手运行在高权限环境**
- OpenClaw 会 **持续安装和使用 Skills / Scripts / Tools**
- 目标是在能力最大化前提下，实现风险可控与审计可追溯

### 核心原则

- **用户友好**：极低的用户手工配置负担
- **高危风险防控**：高危行为必征求用户同意
- **定期巡检与报告**：定期向巡检并向用户报告核心指标

------

# ⚡ 极简部署（核心特点）

本项目的核心目标是：

> **让 Agent 自己部署安全规则。**

无需：

- 安装安全插件
- 配置复杂策略
- 修改 Agent 框架代码

## 一、Windows 部署流程

### Step 1

打开飞书机器人聊天框，向 OpenClaw 发送脚本`nightly-security-audit-windows.ps1`后，发送指令
```
把这个脚本移动到 openclaw 的 workspace\scripts\ 目录下
```

![提供脚本](fig\windows1.png)

### Step 2
向 OpenClaw 发送 Windows 安全指南文档`OpenClaw-Windows-Guide-zh.md`，然后发送指令（指令可做适当微调）：
```
请完全按照这份指南，为我部署安全措施。
```

等待响应即可完成部署

![提供windows安全指南](fig\windows2.png)

## 二、飞书部署流程

### Step 1

打开飞书机器人聊天框，向 OpenClaw 发送飞书安全指南文档`OpenClaw-Windows-Guide-zh.md`，然后发送指令（指令可做适当微调）：
```
请完全按照这份指南，为我部署安全措施。
```

等待响应即可完成部署


![提供部署指南](fig\feishu1.png)

------

# 🔪 测试样例

1. OpenClaw 成功识别出敏感凭证泄露的风险，并拒绝配合实施该行为。

![拒绝恶意行为1](fig\safe1.png)

2. OpenClaw 成功识别出外发敏感数据 + Webhook 滥用的风险，并拒绝配合实施该行为。

![拒绝恶意行为1](fig\safe2.png)

------

# 📂 项目结构

```
SafeOpenClaw
│
├─ docs
│   ├─ OpenClaw-feishu-Security-Guide-en.md
│   ├─ OpenClaw-feishu-Security-Guide-zh.md
│   └─ OpenClaw-Windows-Security-Guide-en.md
│   └─ OpenClaw-Windows-Security-Guide-zh.md
├─ script
│   └─ nightly-security-audit-windows.ps1
│
└─ README.md
```

------

# 📄 核心文件说明

## [飞书安全指南](docs/OpenClaw-feishu-Guide-zh.md)

- 行为红黄线控制（敏感数据外发、Webhook 滥用等红线行为需征求人工同意，读取用户信息等黄线行为允许执行但需记录审计）
- 供应链安全审计（Skill 安装审查，离线审计全部文件，扫描token 提取、联系人/文档数据外传等风险）
- 消息与权限防泄漏机制（发送前扫描敏感信息、限制消息目标、权限控制）
- 交互日志与巡检（记录交互历史，每6小时生成安全状态报告）

------

## [Windows 安全指南](docs/OpenClaw-Windows-Guide-zh.md)

- 行为红黄线控制（篡改认证、数据外发、权限持久化等红线行为需征求人工同意，提权、安装软件等黄线行为允许执行但需记录审计）
- 供应链安全审计（Skill 安装审查，离线审计全部文件，扫描提示注入、依赖安装诱导等风险）
- 关键文件完整性保护（针对核心配置建立 SHA256 基线）
- 自动化安全巡检（建立每日巡检任务）

------

## [Windows 安全扫描脚本](docs/OpenClaw-Windows-Guide-zh.md)

- 包含 OpenClaw 深度审计、端口与进程监控、文件完整性基线、OpenClaw 定时任务检测等共 12 种安全审计。

------

# ⚠ 免责声明

本项目提供 **安全实践建议**，不能保证系统绝对安全。

------

# 📕 参考资料

- OpenClaw 极简安全实践指南
  [https://github.com/slowmist/openclaw-security-practice-guide]((https://github.com/slowmist/openclaw-security-practice-guide))

------

# 📝 License

本项目采用 [MIT](https://opensource.org/licenses/MIT) 协议。




------

# 🧠 Project Overview

**Safe OpenClaw** provides a set of **simple yet effective security guidance** designed to reduce **security risks when OpenClaw agents run in real-world environments**.

Unlike traditional security solutions that require **plugins / sandboxes / complex policy systems**, this project explores a simpler and more auditable approach:

> **Define agent security rules directly using Markdown documents.**

**OpenClaw** can **directly read these security guidelines and automatically deploy security policies**, significantly reducing user configuration costs.

This approach is primarily designed to defend against the following **Agent-specific security risks**:

- Prompt Injection
- Destructive system operations
- Skill / plugin supply chain poisoning
- Sensitive information disclosure
- Permission abuse
- ...

------

# 🎯 Applicable Scenarios and Core Principles

This approach is suitable for the following OpenClaw runtime environments:

### System Environment

- Windows
- macOS *（TODO: Documentation to be added）*

### Collaboration Platforms

- Feishu(Lark)
- DingTalk *（TODO: Documentation to be added）*
- WeChat / Enterprise WeChat *（TODO: Documentation to be added）*

### Typical Usage Patterns

- OpenClaw runs as an **automation assistant in high-privilege environments**
- OpenClaw will **continuously install and use Skills / Scripts / Tools**
- The goal is to achieve controllable risk and traceable audits while maximizing capabilities

### Core Principles

- **User-Friendly**: Minimal manual configuration burden on users
- **High-Risk Prevention**: High-risk behaviors require user approval
- **Regular Inspection and Reporting**: Regularly inspect and report core metrics to users

------

# ⚡ Minimal Deployment (Core Features)

The core objective of this project is:

> **Let the agent deploy security rules itself.**

No need for:

- Installing security plugins
- Configuring complex policies
- Modifying agent framework code

## I. Windows Deployment Process

### Step 1

Open the Feishu(Lark) bot chat window, send the script `nightly-security-audit-windows.ps1` to OpenClaw, then send the instruction:
```
Move this script to the openclaw workspace\scripts\ directory.
```

![Provide Script](fig\windows1.png)

### Step 2
Send the Windows Security Guide document `OpenClaw-Windows-Guide-zh.md` to OpenClaw, then send the instruction (the instruction can be adjusted as needed):
```
Please deploy security measures according to this guide.
```

Wait for the response to complete the deployment

![Provide Windows Security Guide](fig\windows2.png)

## II. Feishu(Lark) Deployment Process

### Step 1

Open the Feishu(Lark) bot chat window, send the Feishu(Lark) Security Guide document `OpenClaw-feishu-Guide-en.md` to OpenClaw, then send the instruction (the instruction can be adjusted as needed):
```
Please deploy security measures according to this guide.
```

Wait for the response to complete the deployment


![Provide Deployment Guide](fig\feishu1.png)

------

# 🔪 Test Example

1. OpenClaw successfully identified the risk of sensitive credential leakage and refused to cooperate in implementing this action.

![Refuse Malicious Behavior](fig\safe1.png)

2. OpenClaw successfully identified the risk of outgoing sensitive data + Webhook misuse and refused to cooperate in implementing this action.

![Refuse Malicious Behavior](fig\safe2.png)

------

# 📂 Project Structure

```
SafeOpenClaw
│
├─ docs
│   ├─ OpenClaw-feishu-Security-Guide-en.md
│   ├─ OpenClaw-feishu-Security-Guide-zh.md
│   └─ OpenClaw-Windows-Security-Guide-en.md
│   └─ OpenClaw-Windows-Security-Guide-zh.md
├─ script
│   └─ nightly-security-audit-windows.ps1
│
└─ README.md
```

------

# 📄 Core File Description

## [Feishu(Lark) Security Guide](docs/OpenClaw-feishu-Guide-zh.md)

- Behavior Red/Yellow Line Control (Sensitive data outflow, Webhook abuse and other red-line behaviors require manual approval, reading user information and other yellow-line behaviors are allowed but require audit logging)
- Supply Chain Security Audit (Skill installation review, offline audit of all files, scanning for token extraction, contact/document data transmission risks, etc.)
- Message and Permission Information Leakage Prevention (Scan for sensitive information before sending, limit message targets, permission control)
- Interaction Logs and Inspection (Record interaction history, generate security status reports every 6 hours)

------

## [Windows Security Guide](docs/OpenClaw-Windows-Guide-zh.md)

- Behavior Red/Yellow Line Control (Credential tampering, data outflow, privilege persistence and other red-line behaviors require manual approval, privilege escalation, software installation and other yellow-line behaviors are allowed but require audit logging)
- Supply Chain Security Audit (Skill installation review, offline audit of all files, scanning for prompt injection, dependency installation inducement risks, etc.)
- Critical File Integrity Protection (Establish SHA256 baseline for core configuration)
- Automated Security Inspection (Establish daily inspection tasks)

------

## [Windows Security Scan Script](docs/OpenClaw-Windows-Guide-zh.md)

- Contains 12 types of security audits including OpenClaw deep audit, port and process monitoring, file integrity baseline, OpenClaw scheduled task detection, etc.

------

# ⚠ Disclaimer

This project provides **security best practices** and cannot guarantee absolute system security.

------

# 📕 References

- OpenClaw Minimal Security Practice Guide
  [https://github.com/slowmist/openclaw-security-practice-guide]((https://github.com/slowmist/openclaw-security-practice-guide))

------

# 📝 License

This project is licensed under [MIT](https://opensource.org/licenses/MIT).
