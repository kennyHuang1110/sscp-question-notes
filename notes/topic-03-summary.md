# Topic 3 知識概念整理：稽核、IDS、事件處理與偵測

Topic 3 的主軸是「如何知道事情發生了、如何追蹤責任、如何偵測入侵，以及如何在事件後保留證據」。它和 Topic 1 的 accountability 有重疊，但更偏偵測、稽核與事件回應。

## 1. Accountability 與 Audit

Accountability 是讓系統行為可以追溯到特定主體。它依靠三個基礎：

- Identification and authentication：知道使用者是誰，並驗證身分。
- Access control：限制使用者能做什麼。
- Audit mechanisms / audit trails：留下可追蹤紀錄。

系統要有 accountability，不能只靠「大家都有帳號」。如果沒有稽核紀錄，就很難證明誰在何時做了什麼。

Audit trail 通常包含使用者、時間、事件、資源、成功或失敗狀態。它可用來支援偵測、調查、責任歸屬與合規。

Timely review of system access audit records 是 detective control。它不是防止事件發生，而是讓組織及早發現異常。

Clipping level 是在產生違規紀錄或警示前，可以容忍的違規次數。它用來降低雜訊，但設定太高可能錯過攻擊。

## 2. Auditor、Data Owner 與管理責任

Systems auditor 或 information systems auditor 的角色是衡量資訊系統安全控制是否有效。他們應保持客觀性，避免自己設計、自己稽核造成利益衝突。

Self-audit 的問題不是一定錯，而是 objectivity 較弱。Independent audit 較能提供可信的外部觀點。

Data / Information Owner 最適合決定資料保護需求，因為 owner 了解資料的價值、敏感度、業務用途與風險。

在 OLTP 等交易系統中，偵測到 invalid transactions 時，常見概念是把錯誤交易寫入報表並交由人員 review，而不是直接丟棄或自行修正。這是為了保留可追蹤性與避免自動修正造成更大錯誤。

Due care 是「合理謹慎地做該做的事」，可理解為 good faith、prudent person、best interest。它不等於追求 profit。

Due diligence 是持續調查、追蹤與確認風險處理是否有效。例如建立並執行 patch management process，就比較接近 due diligence。

## 3. IDS 的基本概念

IDS Intrusion Detection System 用來監看網路流量或主機稽核紀錄，判斷是否有違反安全政策或攻擊行為。它主要是 detective control。

IDS 的兩大部署型態：

- NIDS Network-based IDS：監看網路區段流量。
- HIDS Host-based IDS：安裝在重要主機上，監看本機事件。

NIDS 觀察封包 header、payload 與流量模式，因此適合偵測網路層面的攻擊、掃描、DoS 或異常服務。它通常放在 discrete network segment 上。

HIDS 觀察主機內部活動，例如 system logs、event logs、processes、resources、檔案變更。它適合判斷攻擊是否成功，以及是否有未授權變更。

HIDS 的優點是能看到主機內部細節；缺點是可能消耗主機資源，且對 host OS 較侵入。

Host-based intrusion detection 很依賴 audit trails，因為主機事件紀錄是它判斷異常的主要資料來源。

IDS alarm / IDS architecture 的三大元件：

| 元件 | 功能 | 考題關鍵字 |
| --- | --- | --- |
| Sensor | 偵測事件、收集網路或主機資料 | detect、monitor、collect events |
| Communication | 把事件或警示從 sensor 傳到管理端 | transmit、send alert、report event |
| Enunciator | 呈現警示，讓管理員知道有事件 | announce、display alarm、notify administrator |

Response 是事件後的處置流程，不是 IDS alarm 的基本組件本身。

## 4. Signature-based 與 Anomaly-based IDS

Signature-based detection 又稱 knowledge-based detection。它把系統或網路活動拿來比對已知攻擊特徵。

優點：

- 對已知攻擊準確。
- 警示較容易解釋。
- 誤報通常較低。

缺點：

- 只能有效偵測已知 signature。
- signature database 必須持續更新。
- 新型攻擊、變形攻擊可能漏掉。

Anomaly-based detection 又稱 behavior-based 或 statistical anomaly-based detection。它先建立 normal baseline，再偵測偏離正常行為的事件。

優點：

- 可能發現未知攻擊。
- 適合偵測異常流量、新服務突然出現、行為模式突變。

缺點：

- False positive 較高。
- 正常業務變化也可能被視為異常。
- baseline 品質會直接影響偵測效果。

Traffic anomaly-based IDS 特別適合「不知道攻擊長什麼樣，但要找 unusual traffic behavior」的情境。

常見 IDS 類型與考點整理：

| 類型 | 主要看什麼 | 適合偵測 | 優點 | 缺點 / 常考陷阱 |
| --- | --- | --- | --- | --- |
| NIDS Network-based IDS | 網路區段流量、packet header、payload、traffic pattern | 掃描、DoS、網路層攻擊、異常服務出現 | 不必安裝在每台主機，可監看整個 network segment | 加密流量看不清 payload；通常不知道攻擊是否已在主機成功 |
| HIDS Host-based IDS | system logs、event logs、processes、resources、檔案變更 | 未授權主機變更、登入異常、攻擊是否成功 | 看得到主機內部細節，適合搭配 audit trails | 會消耗主機資源，對 host OS 較 invasive |
| Signature-based / Knowledge-based IDS | 已知攻擊特徵、attack database、known vulnerabilities | 已知攻擊、已知 exploit pattern | 準確、容易解釋、false positive 通常較低 | 偵測未知攻擊能力弱；signature database 要持續更新 |
| Anomaly-based / Behavior-based IDS | normal baseline 與目前行為差異 | 未知攻擊、異常流量、行為模式突變 | 有機會抓到 zero-day 或新型攻擊 | false positive 較高；正常業務變化也可能被警示 |
| Statistical anomaly-based IDS | 統計模型、使用量、頻率、時間、流量基準線 | 偏離平常統計輪廓的事件 | 能發現不符合平常模式的活動 | baseline 不準會造成大量誤報 |
| Traffic anomaly-based IDS | unusual traffic behavior、新服務、新連線模式 | 不知道攻擊特徵但流量明顯異常的情境 | 題目出現 unknown attacks、unusual traffic behavior 常選它 | 仍可能誤判正常流量尖峰或新業務流量 |
| Protocol anomaly-based IDS | 協定格式、狀態與 RFC/協定規則偏離 | 違反協定規範的封包或連線行為 | 可抓不正常協定使用方式 | 如果題目重點是「新服務出現 / unusual traffic」，通常不是首選 |
| Application-based IDS | 應用程式交易、應用 log、使用者操作 | 應用層濫用、交易異常、特定 app 攻擊 | 看得到 NIDS 看不懂的應用語意 | 範圍較窄，通常只保護特定應用 |

題目判斷口訣：

- 問 network traffic、packet header/payload、discrete network segment：選 NIDS。
- 問 system logs、event logs、processes、resources、是否攻擊成功：選 HIDS。
- 問 known attacks、attack database、predefined pattern：選 signature-based / knowledge-based。
- 問 normal profile、baseline、unknown attack、behavior：選 anomaly-based / behavior-based。
- 問 unusual traffic behavior、new service appearing on network：選 traffic anomaly-based。
- 問 false positive 多：通常是 anomaly-based / statistical anomaly-based。

## 5. Incident Reporting 與事件處理

Incident reporting 的重點是讓事件被集中、及時、完整地通報與追蹤。它通常包含：

- 誰發現事件。
- 事件發生時間與影響範圍。
- 初步症狀與相關系統。
- 已採取的處置。
- 後續追蹤與結案紀錄。

中央化通報可以避免事件散落在個人信箱或口頭訊息中，提升可追蹤性與管理可見度。

Incident tracking 與 audit trail control assessment 相關，但它不是用來直接評估 identification/authentication 的主要控制。

Business Continuity Plan 應定期 review，至少每年一次，並在重大業務或系統變更後重新檢視。

## 6. Evidence Collection 與 Forensics

數位證據處理的核心是不要改變原始證據，並能證明證據完整性。

常見做法：

- 使用 write blocker，避免寫入原始磁碟。
- 製作 full-disk image，而不是直接在原機上分析。
- 對映像檔與重要 log 建立 message digest / hash。
- 維持 chain of custody，記錄誰在何時接觸證據。

在遭攻擊的系統上直接「打開資料夾看內容」可能會改變存取時間、暫存資料或系統狀態，因此可能破壞證據收集流程。

Forensics 的重點不是快速找答案，而是讓調查結果可被信任、可重現、可在管理或法律程序中使用。

## 7. Honeypot 與安全工具

Honeypot 是刻意設置的誘捕系統，用來觀察攻擊者行為、收集攻擊技術或讓攻擊者離開真正資產。它不應存放真實敏感資料，也不應成為攻擊其他系統的跳板。

常見安全或攻擊測試工具：

- Nessus、SAINT：漏洞掃描。
- Nmap：網路掃描與服務探測。
- L0phtCrack、OphCrack、John the Ripper：密碼破解或稽核。
- Tripwire：檔案完整性監控，不是 hacker tool 的典型代表。

工具本身不是善惡的分界，使用目的、授權範圍與控制流程才是關鍵。

## 8. Ethical Hacking 與 Penetration Testing

Ethical hacking 是在授權範圍內模擬攻擊，找出實際可被利用的弱點。它必須有明確的 scope、時間、方法限制、通報窗口與停止條件。

聘用 penetration testing firm 時，應看：

- 技術能力與方法論。
- 授權與保密條款。
- 是否能控制測試對營運的影響。
- 報告品質與修補建議。

「使用很厲害的前駭客」不是必要條件。更重要的是專業、可控、合法、可審計。

Ethical hacker 不代表永遠不能使用可能影響服務的工具，而是必須在授權範圍內控制風險，避免造成未被允許的破壞。

## 9. OSI 與網路概念

Network layer 常見協定包含 IP、OSPF、RIP。HTTP 是 application layer，不是 network layer。

Session layer 負責建立、管理與結束主機間的邏輯連線。題目中常把 full duplex 放在 session 概念附近理解：雙方可同時雙向通訊。

這些 OSI 概念的重點不是死背層級，而是理解安全控制或攻擊發生在哪一層。例如 NIDS 可能檢查網路封包，而應用層攻擊可能需要應用層日誌或 WAF 才看得清楚。

## 10. Operational Controls 與日常管理

Preventive operational controls 常見例子：

- 保護 laptops、PCs、workstations。
- 控制 media access and disposal。
- 職責分離與作業流程控制。
- 人員安全相關流程。

Security awareness and technical training 很重要，但通常歸在 administrative / awareness 類型，而不是典型 preventive operational control。

Software license violations 常透過檢查個人電腦或軟體資產清冊發現。這屬於資產管理、合規與營運控制的一部分。

Account review 可以發現離職帳號、閒置帳號、權限過大或不再需要的存取權，但它不直接評估 user-chosen password strength。

## 11. 概念對照速查

| 概念 | 核心意思 |
| --- | --- |
| Accountability | I&A + access control + audit trail |
| Audit trail | 把行為連回人、時間與資源 |
| Audit review | Detective control |
| Systems auditor | 衡量控制有效性 |
| Data owner | 決定資料保護需求 |
| Due care | 合理謹慎、善意行事 |
| Due diligence | 持續查核與追蹤風險處理 |
| IDS | 偵測違反安全政策的活動 |
| Sensor | IDS 偵測與收集事件的元件 |
| Communication | IDS 傳送事件或警示的元件 |
| Enunciator | IDS 呈現或通知警示的元件 |
| NIDS | 監看網路流量 |
| HIDS | 監看主機 log、process、resources |
| Signature-based / Knowledge-based | 比對已知攻擊特徵、attack database |
| Anomaly-based / Behavior-based | 比對 normal baseline 的偏離 |
| Statistical anomaly-based | 用統計基準線判斷異常，false positive 較高 |
| Traffic anomaly-based | 偵測 unusual traffic behavior、新服務出現 |
| Protocol anomaly-based | 偵測違反協定規則的封包或連線 |
| Application-based IDS | 監看特定應用 log、交易與使用者操作 |
| False positive | 把正常事件誤判成攻擊 |
| Write blocker | 避免修改原始證據 |
| Full-disk image | 對副本分析，保護原始媒體 |
| Hash / message digest | 證明證據完整性 |
| Honeypot | 誘捕與觀察攻擊者 |
| Tripwire | 檔案完整性監控 |
| HTTP | Application layer |
| IP / OSPF / RIP | Network layer |
