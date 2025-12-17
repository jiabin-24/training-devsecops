# VSCode DevSecOps 課程文件

## 課程介紹

本課程圍繞 DevSecOps 理念，結合 VS/VSCode 工具鏈，介紹靜態程式碼分析工具、相依項目安全掃描、機密外洩防護等技術，並分享如何在開發流程中落實安全性，幫助開發者在開發流程中整合安全實務，實現開發、維運、安全一體化。

## 工具環境

- [Visual Studio](https://visualstudio.microsoft.com/downloads/)
    - 推薦 2026/2022 Community 版本
    - Community/Professional/Enterprise 擇一即可（勾選【.NET 桌面開發】和【ASP.NET 與網頁開發】工作負載，並在各個元件中啟用 Roslyn 分析器；課程示範靜態分析和整合 SonarLint 擴充需要此環境）

- [Microsoft .NET SDK 8.0](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
    - .NET 程式執行環境
    - 提供範例專案編譯與 SonarScanner for .NET 所需的 CLI（命令列工具）

- [Visual Studio Code](https://code.visualstudio.com/)
    - 輕量級 IDE（可用於 .NET、Java、JavaScript）
    - 安裝 SonarLint 外掛，以在輕量編輯器中即時檢查程式碼品質與安全性

- [Java SE Development Kit 17.0.12](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
    - 提供 Java 執行環境，用於本地執行 SonarQube
    - 可以先下載 zip 套件，課程中指導安裝與設定

- [SonarQube Community Edition](https://www.sonarsource.com/products/sonarqube/downloads/) 和 [SonarScanner for .NET](https://docs.sonarsource.com/sonarqube-server/9.8/analyzing-source-code/scanners/sonarscanner-for-dotnet/)
    - 下載即可，課程中會指導安裝。（本地 SAST 伺服器建議透過 Docker 或 zip 套件啟動）以及將 .NET 專案分析結果上傳到 SonarQube。

- [OWASP Dependency-Check CLI](https://owasp.org/www-project-dependency-check/)
    - 下載安裝即可，課程中指導安裝。（SCA 掃描工具，識別第三方組件中的已知漏洞）

- [OWASP ZAP](https://www.zaproxy.org/download/)
    - 下載安裝即可，課程中指導安裝。（DAST 與滲透測試示範工具，執行基本攻擊模擬）

- [Git for Windows](https://gitforwindows.org/) 以及 [gitleaks](https://github.com/gitleaks/gitleaks)
    - 需要一個 GitHub 帳號（用於執行 GitHub Action/管線）
    - 下載即可，課程中指導安裝。（版本控制與機密外洩掃描工具組，用於教學情境中的憑證檢測）

## 課程大綱

1. DevSecOps 基礎理念與安全開發流程
2. VS/VSCode 環境設定及主流安全外掛推薦（如 SonarLint、gitleaks 等）
3. 靜態應用安全測試（SAST）原理與工具實作（SonarQube、Roslyn 分析器）
4. 相依項目安全掃描與第三方組件漏洞檢測（OWASP Dependency-Check）
5. 動態應用安全測試（DAST）與滲透測試示範（OWASP ZAP）
6. 機密外洩防護與憑證檢測流程（gitleaks 等工具）
7. 自動化安全檢測與持續整合實作（安全管線構建）
8. 程式碼提交與安全檢測的最佳實務

## Day 1 課程計畫

**主題：DevSecOps 理念與安全開發環境實作**

- 程式碼安全與 DevSecOps 基礎理念
- VS/VSCode 安全外掛及 lint 工具實作
- 整合 SonarQube 等靜態程式碼分析工具，提升 .NET 專案程式碼品質與安全性
- 機密外洩防護：憑證檢測與安全提交流程規範
- 靜態應用安全測試（SAST）原理與實戰演練

- 實作：提交程式碼並進行安全掃描

## Day 2 課程計畫

**主題：自動化安全檢測與持續整合實作**

- 軟體組成分析（SCA）：識別不安全的第三方套件
- 使用 OWASP Dependency-Check 工具進行相依性掃描
- 滲透測試與動態應用安全測試（DAST）概念
- 使用 OWASP ZAP 工具進行簡易滲透測試
- 案例分享：如何在 VS 及 VS Code 專案中導入安全檢測流程

- 安全事件回應與日誌稽核
- 實作：構建安全管線並自動化漏洞檢測

## 課程重點

- 如何在 VS/VSCode 中整合安全工具
- 自動化安全掃描流程示範
- 程式碼提交與安全檢測的最佳實務
- DevSecOps 流程中的常見問題與解決方案

## 綜合效益

- 提升開發團隊安全意識
- 降低安全風險與漏洞發生率
- 加快安全合規流程
- 實現安全左移，減少後期修復成本

## 附：初始計畫

![origin-agenda](./images/origin-agenda.png)
