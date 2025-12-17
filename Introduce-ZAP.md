# ZAP 介绍

### ZAP 的扫描方式

#### 1. 拦截代理（Intercepting Proxy）
类似 Burp Suite Proxy：

- ZAP 位于 浏览器 ↔ Web 服务器 中间
- 可以 拦截、查看、修改所有 HTTP/HTTPS 流量
- 用于测试认证流程、业务逻辑漏洞、越权、参数篡改等

#### 2. 自动扫描器（Active Scan）
ZAP 会自动“**发送**”构造的恶意请求来测试漏洞（主动扫描），如：

- SQL 注入
- XSS
- CSRF
- 目录遍历
- 命令注入
- 文件包含漏洞

⚠ 注意：Active Scan 是“主动攻击动作”，可能影响目标系统，必须获得授权才能使用。

#### 3. 被动扫描（Passive Scan）
完全安全，不会发送恶意请求，只分析已有流量，能发现：

- 不安全的 HTTP header
- 敏感信息泄露
- Cookie 设置错误
- 弱安全配置等

#### 4. 爬虫（Spider / AJAX Spider）
自动遍历网站 URL（包括 JS 动态生成的路径），是漏洞扫描的前置步骤。

#### 5. 模糊测试（Fuzzer）
Fuzzer（模糊测试器） 是一种向目标接口或参数批量发送大量异常、恶意或随机输入 的技术，用来观察应用是否出现异常行为，从而发现潜在安全漏洞。

- SQL 注入、XSS 等 输入型漏洞探测
- 爆破密码、Token、文件名等 暴力破解
- 路径遍历、目录枚举等 路径发现
- 通过各种 payload 探测响应差异来 定位漏洞触发点

#### 6. WebSocket 测试
能够拦截、篡改 WebSocket 加密实时通信（许多现代 WebApp、前后端分离项目需要这一能力）

#### 7. API 安全测试
自动生成并执行 API 安全测试，非常适合微服务架构/API 项目。
支持导入：

- Swagger/OpenAPI（在线 URL 或者本地文件-适合不暴露对外 Swagger URL 的场景）
- GraphQL Schema

能发现哪些问题？
- 安全 Header 缺失
- 输入注入（SQLi / XSS）
- 不安全的 JSON 响应
- CORS 配置问题
- 认证漏洞
- 敏感信息泄露
- 弱密码 / 开放端点
- API 不安全的返回码
- HTTP 方法滥用
- 不安全的错误回显

#### 8. CI/CD 自动化（DevSecOps）

- Baseline Scan（zap-baseline.py）：快速、轻量，不会主动攻击。（推荐）可以用于 PR 扫描
- Full Scan（zap-full-scan.py）：会进行主动攻击。适合部署到测试环境后进行
- API Scan（zap-api-scan.py）：用 Swagger/OpenAPI 文件扫描后端 API

---

### 🚀 如果你想开始学习 ZAP，建议从这里入手

1. 使用 ZAP QuickStart 自动扫描一个测试站点（例如 DVWA）
2. 配置浏览器代理，学会拦截 HTTP 流量
3. 练习 Active Scan + Manual Explore + Fuzzer
4. 尝试导入 Swagger 用 ZAP 扫描 API
5. 集成一次简单的 CI 自动扫描
6. 查看更多高级功能：Authentication、Context、Session 等

---

### 参考文档

- [OWASP ZAP 安全测试 - 使用（自动扫描、手动扫描）](https://www.cnblogs.com/fanf/p/17268727.html)
- [OWASP ZAP 使用教程](https://www.cnblogs.com/zeussbook/p/10931092.html)