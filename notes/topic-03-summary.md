# Topic 3 重點整理

Topic 3 主要在考「稽核與問責」、「入侵偵測」、「事件處理與鑑識」、「滲透測試/倫理駭客」、「Due care / Due diligence」。

## 1. Accountability 與 Audit

### Accountability 問責

- 問責需要能把行為追溯到特定使用者或程序。
- 核心條件：
  - Identification and authentication
  - Audit mechanisms / audit trails
  - Access control
- 常見答案：
  - System accountability 需要 **audit mechanisms**。
  - 提供 accountability 最直接的是 **audit trails**。
  - 控制敏感資料存取的 accountability 來自 **識別、驗證、存取控制與稽核功能**。

### Audit trails 稽核軌跡

- Audit trails 可用來偵測入侵、追蹤使用者行為、分析違規。
- Host-based IDS 很依賴主機上的 audit trails / event logs。
- Timely review of system access audit records 屬於 **detection**。
- Clipping level 是設定可接受的正常錯誤/違規基準，超過門檻才記錄或分析。

### Auditor 稽核角色

- 衡量資訊系統安全控制有效性：**systems auditor**。
- 向高階管理層報告控制有效性：**information systems auditors**。
- Self-audit 與 independent audit 的主要差異：**objectivity**。

## 2. Data Owner 與管理責任

### Data / Information Owner

- 最能決定資料保護所需技術控制的人是 **Data or Information Owner**。
- 因為 owner 知道資料的價值、重要性、敏感度與業務需求。

### Due care / Due diligence

- Due care 關鍵字：good faith、prudent man、best interest。
- Due care 不直接和 **profit** 相關。
- 不違反 due diligence 的例子：依照 patch management process 安裝最新安全修補。

## 3. IDS 基礎

### IDS 是什麼

- IDS 用來即時監控網路流量或主機稽核紀錄，偵測是否違反安全政策。
- 最常見的兩種 IDS：
  - Network-based IDS, NIDS
  - Host-based IDS, HIDS

### NIDS

- 監控網路流量，通常部署在特定 network segment。
- 可檢查 packet payload 與 header。
- 較適合偵測 DoS、掃描、可疑網路流量。
- 題目問「real-time network traffic」通常選 **network-based IDS**。

### HIDS

- 安裝在重要主機上。
- 會分析 system logs、event logs、processes、resources。
- 可偵測主機上的未授權變更。
- 缺點：
  - 會消耗主機資源。
  - 可能對主機 OS 侵入性較高。
- 題目問「review system and event logs」通常選 **host-based IDS**。

### Alarm components

- IDS alarm 的基本元件包含：
  - Sensor
  - Communication
  - Enunciator
- **Response** 不是 alarm 的 fundamental component。

## 4. IDS 分析方法

### Signature-based / Knowledge-based

- Signature-based detection 會比對已知攻擊樣式。
- Knowledge-based IDS 使用已知攻擊、已知弱點與 exploit attempt 資料庫。
- 優點：對已知攻擊準確。
- 缺點：只能偵測已知 signature，對未知攻擊能力差。
- 常見同義：
  - Knowledge-based IDS = signature-based IDS
  - Behavior-based IDS = statistical anomaly-based IDS

### Anomaly-based / Behavior-based

- 先建立正常行為 baseline，再偵測偏離正常模式的活動。
- 可偵測未知攻擊或新服務突然出現。
- Traffic anomaly-based IDS 可用來發現 unusual traffic behavior。
- 缺點：false positive 較多，因為正常使用者與系統行為可能變化很大。

### Signature vs Anomaly

| 類型 | 核心概念 | 優點 | 缺點 |
| --- | --- | --- | --- |
| Signature / Knowledge-based | 比對已知攻擊模式 | 已知攻擊準確 | 無法有效偵測未知攻擊 |
| Anomaly / Behavior-based | 比對正常行為 baseline | 可發現未知異常 | false positive 多 |

## 5. Incident、Evidence 與 Forensics

### Incident reporting

- 讓通報流程集中化，比較不會阻礙員工通報事件。
- 會阻礙通報的原因：
  - 害怕被捲入麻煩
  - 害怕被誤會或指控
  - 不知道公司政策與程序

### Evidence collection

- 蒐證要避免改變原始證據。
- 正確作法：
  - 使用 write blocker
  - 做 full-disk image
  - 對 log 建立 message digest / hash
- 會破壞蒐證完整性的行為：直接顯示資料夾內容，因為可能改變存取時間或系統狀態。

## 6. Honeypot 與 Hacker Tools

### Honeypot

- 主要目的不是單純誘捕，而是：
  - 觀察攻擊是否正在發生
  - 學習攻擊技術
  - 讓網路防禦可以被強化

### Hacker tools

- 較常被駭客使用：
  - Nessus
  - SAINT
  - Nmap
  - L0phtCrack
  - OphCrack
  - John the Ripper
- 比較不像駭客工具的答案：**Tripwire**。

## 7. Ethical Hacking 與 Penetration Testing

### 外部滲透測試廠商

- 使用外部 penetration service firm 的合理理由：
  - 較少公司內部偏見
  - 可能更完整報告
  - 可模擬外部威脅
- 不應選的理由：**他們使用 highly talented ex-hackers**。

### Ethical hacking

- Ethical hacking 可使用可能影響系統的工具，但必須被授權、受控並避免負面修改目標系統。
- 錯誤說法：ethical hackers never use tools that have the potential of affecting servers or services。

## 8. 其他常考點

### OSI / Network layer

- Network layer 常見協定：
  - IP
  - OSPF
  - RIP
- HTTP 屬於 application layer，不是 network layer。

### Session layer

- Session layer 可建立 peer hosts 間的 logical persistent connection。
- 題目中的模式選 **full duplex**。

### Operational controls

- Preventive operational controls：
  - 保護 laptops、PCs、workstations
  - 控制病毒
  - 控制 media access and disposal
- Security awareness and technical training 較偏 administrative / awareness，不是題目中的 preventive operational control。

### Software license violations

- 偵測軟體授權違規的最佳方式：定期掃描 PC，確認是否安裝未授權軟體。

### BCP review

- Business Continuity Plan 至少 **每年檢視一次**。

## 9. 題號速查

| 題號 | 關鍵答案 |
| --- | --- |
| Q1 | Accountability 需要 audit mechanisms |
| Q2 | Review audit records = detection |
| Q3 | HIDS 依賴 audit trails |
| Q4 | Controls effectiveness 由 systems auditor 衡量 |
| Q5 | Invalid OLTP transactions 寫入報告並 review |
| Q6 | Data owner 決定資料保護控制 |
| Q8 | Signature-based = match predefined known attack events |
| Q9 | IDS 監控 network traffic / host audit logs |
| Q10 | NIDS 即時監控 network traffic |
| Q11 | HIDS resident on critical hosts |
| Q13 | NIDS payload/header review 可偵測 DoS |
| Q14 | HIDS review system/event logs |
| Q15 | HIDS drawback: invasive to host OS |
| Q17 | Signature IDS 只能偵測已知 signatures |
| Q18 | Statistical anomaly-based IDS 建立 normal profile |
| Q19 | Anomaly IDS 容易 false positive |
| Q20 | Display folder contents 破壞蒐證流程 |
| Q21 | Unknown attacks / unusual traffic = traffic anomaly-based |
| Q22 | HIDS 不會消耗大量資源是錯的 |
| Q23 | Response 不是 IDS alarm fundamental component |
| Q25 | HTTP 不是 Network layer |
| Q26 | Session layer mode: full duplex |
| Q27 | Tripwire 較不像 hacker tool |
| Q28 | Centralized reporting 較不會阻礙 incident reporting |
| Q29 | 按 patch process 更新不違反 due diligence |
| Q30 | Honeypot 用於學習攻擊並強化防禦 |
| Q31 | IS auditors 向 senior management 報告 controls effectiveness |
| Q32 | IDS 常見實作：NIDS + HIDS |
| Q33 | NIDS 監控 discrete network segment |
| Q34 | Knowledge-based = signature; behavior-based = statistical anomaly |
| Q35 | Knowledge-based IDS 用攻擊/弱點資料庫 |
| Q36 | Knowledge-based IDS 比 behavior-based 常見 |
| Q37 | Behavioral characteristics = anomaly detection |
| Q38 | Assurance procedures 確保控制機制落實 policy |
| Q39 | Known attack database = signature-based IDS |
| Q40 | Detect intrusions 最有用的是 audit trails |
| Q42 | Anomaly detection 較容易 false positive |
| Q44 | Accountability 需要 audit trails |
| Q45 | Ex-hackers 不是使用外部滲透廠商的有效理由 |
| Q46 | Ethical hacker 不是永遠不能用可能影響服務的工具 |
| Q48 | Accountability 透過 I&A 與 audit function |
| Q49 | Tripwire 較不像 hacker tool |
| Q50 | Normal behavior 變化大造成 anomaly IDS false positives |
| Q51 | Self-audit vs independent audit = objectivity |
| Q52 | Account review 不應判斷 user-chosen password strength |
| Q53 | Due care 不相關：profit |
| Q54 | Awareness/training 不是 preventive operational control |
| Q55 | Incident tracking 不屬於 audit trail control assessment |
| Q58 | 定期掃描 PC 偵測未授權軟體 |
| Q59 | Clipping levels 設 baseline，超過門檻才記錄分析 |
| Q60 | BCP 至少每年 review |

## 10. 快速記憶

- Accountability = I&A + audit trail。
- IDS 最常考 NIDS vs HIDS。
- Signature 會抓已知攻擊；Anomaly 會抓異常行為，但 false positive 多。
- Audit trail 是偵測與問責的核心。
- Data owner 決定資料保護需求；auditor 評估控制有效性。
- 鑑識蒐證不要碰原始資料，先 image、hash、write blocker。
