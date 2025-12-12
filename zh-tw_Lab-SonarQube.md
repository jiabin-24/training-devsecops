# SonarQube 集成到開發管道（GitHub）

## 引言
在現代軟體開發中，程式碼品質至關重要。隨著開發團隊的規模和複雜度增加，使用工具來監測和提升程式碼品質變得愈發重要。SonarQube 作為一個流行的程式碼審查工具，與 GitHub 的整合能夠幫助開發者即時發現並修復程式碼中的問題。

## SonarQube 簡介

### 什麼是 SonarQube？
SonarQube 是一個開源平台，主要用於程式碼品質管理和持續檢測。它可以分析程式碼並提供有關程式碼品質的詳細報告，包括程式碼的複雜度、重複率、安全漏洞等。

### SonarQube 的主要特性

- **支持多種程式語言**：SonarQube 支援多達 27 種程式語言，包括 Java、C#、JavaScript 等。
- **可視化報告**：提供直觀的程式碼品質指標，使開發者能夠快速了解專案狀況。
- **整合 CI/CD 工具**：能夠與多個持續整合工具（如 Jenkins、Travis CI 等）無縫整合。
- **自動檢測安全漏洞**：可以幫助團隊及時發現安全隱患。

## GitHub 簡介

### 什麼是 GitHub？
GitHub 是一個基於雲端的程式碼託管平台，使用 Git 作為版本控制系統。它允許開發者協作、管理專案及追蹤程式碼的變更。

### GitHub 的主要特性

- **版本控制**：便於團隊管理程式碼變更和版本歷史。
- **協作**：支援多人協作開發，提供 Pull Requests、Issues 等功能。
- **社群支援**：擁有豐富的開源專案和社群支援。

## SonarQube 與 GitHub 的整合

### 為什麼整合 SonarQube 與 GitHub？
透過將 SonarQube 整合到 GitHub 中，開發團隊可以在每次程式碼提交時自動進行程式碼品質檢查，從而在開發早期發現問題。

***

## 本機安裝 SonarQube（Server）
相較於 SonarSource 官方提供的線上託管服務（SonarQube Cloud，原 SonarCloud），SonarQube 也提供本機安裝版本（可部署於個人/企業、本機/內網環境）。

> [SonarQube Server download](https://www.sonarsource.com/products/sonarqube/downloads/)，下載最新版本

![sonarqube-download](./images/sonarqube-download.png)

### 1、本機安裝 Java JDK 17（用於執行 SonarQube Server）

- 下載位址：<https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html>

將 zip 套件下載到本機之後，解壓縮放置於例如 `C:\Program Files\Java`

![jdk-location](./images/jdk-location.png)

打開系統設定 -> 環境變數

![system-variable](./images/system-variable.png)

在環境變數中新增 `JAVA_HOME` 並設定值為上一步解壓後的資料夾路徑（此處為 `C:\Program Files\Java\jdk-17.0.12`）

![java-home](./images/java-home.png)

編輯 `Path` 環境變數，加入 `%JAVA_HOME%\bin`

![java-bin](./images/java-bin.png)

此時，打開 PowerShell/CMD，輸入 `java -version`，應能看到已安裝並設定的 Java 版本

![ps-java-version](./images/ps-java-version.png)

### 2、設定 SonarQube Server

將 SonarQube Community 的安裝包（zip）下載並解壓到本機後（例如：`D:\software\sonarqube-25.11.0.114957`），打開其設定檔 `conf/sonar.properties`（此處為 `D:\software\sonarqube-25.11.0.114957\conf\sonar.properties`），該檔定義了 SonarQube 的多項屬性（例如資料庫連線資訊）。

我們將 `sonar.embeddedDatabase.port=9092` 取消註解，表示使用內建的 H2 資料庫（無需另行安裝資料庫管理工具，即可使用）。如果你本機之前已安裝 MySQL/SQL Server，也可以取消相應的設定，以讓 SonarQube 連接到這些資料庫（注意：不建議在生產環境使用內建的 H2 資料庫，H2 僅用於個人測試、示範使用）。

![sonarqube-conf](./images/sonar-config.png)

### 3、啟動 SonarQube Server

定位到 SonarQube Server 的 `bin\windows-x86-64` 目錄（若為其他作業系統請定位到相應目錄），在該資料夾開啟命令列工具（例如：PowerShell）

![start-sonar](./images/start-sonar.png)

執行 `./StartSonar.bat`，以啟動 SonarQube Server。啟動成功截圖如下

![start-sonar-success](./images/start-sonar-success.png)

注意：若啟動失敗，可查看 `\logs` 資料夾下的日誌檔以定位失敗原因（例如：`\logs\web.log`）

### 4、瀏覽 SonarQube 頁面

打開瀏覽器，前往 <http://localhost:9000>，輸入使用者名稱/密碼（初始帳號與密碼均為 admin，登入後請重置密碼），登入後查看 SonarQube 的主要功能

- 品質閘（Quality Gate）自動判定
- 程式碼安全檢測（Security + Hotspots）
- 可靠性檢查（Reliability）
- 可維護性檢查（Maintainability / Code Smells）
- 程式碼重複率檢測（Duplications）
- 測試覆蓋率統計（Coverage）
- 新增程式碼與全量程式碼的品質比較（New Code / Overall Code）
- 可視化分析儀表板（Overview、Measures、Activity）
- 問題管理（Issues 管理、規則、指派）

![sonar-home](./images/sonar-home-page.png)

***

## 使用 SonarQube 掃描本機專案

### 1、建立本機專案

如圖，先建立一個本機專案（讀取本機程式碼）

![create-local-project](./images/create-sonar-project.png)

填寫專案名稱（key、branch 使用自動生成的即可），然後下一步

![create-local-project-detail](./images/create-sonar-project-2.png)

選擇預設的設定即可（當然也可以選擇自訂的規則），點選【Create project】

![create-local-project-strategy](./images/create-sonar-project-3.png)

### 2、設定專案

建立專案完成後，會彈出 `Analysis Method` 頁面，選擇 `Locally`

![bind-project](./images/bind-project.png)

產生專案權杖（使用預設填入的資訊即可），直接點選【Generate】（注意：產生權杖後請即時將該權杖儲存到本機，後續無法再於 SonarQube 頁面中檢索該權杖）

![create-token](./images/create-token.png)

依照你的程式碼情況，選擇合適的分析步驟（此處以 .NET 為例；若為其他框架例如 Java，可選擇相應的 Tab - Maven），根據提示執行相應命令

![analysis-code](./images/analysis-code.png)

### 3、檢視專案程式碼檢查結果

依上述步驟對專案程式碼執行分析並上傳到 SonarQube Server 後，就可以在頁面中檢視程式碼的各項指標。也可以進入具體的維度分頁查看清單與詳細內容

![project-overview](./images/project-overview.png)

***

## 將 SonarQube 整合到 GitHub Action

除了在本機手動執行程式碼分析並上傳到 SonarQube Server 之外，也可以將 SonarQube 整合到 GitHub，讓開發團隊在每次程式碼提交時自動進行程式碼品質檢查，從而在開發早期發現問題。

### 1、在 GitHub 建立應用程式

> 詳細步驟連結：<https://docs.sonarsource.com/sonarqube-community-build/devops-platform-integration/github-integration/setting-up-at-global-level/setting-up-github-app>

打開 GitHub，進入個人設定 Settings

![github-setting](./images/github-setting.png)

在設定頁面底部，點選【Developer settings】，再新建一個「GitHub App」

![github-developer-setting](./images/github-develop-setting.png)

填寫以下資訊（請依自身情況調整）：
- GitHub App name：SonarQube Integration Local
- Homepage URL：http://localhost:9000
- Callback URL：http://localhost:9000
- Webhook：可取消（反選 Active）

生成 Client Secret、Private Key 並即時保存

針對 Permissions，選擇以下權限

![github-app-permission](./images/github-app-permission.png)

點選【Create GitHub App】，建立完成後再點選安裝該 App

### 2、在 SonarQube Server 設定連接 GitHub 的資訊

在 SonarQube 的頁面上建立新專案，選擇 GitHub，若是初次建立 GitHub 的專案，會出現設定頁面。之後也可在 `Administration -> Configuration -> DevOps Platform Integrations` 中再次開啟

依序填入 GitHub App 的相關資訊

- Configuration name：sonar-local-github（可自訂名稱）
- GitHub API URL：https://api.github.com
- GitHub App ID：從 GitHub App 取得
- Client ID：從 GitHub App 取得
- Client Secret：上一步生成的值
- Private Key：上一步生成的值

![sonar-github-config](./images/sonar-github-config.png)

設定完成後，即可檢索你 GitHub 帳號中的儲存庫，選擇要匯入到 SonarQube Server 的專案（注意：此時專案尚未有程式碼相關的檢查資訊，需進一步設定 GitHub Action）

![import-github-proj](./images/import-github-project.png)

### 3、將 SonarQube 整合到 GitHub Action 管道中

在建立出的專案 - Analysis Method 中，選擇【With GitHub Actions】

![sonar-github-action](./images/sonar-github-action.png)

依照彈出頁面的指示完成管道設定

- 生成儲存庫的設定檔
- 針對具體程式語言，加入對應的 action yaml 檔案

![sonar-github-step](./images/sonar-github-step.png)

### 4、將本機埠映射到 Internet

由於 SonarQube Server 安裝在本機，Internet 無法直接存取，導致 GitHub Action 在執行時無法將掃描結果上傳到本機的 SonarQube Server。此時可使用 .NET 的 DevTunnel 或者 [ngrok](https://ngrok.com/)（需註冊）將本機位址穿透到 Internet。

這裡以 ngrok 為例

```yaml
version: "3"
agent:
  authtoken: xxx
tunnels:
    githubApp:
        addr: http://localhost:9000
        proto: http
```

執行 `ngrok start --all --config ngrok.yml`，然後在 GitHub Repo 的 secret 中，將 `SONAR_HOST_URL` 設定為該域名

![ngrok-url](./images/ngrok-url.png)

### 提交程式碼，或手動觸發 GitHub 管道

將程式碼掃描結果隨每次程式碼提交上傳到 SonarQube Server

![github-action-statue](./images/github-action-statue.png)
