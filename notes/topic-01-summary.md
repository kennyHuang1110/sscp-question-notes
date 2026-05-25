# Topic 1 考題重點整理

## 1. 存取控制核心觀念

### CIA 與存取控制目的

- 存取控制的主要目的：保護機密性、完整性、可用性，也就是 CIA。
- 題目若問「控制資訊系統與網路存取是為了保護什麼」，優先選 Confidentiality, Integrity, Availability。
- Identification and Authentication 是多數 access control system 的基石。
- Identification 建立使用者身分與 accountability。
- Authentication 驗證使用者是否真的是該身分。

### Least Privilege 最小權限

- 使用者只取得完成工作所需的最低權限。
- 常見答案語氣：
  - access only information for which they have a need to know
  - only rights necessary to perform their work
- Least privilege 可降低 authorization creep，也就是職務變動後權限越累越多。

### Need-to-know

- 即使 clearance 足夠，也仍需有業務上的 need-to-know。
- 在 MAC 題目中，clearance/classification 不等於自動可讀所有資料。

### Separation of Duties 職責分離

- 避免單一人員完成整個敏感流程。
- 正確例子：operator 不可修改 system time。
- 題目若出現「forcing collusion」通常是在描述職責分離或限制單一角色權限。

### Accountability 可歸責性

- 需要 audit mechanisms / audit trails。
- 系統要能追蹤「誰做了什麼」。
- Host-based IDS 最依賴 audit trails。

## 2. DAC、MAC、RBAC、Lattice

### DAC Discretionary Access Control

- 權限由 data owner 決定。
- 在 discretionary access environment 中，授權他人存取資料的是 Data Owner。
- Identity-based access control 是 DAC 的一種，根據個人身分給權限。

### MAC Mandatory Access Control

- 由系統根據 security labels 強制執行，不由資料擁有者任意授權。
- Sensitivity label 通常包含：
  - classification
  - category set / compartment set
- Object sensitivity label = single classification + compartment/category set。
- Incomparable labels：兩個 label 的 category/compartment 彼此不完全包含。

### RBAC Role-Based Access Control

- 根據使用者在組織中的 role/title/job function 授權。
- 題目若說 access controls are based on individual's role or title，答案是 RBAC。

### Lattice-Based Access Control

- 用上下界描述 subject/object 的安全層級。
- 題目關鍵字：least upper bound、greatest lower bound。
- Subject 的 upper bound 需要等於或高於 object 才能存取。

### Access Control Matrix

- 用 subject/object 組合列出每個 subject 對各 object 的權限。
- 題目若說「quickly summarize what permissions a subject has for various system objects」，就是 Access Control Matrix。

## 3. 安全模型

### Bell-LaPadula

- 重點：機密性 Confidentiality。
- 以 military classification 與 clearance 為基礎。
- Orange Book / TCSEC 主要基於 Bell-LaPadula。
- DoD multilevel security policy 的早期數學模型。
- 不處理 integrity 或 conflict of interest。
- 常見考法：
  - The Orange Book is founded upon Bell-LaPadula.
  - Military classification of data and people with clearances = Bell-LaPadula.

### Biba

- 重點：完整性 Integrity。
- 與 Bell-LaPadula 常被拿來比較。

### Clark-Wilson

- 重點：商業完整性、well-formed transactions、separation of duties。
- 關鍵元件：
  - Constrained Data Item, CDI
  - Integrity Verification Procedure, IVP
  - Transformation Procedure, TP

### Brewer-Nash / Chinese Wall

- 重點：conflict of interest。
- 題目若問哪個模型處理利益衝突，通常選 Brewer-Nash。

### Take-Grant

- 偏權限轉移模型。
- Topic 1 中主要作為干擾選項。

## 4. Authentication 認證

### 三大認證因素

- Something you know：password、PIN、passphrase。
- Something you have：token、smart card。
- Something you are：biometrics。
- Two-factor authentication：使用兩種不同因素。
- Robust authentication 常和 two-factor / stronger validation 相關。

### Password / PIN / Passphrase

- Password 的主要用途是 authenticate the user，不是 identify user。
- PIN 是 confidential number。
- Passphrase 通常比 password 長，系統可能轉換成 virtual password。
- 好密碼特徵：長、複雜、不像字典字、不含明顯個資。
- 系統產生密碼：
  - 通常較難猜。
  - 但使用者較難記。
  - 若產生演算法外洩，整個系統可能受影響。
  - 不應選「更容易受 brute force / dictionary attack」。

### Password Aging

- 密碼變更週期取決於：
  - 資訊敏感度/criticality
  - 密碼使用頻率

### Preventive Login Controls

- Password aging、minimum password length、account expiration 都屬 preventive login control。
- Last login message 是 detective/notification 性質，不是 preventive login control。

### Password Sniffing 防護

- 防止 sniffing 造成密碼外洩：
  - one-time passwords
  - encryption
- Static/reusable password 風險較高。

### SSO Single Sign-On

- 優點：convenience + centralized administration。
- 主要風險：一組憑證洩漏後，攻擊者可能取得最大範圍的未授權存取。

## 5. Kerberos、PKI、AAA、Remote Access

### Kerberos

- Trusted third-party authentication protocol。
- 依賴 symmetric ciphers。
- 可防 replay/playback attack。
- KDC 持有使用者與服務的 cryptographic keys。
- KDC / TGS / Authentication Server 是高價值目標，需防 physical attack 與 malicious code。
- Kerberos ticket 類似 PKI 裡的 public-key certificate。

### SESAME

- 用來改善 Kerberos 一些弱點。
- 使用 public key cryptography 來分配 secret keys，並提供額外 access control support。

### PKI 與 Smart Card

- Smart card 在 PKI 中的主要角色：
  - tamper-resistant
  - mobile storage
  - 使用者 private key 的儲存與應用

### RADIUS / TACACS+ / DIAMETER

- AAA = Authentication, Authorization, Accounting。
- Administration 不是 AAA 的 A。
- RADIUS client/server 使用 UDP。
- RADIUS 對 Access-Request 的回應包含：
  - Access-Accept
  - Access-Reject
  - Access-Challenge
- Access-Granted 不是標準 RADIUS response。
- 初版 TACACS 使用 UDP。

### EAP

- Extensible Authentication Protocol。
- PPP 的認證框架，支援多種認證機制，例如 cleartext password、challenge-response、arbitrary dialog sequences。

### Remote Access

- 遠端存取安全目標包含：
  - reliable authentication of users and systems
  - protection of confidential data
  - manageable access control
- Automated login for remote users 不是安全目標。
- 外部透過 Internet 存取 LAN 前，優先考慮 proper authentication options。
- IP-based authentication 對 mobile users 不友善。
- 消除 dial-up RAS hacking vector 的較佳方式：將 RAS 放在 firewall 外，合法使用者仍需通過 firewall 認證。

## 6. Biometrics 生物辨識

### 基本觀念

- Biometrics 使用 fingerprint、retina、iris 等生理或行為特徵認證。
- 真正 positive identification 主要建立在 physical attributes。
- 常見可用部位：hands、face、eyes。
- 本地冒名 masquerading attack 最佳防護：biometrics。

### Retina vs Iris

- Retina scan 測量眼底血管圖案：pattern of blood vessels at the back of the eye。
- Iris scanner 安裝問題：光學單元需避免陽光直接照進 aperture。
- Retina scan 使用者接受度最低，因為侵入感較高。

### FAR / FRR / CER

- FAR False Acceptance Rate：錯誤接受未授權者。
- FRR False Rejection Rate：錯誤拒絕合法使用者。
- 提高系統敏感度會更嚴格，通常導致 FRR 上升。
- CER / Crossover Error Rate 可快速比較生物辨識裝置準確度。
- CER 越低，準確度越好。
- 對 access control 來說，最應避免 Type II Error，也就是 false acceptance。

### 生物辨識選型因素

- 最重要特性：accuracy。
- 也需考慮 enrollment time、throughput rate、acceptability。
- 題目中可接受 throughput rate：約 10 subjects per minute。
- 成本 cost 不屬於題目所問的「security characteristic」。

## 7. Physical Security 實體安全

### Physical Controls

- Guards、locks、lighting、fences、facility construction materials、server room security、cable protection、magnetic door/window switches。
- Passwords 是 logical control，不是 physical access control。
- Employee badge 偏實體控管，不是 logical control。
- Training 是 administrative control，不是 physical control。

### Guards

- Guard 適合需要 human judgment / discriminating judgment 的情境。
- 最後一道實體防線：people。

### CPTED

- Crime Prevention Through Environmental Design。
- 透過實體環境設計影響人類行為，降低犯罪。

### Facility Vulnerability Assessment

- 可用 inspection、history of losses、security controls 評估。
- Security budget 不是直接評估 vulnerability 的方法。

### Alarm Types

- Auxiliary station alarm：資料中心警報自動傳到當地 municipal fire/police alarm circuit，再轉給警消與總部。
- 題目關鍵字：local municipal fire or police alarm circuits。

### Door Locks

- 有人值守設施的 automatic locks 應設為 fail-safe，確保緊急時可逃生。
- Fail-secure 通常偏保護資產，但可能影響人員安全。

### Perimeter Detection

- Vibration detection devices 常見問題：容易受非攻擊性干擾影響。

## 8. Controls 分類與配對

### Preventive / Detective

- Preventive control：防止事件發生。
- Detective control：偵測並揭露事件。
- Audit record review 是 detection。
- Timely review of access audit records = detection。

### Administrative / Technical / Physical

- Administrative：政策、流程、訓練、職責分離。
- Technical / Logical：access control software、encryption、audit trail、IDS。
- Physical：guards、locks、fences、lighting、badges、doors。

### 常見配對

- Preventive/Administrative：soft mechanisms，支援 access control 目標，例如政策、程序、職責分離。
- Preventive/Technical：OS、software、hardware 中的 encryption / access control。
- Detective/Technical：IDS、自動從 audit trail 產生 violation report。
- Detective/Physical：sensor、camera、alarm，需要人員判斷是否真有威脅。

### Nonrepudiation

- Nonrepudiation 最佳分類：logical control。

## 9. Audit、IDS、Assessment

### Clipping Level

- Clipping level：在產生 violation record 前，允許或忽略的違規次數門檻。
- 題目常問「number of violations accepted/forgiven before record is produced」。

### Vulnerability Assessment

- Network-based vulnerability assessment 也稱 active vulnerability assessment。

### 評估 I&A Controls

- 適合檢查：
  - 是否維護並核准授權使用者清單。
  - 密碼是否至少每 90 天或必要時更換。
  - 不活躍 user IDs 是否停用。
- Incident reporting process 比較不是直接評估 identification/authentication control 的問題。

### 評估 Physical Access Controls

- 適合檢查：
  - 管理階層是否定期審查有實體存取權的人員。
  - 進入 computer room / media library 是否需要 key 或 access device。
  - 訪客是否簽到且有人陪同。
- OS 是否防止繞過 security software 是 logical/technical control，不是 physical access control。

## 10. 易混淆陷阱

- Authentication 驗證身分；Identification 宣告身分；Authorization 授權能做什麼。
- Password 用來 authenticate，不是 identify。
- Data Owner 授權 DAC 存取，不是 security manager。
- MAC label 包含 classification + category/compartment set。
- Bell-LaPadula 管 confidentiality，不管 integrity/conflict of interest。
- Clark-Wilson 管 commercial integrity，不是 confidentiality。
- RADIUS 是 AAA，不是 Administration。
- RADIUS 用 UDP；Access-Granted 不是標準回應。
- SSO 方便但單點憑證洩漏風險大。
- Biometrics 提高敏感度會提高 FRR，不是 FAR。
- CER 越低越準。
- Retina scan 看眼底血管；iris scan 要注意光學安裝與陽光。
- Password、encryption、access profile 是 logical controls；badges/locks/guards 是 physical controls。
- Last login message 不是 preventive login control。
- Audit trails / audit review 多半是 detective control。

## 11. 快速背誦表

| 關鍵字 | 答案方向 |
| --- | --- |
| Data owner grants access | DAC |
| Classification + category set | Sensitivity label |
| Military classification / clearance | Bell-LaPadula |
| Orange Book / TCSEC | Bell-LaPadula |
| Confidentiality model | Bell-LaPadula |
| Integrity model with CDI/IVP/TP | Clark-Wilson |
| Conflict of interest | Brewer-Nash |
| Subject/object permissions table | Access Control Matrix |
| Least upper bound / greatest lower bound | Lattice |
| Trusted third-party authentication | Kerberos |
| Symmetric ciphers / tickets / replay protection | Kerberos |
| Public key certificates analogy | Kerberos tickets |
| Authentication, Authorization, Accounting | AAA |
| RADIUS transport | UDP |
| PPP authentication framework | EAP |
| Longer password-like phrase | Passphrase |
| Violations before logging | Clipping level |
| Audit review | Detective control |
| IDS and audit violation reports | Detective/Technical |
| Cameras/sensors with human judgment | Detective/Physical |
| Policies/procedures | Administrative |
| Encryption/access control software | Technical/Logical |
| Guards/locks/fences | Physical |
| CER lowest | Most accurate biometric |
| Higher sensitivity | Higher FRR |
| Most critical biometric trait | Accuracy |
| Most intrusive / lowest acceptance | Retina scan |
| Door in manned facility | Fail-safe |

