# Topic 7 知識概念整理：惡意程式、攻擊者類型、詐欺手法與 Web Application 風險

Topic 7 題數不多，但都是高辨識度考點。核心是惡意程式分類、攻擊者能力、資料操弄詐欺、偽裝攻擊，以及 Web application 常見風險。

這章最重要的是把名稱和特徵對上：Trojan horse、worm、polymorphic virus、script kiddie、data diddling、salami technique、phishing/social engineering、architecture-specific attack、web application attacks。

## 1. Trojan Horse

Trojan horse 是偽裝成正常或有用程式的惡意程式。使用者以為自己在執行合法軟體，實際上它在背後執行未授權動作。

特徵：

- 需要使用者或系統執行它。
- 不一定會自我複製。
- 常偽裝成工具、遊戲、破解軟體、附件或更新程式。
- 可安裝後門、竊取密碼、下載其他 malware。

題庫看到「appears useful but contains malicious code」通常是 Trojan horse。

## 2. Virus、Worm 與 Malware 差異

Virus：

- 需要附著在 host file、boot sector、macro 或其他載體。
- 通常需要執行受感染檔案才會擴散。

Worm：

- 可自行複製與傳播。
- 常透過網路漏洞、弱密碼或服務暴露擴散。
- 不一定需要附著在其他程式。

Trojan：

- 偽裝成合法程式。
- 主要靠欺騙使用者執行。
- 不以自我複製為主要特徵。

題庫看到「self-replicating over network」通常選 worm。

## 3. Polymorphic Virus

Polymorphic virus 會改變自己的外觀或加密形式，讓 signature-based antivirus 更難偵測。

特徵：

- 每次感染時改變 decryptor 或 code pattern。
- 惡意邏輯可能相同，但 binary signature 不同。
- 目標是逃避以固定特徵碼為主的偵測。

題庫看到「changes its signature」或「mutates to avoid detection」通常是 polymorphic。

常見延伸：

- Oligomorphic：有限數量變形。
- Polymorphic：大量變形，通常改變 decryptor。
- Metamorphic：重寫自身程式碼結構，變形更徹底。

## 4. 病毒偵測的限制

題庫會問「是否可能保證偵測所有病毒？」答案通常是 Not possible。

原因：

- 新型 malware 可能沒有 signature。
- Polymorphic / metamorphic malware 會改變外觀。
- 加殼、加密、混淆會增加偵測難度。
- 使用者行為與系統設定也會影響防護。

防毒軟體是必要控制，但不能保證百分之百偵測所有惡意程式。

## 5. Script Kiddies

Script kiddies 是技術能力較低、主要使用他人工具或現成 exploit 的攻擊者。

特徵：

- 不一定理解工具底層原理。
- 依賴公開 exploit、掃描器、自動化工具。
- 可能造成嚴重損害，雖然能力不一定高。

題庫看到「using tools written by others」或「low skill attackers」通常是 script kiddies。

## 6. Data Diddling

Data diddling 是在資料輸入、處理前或處理中動手腳，讓系統產生錯誤結果。

特徵：

- 改的是 input data 或 transaction data。
- 可能發生在資料進入系統之前。
- 結果可能是報表、帳務、庫存或薪資錯誤。

例子：

- 修改交易金額。
- 更改輸入表單。
- 在資料進系統前調整欄位。
- 讓錯誤資料被合法程式處理。

題庫看到「altering data before or during input」通常是 data diddling。

## 7. Salami Technique

Salami technique 是每次偷一點點，單次金額小到不容易被發現，但累積後可形成可觀收益。

特徵：

- 小額、重複、長期。
- 常出現在金融、薪資、利息、交易 rounding。
- 利用四捨五入差額或微小扣款。

例子：

- 每筆交易偷 0.01 元。
- 把利息尾數轉到攻擊者帳戶。
- 從大量帳戶各扣一小筆錢。

題庫看到「small amounts from many transactions」通常是 salami technique。

## 8. Masquerading、Phishing 與 Social Engineering

Masquerading attack 是攻擊者假扮成其他人或合法系統。

常見形式：

- 使用偷來的帳號登入。
- 假冒管理員或服務。
- 偽造 email、網站或登入頁。

Phishing：

- 透過假網站、假郵件或訊息誘騙使用者交出憑證或敏感資料。
- 屬於 social engineering。

Spoofing：

- 偽造來源或身份，例如 email spoofing、IP spoofing、caller ID spoofing。

題庫若說兩個攻擊都是「偽裝成可信來源」，通常可歸為 masquerading attacks。

## 9. 攻擊者動機

題庫常見的主要動機之一是 money / financial gain。尤其是詐欺、資料竊取、勒索、交易操弄與 salami technique。

常見動機：

- financial gain。
- revenge。
- curiosity。
- ideology。
- fame。
- espionage。
- sabotage。

考題若問 computer crime 中常見或最直接動機，financial gain 經常是答案。

## 10. Architecture-Specific Attacks

Architecture-specific attack 是針對特定平台、作業系統、CPU 架構、應用框架或網路架構的攻擊。

特徵：

- 依賴特定環境。
- 不一定能跨平台使用。
- exploit 可能只對某版本 OS、某硬體、某協定實作有效。

例子：

- 只影響特定 Windows 版本的漏洞。
- 只針對某 CPU instruction 或 memory layout。
- 只對某 web framework 的 routing/parser 有效。

題庫看到「specific to hardware/software architecture」通常是 architecture specific。

## 11. Web Application Attacks

Topic 7 題庫最後把風險指向 Web Applications。Web app 是常見攻擊面，因為它通常暴露在 Internet，且直接處理使用者輸入。

常見 web application 風險：

- Injection，例如 SQL injection、command injection。
- Cross-site scripting, XSS。
- Cross-site request forgery, CSRF。
- Broken authentication。
- Broken access control。
- Session hijacking。
- Insecure file upload。
- Sensitive data exposure。
- Security misconfiguration。

Web app 防護重點：

- input validation。
- output encoding。
- parameterized queries。
- strong authentication。
- session protection。
- least privilege。
- secure error handling。
- patching frameworks and libraries。
- logging and monitoring。

題庫若問攻擊者最常利用哪類外部暴露系統，Web Applications 常是高風險答案。

## 12. 題庫速記表

| 概念 | 重點 |
| --- | --- |
| Trojan horse | 偽裝成合法程式，內含惡意功能 |
| Worm | 可自我複製並透過網路傳播 |
| Virus | 附著 host，需要執行或載體 |
| Polymorphic virus | 改變 signature 逃避偵測 |
| Detect all viruses | 不可能保證 |
| Script kiddies | 使用他人工具、技術能力較低 |
| Data diddling | 修改輸入或處理前資料 |
| Salami technique | 從大量交易偷小額 |
| Masquerading | 偽裝成合法使用者或系統 |
| Financial gain | 常見攻擊動機 |
| Architecture specific | 依賴特定平台或架構 |
| Web applications | 常見外部攻擊面 |

## 13. 容易選錯的點

- Trojan horse 不一定自我複製；worm 才是自我傳播的典型。
- Polymorphic 的重點是改變外觀或 signature，不是單純加密資料。
- 防毒不可能保證抓到所有病毒。
- Script kiddie 技術低不代表沒有破壞力。
- Data diddling 是改資料，不是偷硬體或竊聽網路。
- Salami technique 靠小額多次累積，不是一次大額詐欺。
- Phishing、spoofing、使用偷來帳號都可以落在 masquerading/social engineering 的脈絡中。
- Web application 風險核心通常是處理不可信輸入與身份/授權控制失敗。

