# OWASP ZAP 實作教學

## 概述

**OWASP ZAP** 是 OWASP 旗下最廣泛使用的 **動態應用安全測試（DAST）工具**，開源、免費、跨平台，適用於開發者、安全工程師、QA 等不同角色。它透過作為中間代理（Proxy）攔截瀏覽器與 Web 應用之間的流量，來實現漏洞檢測。

## 核心功能

- 攔截代理（Intercepting Proxy）：可查看、修改請求/回應。
- 自動化掃描（Active / Passive Scan）：
    - Passive Scan：不修改流量，僅分析弱點，安全但覆蓋有限。
    - Active Scan：主動發送 payload，能發現更多漏洞，但具有入侵性。
- Spider & AJAX Spider：爬取網站結構，包括 SPA 應用。
- Fuzzer：發送異常參數測試應用健壯性。
- Forced Browsing：自動探測隱藏路徑。
- WebSockets 支援、腳本擴充、多語言外掛體系（Marketplace）。

## 下载安装

> 下載地址：<https://www.zaproxy.org/download/>

下載到本機後雙擊 `ZAP_2_17_0_windows.exe` 執行安裝（以 Windows 系統為例）。安裝完成後開啟該應用

![zap-portal](../images/zap-portal.png)

## 實作步驟

### Exercise 1 自動掃描（Automated Scan）

點選 “Automated Scan” 進入自動掃描設定畫面，只需要設定要測試的網址，然後按 “Attack” 執行傻瓜式的滲透測試。（自動掃描無需設定代理）

![zap-auto-scan](../images/zap-auto-scan.png)

自動掃描結束後可點選 “Alerts”（警報）查看漏洞資訊。

![zap-auto-scan-result](../images/zap-auto-scan-result.png)

### Exercise 2 審查與分析掃描結果

步驟：

1. 掃描完成後，導覽到 OWASP ZAP 中的 “Alerts” 選項卡。
2. 檢視依嚴重性分類的已識別漏洞清單。
3. 點選每個漏洞查看詳細資訊，包括描述、風險等級與建議修復步驟。
4. 前往 “Report” > “Generate Report...” 選擇所需格式（例如 HTML、XML）來產生掃描報告。

查看 Alerts
![zap-alerts](../images/zap-alerts.png)

產生報告
![zap-generate-report](../images/zap-generate-report.png)

檢視產生的報告
![zap-report-overview](../images/zap-report-overview.png)

### Exercise 3 手動掃描（Manual Explore）

由操作者執行頁面瀏覽，ZAP 不會主動發送惡意請求，只分析已有流量。

點選 Manual Explore（手動掃描）進入手動掃描設定畫面，輸入要測試的網址，點選啟動瀏覽器。之後會啟動指定的瀏覽器並開啟對應頁面，操作者可在瀏覽器中繼續操作，ZAP 會將訪問行為記錄並分析。

![zap-manual-scan](../images/zap-manual-scan.png)

同時可結合 ZAP 的其他功能，右鍵特定站點 -> Attack -> 選擇 “Forced Browse Site”、“Forced Browse Directory”、“Forced Browse Directory(and Children)” -> 選擇預設的 directory-list-1.0.txt 目錄字典進行嘗試爬取。

![zap-force-browse](../images/zap-tools.png)

#### 附：透過設定代理執行手動掃描

若不透過 ZAP 啟動瀏覽器，亦可透過設定系統代理方式，以 Chrome 為例，打開其代理設定

![chrome-proxy](../images/chrome-proxy.png)

之後會跳轉到系統的代理設定，依下圖進行設定並儲存（localhost:8080 為 ZAP 預設代理）

![system-proxy](../images/system-proxy.png)

此時，在 Chrome（及系統其他應用）瀏覽網頁的交通會被 ZAP 捕獲並記錄分析（以偵測漏洞）。

> 注意：分析完成後請記得將系統代理還原關閉。

### Exercise 4 主動掃描（Active Scan）

步驟：

- 在 OWASP ZAP 介面中，於 “Sites”（站點）選項卡下右鍵點選目標網站。
- 選擇 “Attack”，然後選擇 “Active Scan”。
- 設定掃描選項並開始掃描。
- 在 “Active Scan” 選項卡中監控主動掃描進度。

![zap-active-scan](../images/zap-active-scan.png)

### 後續

- 攔截請求：在代理模式中，可以將瀏覽器訪問的請求先放入 break。修改參數後按右向箭頭放行，會將修改後的請求送回伺服器。
- 重放攻擊
- 暴力破解
