# Topic 2 知識概念整理：可信系統、Life Cycle 與開發治理

Topic 2 不只是可信系統架構，還有很多「life cycle」概念。這裡的 life cycle 可以分成幾條線一起看：Orange Book 的 assurance life cycle、系統開發生命週期 SDLC、政策文件生命週期、測試與變更生命週期、以及系統上線後的 operation/maintenance。

## 1. Orange Book / TCSEC 與 Assurance

Orange Book / TCSEC 主要關注可信系統如何保護 confidentiality，背景常和 Bell-LaPadula、多層級安全、TCB 放在一起理解。

TCSEC 常見概念包含：

- TCB Trusted Computing Base。
- Reference Monitor。
- Security Kernel。
- Assurance。
- Covert Channel Analysis。
- Configuration Management。

Orange Book 把 system assurance 分成兩大類：

- Operational Assurance：系統運作時能否安全、可靠地執行。
- Life-Cycle Assurance：系統從設計、開發、交付、維護到變更是否被控制。

Operational Assurance 偏「系統正在運作時是否可信」，常涉及 system architecture、system integrity、covert channel analysis、trusted facility management、trusted recovery。

Life-Cycle Assurance 偏「系統整個生命週期是否被管理」，常涉及 configuration management、trusted distribution、開發與維護控制。題目若問 Life-Cycle Assurance 需要什麼，常見答案會靠近 Configuration Management 與 Trusted Facility Management。

ITSEC 補足 Orange Book 對 integrity 與 availability 的不足，因此不只關注 confidentiality。

## 2. TCB、Reference Monitor 與 Security Kernel

TCB 是系統內所有保護機制的總和，包含硬體、韌體與軟體。只要某元件會影響安全決策或安全保護能力，就可能屬於 TCB。

Security Perimeter 是 TCB 與非 TCB 的邊界。邊界內是可信元件，邊界外是不被信任或不需信任的元件。

Reference Monitor 是抽象概念：所有 subject 存取 object 的請求，都必須由它檢查是否允許。它負責比較 subject clearance 與 object classification，並套用存取規則。

Security Kernel 是 Reference Monitor 的實作，位於 TCB 核心。它必須符合三個特性：

- Completeness：所有存取都必須經過它。
- Isolation：它本身不能被未授權修改或繞過。
- Verifiability：它足夠小且清楚，可以被完整測試與驗證。

Trusted system 不是「絕對安全」，而是有足夠、有效、可獨立驗證的證據，證明它能執行宣稱的安全功能。

## 3. System Integrity、Configuration Management 與 Data Hiding

System Integrity 在 TCSEC 中常指硬體與韌體的週期性驗證，確認 TCB 的 on-site hardware/firmware 正常運作。

Configuration Management 的重點是控制與稽核 TCB 的變更。它不是單純記錄硬體清單，而是讓組織知道誰改了什麼、為何修改、是否核准、是否影響安全。主要目標是 system stability。

一個好的 configuration process 會確保：

- requirements 保持清楚、有效、可追蹤。
- 變更、標準與需求被即時且精確地溝通。
- 對 TCB 或重要系統元件的修改有審核與紀錄。

Data hiding 是系統分層與隔離的概念。某一層的功能不應任意存取其他層資料，藉此限制資訊外洩與未授權干擾。

## 4. 作業系統保護架構

Ring Architecture 使用多個 execution domains 或 privilege levels 隔離權限。Ring 0 通常最有權限，應只放最可信的核心功能。

Process Isolation 防止一個 process 讀寫另一個 process 的記憶體或資源，是多使用者、多程序系統的基本安全能力。

Protection Domain 是一組 subject 可存取的 object 與權限邊界。不同 domain 的存在，是為了限制執行干擾與權限擴散。

Object Reuse 是資源重複使用時的資料殘留問題。例如記憶體、buffer、磁碟區塊、印表機記憶體若沒有清除，下一個使用者可能看到前一個使用者的資料。

Fault-tolerant system 能在故障發生時持續運作。Fail-safe 則是在故障時進入較安全狀態。兩者都和可用性與安全狀態有關，但重點不同。

## 5. Covert Channel、Residual Data 與媒體清除

Covert channel 是利用系統原本不是用來傳資料的機制偷偷傳遞資訊。常見類型：

- Storage covert channel：透過儲存狀態或資源狀態傳訊。
- Timing covert channel：透過時間差、延遲、頻率傳訊。

Residual data / Remanence 是資料在刪除或釋放後仍殘留在媒體或記憶體中的現象。這是 object reuse 與 media reuse 的核心風險。

Clearing 與 Purging 的差異：

- Clearing：讓一般系統工具或鍵盤攻擊無法取得資料。
- Purging：讓更高階的實驗室攻擊也難以復原資料。

Purging 的強度高於 clearing。

Degaussing 使用強磁場破壞磁性媒體資料，適用於磁帶、磁片、磁性硬碟等。它不適合 CD-ROM 或唯讀光學媒體；這類媒體通常要 physical destruction。

## 6. CIA 在系統層的意義

Confidentiality 是防止未授權 disclosure。存取控制、加密、分級標籤都是常見手段。

Integrity 是防止未授權 alteration，並維持內外一致性。例如交易資料不能被任意改動，系統狀態要與業務規則一致。

Availability 是確保資源在需要時可用。備援、容錯、災難復原、容量規劃都與 availability 有關。

IPsec 可在 IP 層提供機密性、完整性、驗證與防重放等保護，但它不會自動解決所有應用層安全問題。

## 7. 組織角色與安全治理

Management team 對組織資訊系統安全負最終責任。安全責任不能完全外包給 IT 或安全部門。

Senior management 通常負責決定資訊與系統需要的保護程度，因為這涉及風險承擔與資源配置。

Data / Information Owner 最了解資料價值、敏感度與業務影響，因此最適合決定資料保護需求與適當控制。

GDPR 與資料治理角色常一起考，但名詞重點不同：

- Data Subject：個人資料所屬的自然人，例如客戶、員工、使用者。
- Data Controller：決定個人資料處理目的與方式的人或組織。GDPR 下主要責任通常落在 controller，包含合法處理依據、告知、權利回應、資料保護與合規證明。
- Data Processor：依 controller 指示處理個人資料的第三方或服務提供者，例如雲端代管、薪資系統供應商。Processor 不能自行決定處理目的。
- Data Custodian：負責實作與維護資料保護控制的人或團隊，例如備份、權限設定、加密、系統維運。Custodian 執行保護措施，但通常不是決定資料分類或保護需求的人。
- Data Owner：從業務角度決定資料分類、敏感度、保護需求與可接受風險。SSCP 題目常考 owner 決定「資料該怎麼被保護」，custodian 負責「照要求維護與操作控制」。
- Data Protection Officer, DPO：在特定情況下協助監督 GDPR 合規、提供建議、作為監管機關與資料主體的聯絡點。DPO 不是資料的 owner，也不是替 management 承擔最終責任的人。

GDPR 重要原則可以用來判斷隱私題：

- Lawfulness, fairness, transparency：處理個資要有合法依據，且要清楚告知。
- Purpose limitation：只能為明確、合法目的收集與使用。
- Data minimization：只收集達成目的所需的最少資料。
- Accuracy：資料要維持正確，必要時更新或刪除。
- Storage limitation：保存時間不能超過必要期間。
- Integrity and confidentiality：用適當安全控制保護資料。
- Accountability：controller 必須能證明自己符合 GDPR。

常見 GDPR 考點：

- 個人資料 personal data 是可識別自然人的資料；special categories 包含健康、種族、宗教、政治觀點、生物識別等，通常需要更高保護。
- 資料主體權利包含 access、rectification、erasure / right to be forgotten、restriction、portability、objection。
- Personal data breach 通常要在知道後 72 小時內通知監管機關；若對個人有高風險，也可能要通知受影響的資料主體。
- Privacy by design / by default：系統與流程設計時就內建隱私保護，預設只處理必要資料。
- DPIA Data Protection Impact Assessment：高風險個資處理前做影響評估，例如大規模監控或大量敏感資料處理。
- 跨境傳輸個資需要合適保護機制，例如 adequacy decision 或 standard contractual clauses。

Security analyst 在 application development 或 acquisition project 中，常扮演 control evaluator 與 consultant，協助專案選擇合適安全控制，而不是取代 business owner 或 project manager。

Information security policy 是高層次方向，說明組織的安全目標、責任、規範基礎與行為要求。它的主要目的，是讓 users、administrators、managers 知道自己保護技術與資訊資產的義務。

Policy、Standard、Guideline、Procedure 的差異：

- Policy：高層原則與要求，說明必須遵守什麼。
- Standard：具體、強制的最低要求。
- Guideline：建議做法，有彈性。
- Procedure：逐步操作指引，用來滿足控制要求。

Security planning 通常先建立目標、列出假設、評估替代方案。建立 audit function 很重要，但不一定是初期規劃的第一步。

## 8. NIST 控制分類：Management、Operational、Technical

NIST 常把安全控制分成三大類。這個分類不是在說控制「長什麼樣子」，而是在說控制主要靠什麼方式降低風險。

| 類型 | 重點 | 典型問題 |
| --- | --- | --- |
| Management controls | 管理、治理、風險決策 | 風險是否被評估？控制是否被審查？系統是否被正式授權？ |
| Operational controls | 人員、流程、日常作業 | 人員怎麼操作？事件怎麼處理？備份與媒體怎麼管理？ |
| Technical controls | 系統與技術機制 | 系統如何驗證、授權、記錄、加密與隔離？ |

Management controls 偏「管理層如何決定與監督安全」。它通常不直接在系統內攔截使用者，而是建立方向、責任、風險接受與審查機制。

常見分支概念：

- Risk assessment / risk analysis：辨識威脅、弱點、影響與可能性。
- Security planning：建立安全目標、假設與替代方案。
- Security controls review：檢查控制是否仍有效，屬於 management control。
- Certification and Accreditation：技術評估與管理層正式授權。
- Policy governance：核准政策、分派責任、接受剩餘風險。

Operational controls 偏「人和流程如何每天把安全做出來」。它通常由人員執行，或透過日常程序維持安全。

常見分支概念：

- Personnel security：聘用、轉調、離職、背景審查、職責分離。
- Backup and recovery：備份、復原、測試備份有效性。
- Contingency planning：緊急應變、持續營運、災難復原。
- Operations procedures：日常操作程序、交接、批次作業控制。
- Media protection and disposal：媒體存取、保存、清除、銷毀。
- Incident response / reporting：事件通報、追蹤、處置與結案。
- Awareness and training：讓使用者知道責任與正確行為，常被歸在 administrative / awareness 類。

Technical controls 偏「系統本身自動執行的安全機制」。它們通常透過硬體、軟體、韌體或網路設備實作。

常見分支概念：

- Identification and Authentication：帳號、密碼、token、multi-factor authentication。
- Access control：ACL、RBAC、MAC、DAC、least privilege。
- Audit mechanisms：audit logs、audit trails、automated violation reports。
- Encryption：資料傳輸或儲存加密。
- IDS / monitoring：偵測可疑行為或違規事件。
- Security kernel / reference monitor：系統層存取決策與隔離。

容易混淆的判斷方式：

- 「review of security controls」通常偏 Management，因為它是管理層面的審查。
- 「backup and recovery」通常偏 Operational，因為它是日常作業與復原流程。
- 「auditing」這個字本身要看語境：audit trail / automated violation report 偏 Technical；security audit function 或 control review 偏 Management。
- 「training / awareness」不是 Technical，也不是 Physical；它偏 Administrative / Operational。
- 「encryption、access control software、authentication mechanism」通常偏 Technical。

可以把這三類想成一條鏈：Management 決定要保護什麼與接受多少風險，Operational 讓人和流程每天照規則運作，Technical 則把規則落到系統機制裡。

## 9. Policy / Standard 文件生命週期

Topic 2 的 life cycle 不只指軟體，也包含安全政策、標準與程序文件的生命週期。

政策、標準、程序等文件常見發展流程：

1. Initiation：確認需要建立或修訂文件。
2. Evaluation：評估現況、法規、風險與需求。
3. Development：撰寫文件內容。
4. Approval：由適當管理層正式核准。
5. Publication：發布給相關人員。
6. Implementation：落實到流程、系統與訓練。
7. Maintenance：定期檢視並更新。

Maintenance phase 的目標不是重寫全部內容，而是確保文件仍然正確、有效、符合現況與風險。

好的安全政策不應只短期導向。它需要能支援組織長期安全方向，但也要透過 maintenance 階段持續調整。

## 10. SDLC：安全應放在整個生命週期

Security by Design 的意思是把安全視為整體系統設計的一部分，而不是等系統完成後再補。

SDLC 中的安全觀念：

- Initiation / Planning：建立安全目標、做 risk analysis、確認業務與安全需求。
- Functional design analysis and Planning：發展 security requirements。
- Development / Acquisition：把安全需求轉成設計、控制與實作。
- Testing / Evaluation：驗證功能、安全控制與錯誤處理。
- Implementation：上線、移轉、訓練與正式啟用。
- Operation / Maintenance：維持 authentication、access control、patching、monitoring、backup、變更控制。
- Disposal：資料清除、媒體銷毀、帳號與連線移除。

題目常見重點：

- Risk analysis 最有用的時機是 project initiation and planning，因為越早知道風險，越能影響設計。
- Security requirements 應在 functional design analysis and planning 階段建立。
- Security department 應在 requirements development 就參與，而不是等 testing 才加入。
- Risk reduction 應套用到整個 SDLC，不是只在 development phase。
- Operation/Maintenance 階段重點之一，是維持使用者與程序的 proper authentication，確保 access control decisions 正確。

安全最有效、也最經濟的方式，是系統一開始就被設計成能提供必要安全，而不是最後才外掛控制。

## 11. 使用者參與與開發方法

系統要符合 business objectives，關鍵是 user involvement in system specification and acceptance。使用者若沒有參與需求定義，系統可能安全但不符合業務，或符合技術規格但無法被接受。

Inadequate SDLC 最嚴重的風險之一，是 user participation 不足，導致需求不清、控制設計錯誤、系統上線後不符合實際作業。

常見開發方法：

- RAD：Rapid Application Development，是一種 development methodology，強調快速迭代。
- Prototyping：用原型確認需求與介面。優點是節省時間與成本；但原型也要受變更控制，不能讓臨時設計直接變成未控管的正式系統。
- Spiral model：風險導向的反覆模型，可視為 meta-model，能整合多種開發模型。
- CMM：Capability Maturity Model，核心前提是軟體品質取決於開發與維護流程品質。

## 12. Verification、Validation、Acceptance、Certification、Accreditation

這幾個詞很容易混在一起，可以這樣分：

- Verification：確認系統是否依規格正確建置。重點是「有沒有照規格做」。
- Validation：確認系統是否滿足使用者與業務需求。重點是「是不是做了真正需要的東西」。
- Acceptance：使用者或業務方接受系統。重點是「使用者認可可用」。
- Certification：詳細檢查與測試 IT system 或 product 的安全功能，確認控制正確、有效，且沒有明顯邏輯弱點。
- Accreditation：管理層正式接受系統整體安全狀態，授權它在特定安全設定與控制下運作。

Accreditation 是正式授權上線的管理宣告；Certification 是支援這個決策的技術評估。

## 13. 測試生命週期與測試資料

Debugging 是找出並修正 coding flaws。

White-box testing 了解內部邏輯與程式結構，從內部測試。Black-box testing 不看內部實作，只從輸入輸出與外部行為測試。

Unit testing 應在模組設計時就被考慮。Test data 也應該是規格的一部分，而不是最後臨時湊資料。

Regression testing 是變更後重跑部分測試情境或測試計畫，確認修正沒有引入新錯誤。

Security testing 是確認 modified or new system 有適當 access controls，且沒有引入會影響其他系統的 security holes。

Stress / volume testing 需要接近真實負載，但不能暴露敏感資料。較佳環境是 test environment using sanitized live workloads data。

不應直接使用未遮蔽 live data 進行測試。若需要接近真實情境，應先 sanitization。

測試環境與開發環境分離的主要理由，是控制 test environment 的穩定性。測試環境若常被開發人員任意修改，就無法可靠判斷測試結果。

Bottom-up testing 的優點是較早測試底層元件；Top-down testing 則較早看到主要功能與流程。

Test data generator 可系統性產生測試資料，用於正式實作前測試程式。

## 14. Change Control、Backup 與維護

Change control 是預防性控制，重點包含：

- 程式設計師不應直接存取 production data。
- change request 應包含日期、描述、成本分析、預期影響。
- 變更要經過核准、測試、紀錄與回復計畫。
- emergency changes 也要有明確程序與事後審查。

Source comparison program 可偵測程式碼是否被未授權修改，較偏 detective control。

Backup 的關鍵不是只有「有備份」，而是定期備份並測試備份資料有效性。沒測過的備份不能算可靠復原能力。

Maintenance 階段持續處理 patching、monitoring、account review、authentication、access control、backup validation、configuration updates。

Due care / due diligence 在 SDLC 中常落在 software plans and requirements，因為這是把合理謹慎與持續查核轉成設計與控制要求的早期位置。

## 15. Database Security

資料庫安全不只靠登入權限，也要處理查詢結果與資料組合造成的風險。

Perturbation 是加入雜訊或調整資料，避免使用者從統計結果還原敏感資料。

Cell suppression 是隱藏特定敏感儲存格或結果。

Partitioning 是把資料分割到不同區域，降低單一查詢取得過多敏感資訊的機會。

Aggregation 是多個低敏感資料組合後形成更高敏感資訊。例如單筆資料不敏感，但大量資料合併後可推導出機密。

Inference 是透過合法可見資料推論出不可直接存取的敏感資訊。

Polyinstantiation 是在多層級資料庫中，允許同一 key 或物件對不同安全層級呈現不同資料，以隱藏高層級資料存在。

Trusted front-end 常用於把多層級安全能力加到原本不支援 MLS 的 DBMS 前方，替後端做安全檢查。

## 16. 攻擊、測試設備與營運控制

Passive attack 是觀察或蒐集資料，例如 sniffing、shoulder surfing、scavenging。Data diddling 是修改資料，屬於 active attack。

Compiled code 的風險是比 interpreted code 更難檢查與理解，惡意邏輯可能不容易被發現。

Communications test equipment 必須被政策控管，因為它可能用來觀察網路上傳輸的資訊。

Pseudo flaw 是刻意設計的假弱點，用來引誘或偵測攻擊者行為。

Operational controls 包含 backup and recovery、contingency planning、operations procedures、personnel security。Auditing 通常不是 operational control 的典型例子。

Management controls 包含安全控制審查、風險管理與治理活動。

Personnel security 是 operational control，因為它處理人員聘用、轉調、離職與職責。

Rotation of duties 可降低長期舞弊與共謀風險。Limiting local access of operations personnel 是職責分離的一種，會迫使高風險操作需要不同職能人員合作。

## 17. Security Modes

Security mode 描述在同一系統中，使用者 clearance、need-to-know 與資料分級之間的關係。

Dedicated Security Mode：所有使用者都有所有資料的 clearance、need-to-know 與正式授權。

System-High Security Mode：所有使用者都有系統內最高等級資料的 clearance，但不一定對所有資料都有 need-to-know。

Compartmented Security Mode：所有使用者有最高等級 clearance，但對不同 compartment 的 need-to-know 不一定相同。

Multilevel Security Mode：不是所有使用者都有所有資料的 clearance，系統必須同時處理不同 clearance 與不同 classification 的資料。

## 18. Life Cycle 題目速查

| 問到的概念 | 要抓的原理 |
| --- | --- |
| Orange Book assurance | Operational Assurance + Life-Cycle Assurance |
| Life-Cycle Assurance | Configuration Management、Trusted Facility Management |
| Configuration Management | 控制與稽核 TCB 變更，維持 system stability |
| Security by Design | 從設計初期納入安全最有效、最經濟 |
| Risk analysis | Project initiation and planning 最有用 |
| Security requirements | Functional design analysis and planning 建立 |
| Security department involvement | Requirements development 就要參與 |
| Risk reduction | 應套用整個 SDLC |
| Business objectives | User involvement in specification and acceptance |
| RAD | Development methodology |
| Prototyping | 節省時間與成本，但仍需控制 |
| Spiral model | Meta-model，風險導向反覆模型 |
| CMM | 軟體品質取決於開發與維護流程品質 |
| Verification | 是否照規格做 |
| Validation | 是否滿足使用者需求 |
| Acceptance | 使用者或業務方接受 |
| Certification | 技術性安全評估與測試 |
| Accreditation | 管理層正式授權系統運作 |
| Regression testing | 變更後重跑測試，避免新錯誤 |
| Security testing | 確認 access controls 並避免 security holes |
| Sanitized live workload | 適合 stress testing，避免資料外洩 |
| Separate test/development | 控制測試環境穩定性 |
| Change request | 日期、描述、成本分析、預期影響 |
| Emergency change | 要有程序與事後審查 |
| Backup | 定期備份並測試有效性 |
| Policy document life cycle | Initiation、Evaluation、Development、Approval、Publication、Implementation、Maintenance |
| Operation/Maintenance | 維持 authentication、access control、patching、backup |

## 19. 控制分類速查

| 問到的概念 | 控制類型 | 理由 |
| --- | --- | --- |
| Risk assessment / risk analysis | Management | 管理風險與決策 |
| Security controls review | Management | 審查控制有效性 |
| Certification | Management | 支援管理授權的安全評估 |
| Accreditation | Management | 管理層正式接受風險並授權運作 |
| Security planning | Management | 建立目標、假設與策略 |
| Backup and recovery | Operational | 日常作業與復原流程 |
| Contingency planning | Operational | 事件或災害時維持營運 |
| Personnel security | Operational | 人員流程與職責管理 |
| Operations procedures | Operational | 日常操作程序 |
| Media disposal | Operational | 媒體處理與銷毀流程 |
| Awareness / training | Operational | 透過人員行為降低風險 |
| Access control software | Technical | 系統自動執行授權 |
| Identification and authentication | Technical | 系統驗證身分 |
| Audit trails | Technical | 系統產生可追蹤紀錄 |
| IDS / violation reports | Technical | 系統偵測與回報異常 |
| Encryption | Technical | 技術性保護資料 |
| Reference monitor / security kernel | Technical | 系統層強制存取控制 |

## 20. GDPR 與資料角色速查

| 概念 | 核心意思 |
| --- | --- |
| Data Subject | 個人資料所屬的人 |
| Data Controller | 決定個資處理目的與方式 |
| Data Processor | 依 controller 指示處理個資 |
| Data Custodian | 維護與操作資料保護控制 |
| Data Owner | 決定資料分類、敏感度與保護需求 |
| DPO | 協助監督 GDPR 合規與溝通 |
| Personal data | 可識別自然人的資料 |
| Special categories | 健康、宗教、生物識別等高敏感個資 |
| Data minimization | 只收集必要資料 |
| Purpose limitation | 只為明確目的使用資料 |
| Accountability | Controller 要能證明合規 |
| Data subject rights | 存取、更正、刪除、限制、可攜、反對 |
| Breach notification | 通常 72 小時內通知監管機關 |
| Privacy by design/default | 設計與預設狀態都內建隱私保護 |
| DPIA | 高風險個資處理前的影響評估 |
| Cross-border transfer | 需 adequacy decision、SCC 等保護機制 |

## 21. 其他概念速查

| 概念 | 核心意思 |
| --- | --- |
| TCB | 系統中所有保護機制的總和 |
| Security Perimeter | TCB 與非 TCB 的邊界 |
| Reference Monitor | 檢查 subject 存取 object 的抽象機制 |
| Security Kernel | Reference Monitor 的實作 |
| System Integrity | 驗證 TCB 硬體與韌體正常 |
| Covert Channel | 利用非正式通道傳資訊 |
| Storage / Timing | 兩大 covert channel 類型 |
| Remanence | 刪除後仍殘留的資料 |
| Clearing | 防一般攻擊復原 |
| Purging | 防實驗室等級復原 |
| Degaussing | 適用磁性媒體 |
| Aggregation | 低敏感資料合併成高敏感資訊 |
| Inference | 從可見資料推論敏感資訊 |
| Polyinstantiation | 不同安全層級看到不同資料 |
| Procedure | Step-by-step instructions |
| Passive attack | Sniffing、shoulder surfing、scavenging |
| Data diddling | Active attack |
