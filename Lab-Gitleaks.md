## Gitleaks 实践教程

### 概述

Gitleaks 是一款开源的敏感信息/密钥泄露扫描工具（Secret Scanner），用于在 Git 仓库、文件目录、提交历史中发现硬编码的敏感数据，例如：

- API Keys
- 密码
- Token
- 证书
- 数据库连接字符串
- 私钥等

它是一种 SAST（Static Application Security Testing）静态安全分析工具，可以集成在日常开发流程、CI/CD、Pre-commit Hook 中，用来阻止敏感信息被提交到仓库。

### 下载安装

> GitHub 地址：<https://github.com/gitleaks/gitleaks>

在 Release 中选择最新的版本，展开点击 `gitleaks_x.x.x_windows_x64.zip`（这里以 Windows 为例）

![gitleaks-install](./images/gitleaks-install.png)

下载并解压到本地目录后，可以将其添加到系统环境变量（可选），执行以下命令以验证安装完成
```bash
gitleaks --version
```

### 机密扫描

常见使用方式示例

#### 扫描本地文件夹

```bash
gitleaks detect -s D:\workspace\xxx -r D:\gitleaks_report.json --no-git
```

- -s：源码路径
- -r：报告生成路径，默认生成的是 json 格式
- --no-git：不按 Git 仓库方式扫描，而是 直接扫描文件系统中的文件，而不会读取 Git 历史、commit、diff 等内容

![gitleaks-scan](./images/gitleaks-scan.png)

查看输出的 Json 文件

![gitleaks-output](./images/gitleaks-output.png)

### 自定义配置 .gitleaks.toml

`.gitleaks.toml` 是 Gitleaks 的配置文件，用于自定义扫描规则、白名单、排除路径、增加 regex、调整默认规则等。

它能帮助你：

- 控制扫描行为
- 减少误报（false positives）
- 添加自定义敏感信息规则
- 配置忽略目录/文件/正则

Gitleaks 官方文档说明：配置文件支持

- 默认规则扩展
- paths allowlist
- 自定义 regex

典型结构：
```yml
title = "gitleaks config"

[extend]
useDefault = true   # 是否继承默认规则

[allowlist]
paths = [
  '''(.*?)test(.*?)''',   # 忽略哪些路径
]

regexTarget = "line"      # 匹配方式：line,file,secret
regexes = [
  '''client_id''',        # 忽略匹配这些内容
]

[[rules]]
id = "custom-api-key"
description = "自定义 API Key 规则"
regex = '''(?i)(api_key|apikey|api-secret)\s*=\s*[0-9a-zA-Z_-]{16,}'''
entropy = 2.5
```

#### 如何启用自己的 .gitleaks.toml

动手实践：
1. 忽略某些目录（如测试目录）
```yml
[allowlist]
paths = [
  '''(.*?)test(.*?)'''
]
```

2. 忽略某些关键词（减少误报），例如你的代码里出现大量 client_id，但它并不是秘密：
```yml
[allowlist]
regexTarget = "line"
regexes = [
  '''client_id''',
  '''secret_name''',
]
```

扫描时启用该配置
```bash
gitleaks detect --config=.gitleaks.toml
```

![gitleaks-with-config](./images/gitleaks-scan-with-config.png)

### 后续

1. 结合 pre-commit（推荐）或者原生 Git hook，在本地代码提交前进行机密扫描
2. 集成到 DevOps 流程中，在代码合并到主分支时进行机密扫描（若扫描识别到机密则拦截 PR）

### 参考文档

- [Gitleaks完整指南：5分钟掌握代码安全检测神器](https://blog.csdn.net/gitblog_00207/article/details/155877130)