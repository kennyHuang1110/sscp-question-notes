# Topic 1 知識概念整理：存取控制、身分驗證與實體安全

這份整理不是用來背題目答案，而是把題目背後的安全概念拆開。Topic 1 的主軸是「如何確認人是誰、能做什麼、如何留下責任紀錄，以及如何用邏輯與實體控制保護資產」。

## 1. 安全目標與存取控制的基本觀念

資訊安全常用 CIA 來描述保護目標：

- Confidentiality 機密性：資料只讓被授權的人看到，避免 disclosure。
- Integrity 完整性：資料維持正確、未被未授權修改，避免 alteration。
- Availability 可用性：系統與資料在需要時可用，避免 destruction 或服務中斷。

存取控制不是單一技術，而是一整套流程：

1. Identification：使用者宣稱自己的身分，例如輸入 user ID。
2. Authentication：系統驗證這個身分是否可信，例如密碼、智慧卡、生物特徵。
3. Authorization：確認這個身分可以做哪些事。
4. Accountability：透過稽核紀錄，把行為連回特定身分。

Identification 建立「誰做的」這個基礎；Authentication 證明「這個人真的是他宣稱的人」；Authorization 才決定「他能不能做這件事」。

## 2. 最小權限、Need-to-know 與職責分離

Least privilege 最小權限是只給使用者完成工作所需的最低權限。它的目的不是讓管理變麻煩，而是降低誤用、濫用與帳號被盜時的影響範圍。

Need-to-know 是更細的資料存取判斷：即使使用者有足夠 clearance，也不代表他需要看所有同等級資料。常見於軍事、政府或高度分級環境。

Separation of duties 職責分離是避免單一人員能獨自完成高風險流程。例如操作者不應能任意修改系統時間，因為這會影響稽核紀錄可信度。職責分離的重點是讓濫用需要多人共謀，提高攻擊與舞弊成本。

Authorization creep 權限蔓延是人員換職位後舊權限未移除，導致權限越累越多。最小權限與定期權限審查就是用來處理這類問題。

## 3. 存取控制模型

DAC Discretionary Access Control 是由資料擁有者決定誰可以存取資料。因為權限由 owner 授予，所以彈性高，但也較依賴個人判斷。

MAC Mandatory Access Control 是由系統根據安全標籤與規則強制執行。使用者不能自行改變規則。典型元素包含：

- Subject：要存取資料的人或程序。
- Object：被存取的資料或資源。
- Clearance：subject 的授權等級。
- Classification：object 的分級。
- Category / Compartment：資料所屬領域或隔間。

MAC 的 sensitivity label 通常包含「單一 classification」與「category/compartment set」。兩個標籤如果彼此都不包含對方的所有 category，就可能是 incomparable。

RBAC Role-Based Access Control 是依角色、職稱或職務功能給權限。它比逐一設定個人權限更適合組織管理，例如會計、系統管理員、客服主管各有不同權限集合。

Lattice-based access control 用數學格狀結構描述等級與範圍，常用於多層級安全環境。概念上，subject 必須在足夠高的位置，才能存取對應 object。

Access control matrix 是把 subject 與 object 排成表格，格子裡寫明權限，例如 read、write、execute。它不是特定產品，而是一種描述權限關係的方法。

## 4. 經典安全模型

Bell-LaPadula 主要處理機密性，常和軍事分級、多層級安全、Orange Book / TCSEC 放在一起理解。它關心的是資料不要流到低權限者手上。

Biba 主要處理完整性。它的方向與 Bell-LaPadula 類似但目標不同：不是防止機密外洩，而是避免低可信來源污染高可信資料。

Clark-Wilson 也是完整性模型，但更偏商業交易環境。它強調 well-formed transactions、職責分離與受控資料項目：

- CDI：Constrained Data Item，受控資料。
- IVP：Integrity Verification Procedure，完整性檢查程序。
- TP：Transformation Procedure，允許修改 CDI 的受控程序。

Brewer-Nash / Chinese Wall 用來處理利益衝突。使用者一旦接觸某一客戶或公司資料，就會被限制不能再接觸競爭方資料。

Take-Grant 關心權限如何被授予與傳遞，常用來分析 subject 和 object 之間權限流動。

## 5. 身分驗證因素

驗證因素分成三大類：

- Something you know：密碼、PIN、passphrase。
- Something you have：token、smart card。
- Something you are：指紋、虹膜、視網膜等生物特徵。

Two-factor authentication 是同時使用兩種不同類型的因素。兩個密碼不算真正的雙因素，因為它們都屬於 something you know。

Robust authentication 通常指較強的驗證方式，例如雙因素、動態密碼、challenge-response、持續驗證等。

## 6. 密碼、PIN、Passphrase 與登入控制

Password 的主要用途是 authenticate user，不是 identify user。User ID 用於 identification，password 用於 authentication。

PIN 是 confidential number，通常搭配卡片或 token 使用。Passphrase 比一般 password 長，系統可能將它轉換成 virtual password 或雜湊值進行驗證。

密碼管理重點包含：

- 長度與複雜度降低猜測成功率。
- password aging 根據資料重要性與使用頻率調整更換週期。
- account expiration 可限制臨時帳號或離職帳號風險。
- last login message 屬於偵測或通知性控制，不是預防性登入控制。

防止 password sniffing 的核心是避免靜態密碼在網路上被重複使用。常見作法是加密通訊、使用 one-time password 或 challenge-response。

Single Sign-On 的優點是方便使用者與集中管理；風險是單一憑證若被攻破，攻擊者可取得更大的存取範圍。

## 7. Kerberos、PKI 與 AAA

Kerberos 是 trusted third-party authentication protocol，主要依賴 symmetric cryptography。它用 tickets 讓服務端相信使用者已被可信第三方驗證，並可降低 replay attack 風險。

Kerberos 的核心是 KDC，通常包含 Authentication Server 與 Ticket Granting Server。因為 KDC 持有重要金鑰，所以它本身必須被嚴格保護。

Kerberos tickets 在信任模型中可類比 PKI 的 public-key certificates：兩者都讓其他系統信任某個身分或關係，但實作機制不同。

SESAME 可視為對 Kerberos 弱點的改良方向之一，使用 public key cryptography 協助分送 secret keys，並提供更多 access control 支援。

Smart card 在 PKI 中常用來安全保存並使用使用者 private key，重點是 tamper-resistant、可攜帶、可在卡片內部執行金鑰操作。

AAA 代表：

- Authentication：驗證身分。
- Authorization：決定權限。
- Accounting：記錄使用情況與責任歸屬。

RADIUS 使用 UDP。RADIUS server 對 Access-Request 的典型回應是 Access-Accept、Access-Reject、Access-Challenge，不是 Access-Granted。TACACS+ 與 DIAMETER 也是常見 AAA 相關協定。

EAP 是可擴充的驗證框架，支援 PPP 等環境中的多種驗證方法，例如明文密碼、challenge-response 或其他對話式流程。

## 8. 遠端存取安全

遠端存取的基本安全目標是：

- 可靠驗證使用者與系統。
- 保護傳輸中的機密資料。
- 能管理使用者對系統與網路資源的存取。

自動登入不是安全目標，因為它可能繞過驗證與責任追蹤。

IP-based authentication 對 mobile users 不友善，因為使用者位置與 IP 可能經常變動。遠端存取應優先考慮適當的 authentication options，而不是只依賴來源位址。

對舊式 dial-up RAS，若要降低被當成攻擊入口的風險，可把 RAS 放在 firewall 外側，讓合法使用者仍必須通過 firewall 驗證。

## 9. 生物辨識

Biometrics 使用人的物理或行為特徵進行驗證，例如 fingerprint、retina、iris、hand geometry。它特別適合防止本機冒用，因為攻擊者不能只靠知道密碼或偷到卡片。

Retina scan 掃描眼底血管圖樣，通常準確但侵入感高、接受度低。Iris scan 掃描虹膜圖樣，設備安裝時要注意光學 aperture，例如避免陽光直接照入。

常見評估指標：

- FAR False Acceptance Rate：錯誤接受未授權者。
- FRR False Rejection Rate：錯誤拒絕合法者。
- CER Crossover Error Rate：FAR 與 FRR 交會點，常用來比較設備準確度，越低通常越準。

提高系統敏感度會讓系統更挑剔，通常降低 FAR 但提高 FRR。對存取控制而言，最重要的是避免 false acceptance，因為那代表未授權者被放行。

除了 accuracy，也要考慮 enrollment time、throughput rate、user acceptance、使用環境與成本。成本是採購因素，但不是生物辨識本身的安全特性。

## 10. 實體安全

Physical controls 包含 guards、locks、lighting、fences、facility construction、server room security、cable protection、magnetic door/window switches、badges 等。Passwords、access profiles、user IDs 屬於 logical controls。

Guards 的價值在於 human judgment。當情境需要判斷、辨識異常或處理例外時，人員比單純設備更有彈性。

CPTED Crime Prevention Through Environmental Design 是透過環境設計影響人的行為，降低犯罪機會。例如視線清楚、動線控制、照明與區域界線。

Facility vulnerability assessment 可看 inspection、history of losses、existing security controls；security budget 不是直接評估脆弱性的依據。

Alarm 常見類型中，auxiliary station alarm 會把警報接到當地 municipal fire/police alarm circuits。

Door lock 的 fail-safe / fail-secure 要看安全與生命安全的取捨：

- Fail-safe：故障或斷電時可安全離開，常用於有人駐守設施的門。
- Fail-secure：故障或斷電時維持上鎖，重點是防止未授權進入。

Vibration detection 常見問題是容易受非攻擊因素干擾，例如天氣、車輛、環境震動。

## 11. 控制分類

依功能可分：

- Preventive control：阻止事件發生，例如 access control、locks、minimum password length。
- Detective control：發現已發生或正在發生的事件，例如 audit review、IDS、camera monitoring。
- Corrective control：事件後修復或恢復，例如 restore backup。

依型態可分：

- Administrative / Management：政策、流程、訓練、權限審查。
- Technical / Logical：OS access control、encryption、audit trail、IDS。
- Physical：guards、locks、fences、lighting、badges、doors。

常見配對：

- Preventive administrative：政策、程序、職責分離、權限核准。
- Preventive technical：加密、存取控制軟體、身份驗證機制。
- Detective technical：IDS、稽核紀錄自動產生違規報告。
- Detective physical：感測器、攝影機、警報，通常需要人判斷是否是真正威脅。

Nonrepudiation 不可否認性通常歸類為 logical control，因為它依靠技術證據讓行為者難以否認其行為。

## 12. 稽核與評估

Audit trails 是 accountability 的核心，因為它把行為、時間、身分與資源連在一起。Host-based IDS 很依賴 audit trails / event logs。

Timely review of access audit records 是 detective control，因為它用來發現問題，不是事前阻止。

Clipping level 是在產生違規紀錄前可容忍或忽略的違規次數。設定太低會造成雜訊太多，太高則可能漏掉攻擊。

Network-based vulnerability assessment 通常是 active vulnerability assessment，因為它會主動探測系統與網路。

評估 identification and authentication controls 時，會看授權使用者清單、密碼更換、閒置帳號停用等。Incident reporting process 重要，但它不是 I&A 控制的核心。

評估 physical access controls 時，會看敏感區域名單是否審查、進入 computer room / media library 是否需 key 或 access device、訪客是否登記與陪同。OS 是否防止繞過 security software 屬於 logical/technical control，不是 physical access control。

## 13. 概念對照速查

| 概念 | 核心意思 |
| --- | --- |
| Identification | 宣稱身分，建立 accountability 的起點 |
| Authentication | 驗證身分，密碼主要做這件事 |
| Authorization | 決定能做什麼 |
| Accountability | 用 audit trail 把行為連回身分 |
| DAC | Data owner 授權 |
| MAC | 系統依 label 與規則強制控管 |
| RBAC | 依角色或職務給權限 |
| Bell-LaPadula | 機密性、多層級安全、軍事分級 |
| Biba | 完整性 |
| Clark-Wilson | 商業完整性、受控交易、職責分離 |
| Brewer-Nash | 利益衝突 |
| Kerberos | trusted third-party、symmetric crypto、tickets |
| AAA | Authentication、Authorization、Accounting |
| CER | 比較生物辨識準確度，越低通常越好 |
| Audit review | Detective control |
| Guards/locks/fences | Physical controls |
| Encryption/access control software | Technical/logical controls |
