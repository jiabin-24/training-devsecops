## Dependency-Check 入门

### 概述
Dependency-Check 是 OWASP（Open Web Application Security Project）的一个实用开源程序，用于识别项目依赖项并检查是否存在任何已知的，公开披露的漏洞。目前，已支持 Java、.NET、Ruby、Node.js、Python 等语言编写的程序，并为 C/C++ 构建系统（autoconf 和 cmake）提供了有限的支持。而且该工具还是 OWASP Top 10 的解决方案的一部分。

Dependency-Check 支持面广（支持多种语言）、可集成性强，作为一款开源工具，在多年来的发展中已经支持和许多主流的软件进行集成，比如：命令行、Ant、Maven、Gradle、Jenkins、Sonar 等；具备使用方便，落地简单等优势。

### 实现原理
依赖性检查可用于扫描应用程序（及其依赖库），执行检查时会将 Common Platform Enumeration（CPE）国家漏洞数据库及 NPM Public Advisories 库下载到本地，再通过核心引擎中的一系列分析器检查项目依赖性，收集有关依赖项。

### NVD 概述
Dependency-Check 依赖 NVD 漏洞数据库（美国国家通用漏洞数据库）进行依赖漏洞检查（全球信息安全领域著名的漏洞数据库包括中国国家信息安全漏洞库，美国国家信息安全漏洞库 NVD，赛门铁克漏洞库等等）官网：<https://nvd.nist.gov/>

NVD 的更新频率是出现问题实时更新，具体链接：<https://nvd.nist.gov/general/nvd-dashboard>

### 具体使用步骤

#### 1. 下载 Command Line 并解压到本地
下载地址：<https://owasp.org/www-project-dependency-check/>

#### 2. 申请 NVD API Key
若没有该 API Key，在下载 NVD 数据库时会很慢（即使有该 Key，大概也要一个小时）：
```
[WARN] An NVD API Key was not provided - it is highly recommended to use an NVD API key as the update can take a VERY long time without an API Key
```

申请地址：<https://nvd.nist.gov/developers/request-an-api-key>

#### 3. 配置 JDK 环境

打开 `dependency-check\bin\dependency-check.bat`，添加配置
```bash
set JAVACMD=C:\Program Files\Java\jdk-17.0.12\bin\java.exe
```

![dependency-check-config](./images/dependency-check-config.png)

#### 4. 下载 NVD 数据库
执行以下命令
```powershell
dependency-check.bat --nvdApiKey=your_api_key_here --updateonly
```

![nvd-download](./images/nvd-download.png)

#### 5. 扫描打包应用并生成报告

OWASP Dependency-Check 并不依赖某种语言的构建系统，它可以直接扫描
- .dll
- .exe
- .nupkg
- .deps.json
- 目录（包含以上文件）

##### Java
```bash
dependency-check.bat -n --failOnCVSS 7 --project "unisystem" --scan "D:\\jars\app.jar" -o "D:\\reports"
```

- -n：是不更新扫描
- --failBuildOnCVSS：表示失败的告警阈值，不影响扫描结果。通过设置合理的阈值，开发者可以确保他们的项目不会因为包含高风险漏洞的依赖而构建成功，进而避免潜在的安全风险
- --project：用来指定扫描报告中显示的“项目名称”，不会影响扫描逻辑本身（会出现在生成的 HTML、JSON 等报告中，用作报告标题）

##### .NET
```bash
dependency-check.bat -n --project "dll-scan" --scan "D:\dotnet-app\MyLib.dll" -o "D:\reports"
```

.NET 还可以扫描 .exe、.deps.json（推荐） 甚至是包含以上文件的目录

![dependency-check-dotnet](./images/dependency-check-dotnet.png)

#### 6. 查看报告

![dependency-check-result](./images/dependency-check-result.png)

- dependency-check version（工具版本）：12.1.9
- Report Generated On（报告生成时间）：Wed, 17 Dec 2025 00:59:22 +0800
- Dependencies Scanned（依赖项扫描数）：6 (4 unique)
- Vulnerable Dependencies（漏洞依赖项数）：0
- Vulnerabilities Found（总计漏洞数）：0
- Vulnerabilities Suppressed（抑制的漏洞数）：0（抑制的漏洞指的是在扫描过程中，被工具识别并且被用户或配置文件明确忽略的漏洞数量。这些被“抑制”的漏洞不会出现在最终的报告中）

| Dependency | Vulnerability IDs | Package | Highest Severity | CVE Count | Confidence | Evidence Count |
|------|------|------|------|------|------|------|
| 代表存在漏洞的依赖项 | 代表对应NVD的漏洞ID | 代表依赖中的包 | 代表漏洞的严重程度 | 代表与依赖包与漏洞关联的数量 | 代表工具对漏洞检测结果的置信度 | 代表支持该漏洞检测结果的证据数量|

##### CVSS 概述
NVD 评级依赖 CVSS（CommonVulnerability Scoring System），即“通用漏洞评分系统”，是一个“行业公开标准，其被设计用来评测漏洞的严重程度，并帮助确定所需反应的紧急度和重要度，具体评分标准如下：

1. **CRITICAL**（9.0 - 10.0）：这些漏洞通常具有最高的严重性，可能导致远程代码执行、大规模数据泄露、系统完全被攻陷等影响。应立即修复或采取其他补救措施。
2. **HIGH**（7.0 - 8.9）：这些漏洞可能会导致重大安全问题，如数据泄露、拒绝服务攻击等。应尽快修复。
3. **MEDIUM**（4.0 - 6.9）：这些漏洞可能会导致中等程度的安全问题，但利用难度较大或者影响范围有限。虽然不需要紧急处理，但应在合理的时间内修复。
4. **LOW**（0.1 - 3.9）：这些漏洞通常不会造成重大影响。可以在日常维护过程中进行修复。
5. **NONE**（0）：无问题。

##### Dependency-Check 报告漏洞划分标准
Dependency-check 对漏洞严重程度判定有四档分别为：

1. **CRITICAL**：这些漏洞通常会导致非常严重的后果，如远程代码执行、大规模数据泄露等。应优先处理。
2. **HIGH**：高危漏洞可能导致严重的后果，但不一定能直接被利用。也应尽快修复。
3. **MEDIUM**：中等风险漏洞可能会导致较大的安全问题，但利用难度较大或影响范围有限。
4. **LOW**：低风险漏洞一般不会造成重大影响，但在有余力的情况下也应考虑修复。

---

### 参考文档

- [安全组件扫描工具 owasp/dependency-check 使用](https://www.cnblogs.com/allay/p/18735876)