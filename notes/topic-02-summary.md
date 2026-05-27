# Topic 2 知識概念整理：可信系統、系統安全架構與開發治理

Topic 2 的核心是「系統本身怎樣被設計成可信」，包含 Orange Book / TCSEC、TCB、Reference Monitor、作業系統保護、資料殘留、SDLC、資料庫安全與組織治理。它比 Topic 1 更偏系統架構與管理流程。

## 1. Orange Book / TCSEC 與 Assurance

Orange Book / TCSEC 主要關注可信系統如何保護 confidentiality，尤其是多層級安全環境。它的背景與 Bell-LaPadula 有密切關係。

TCSEC 常見概念包含：

- TCB Trusted Computing Base。
- Reference Monitor。
- Security Kernel。
- Assurance。
- Covert Channel Analysis。
- Configuration Management。

Orange Book 把 system assurance 分成兩大類：

- Operational Assurance：系統運作時是否能安全、可靠地執行。
- Life-Cycle Assurance：系統從設計、開發、維護到變更是否被控制。

Operational Assurance 常涉及 system architecture、system integrity、covert channel analysis、trusted facility management、trusted recovery。

Life-Cycle Assurance 常涉及 configuration management、trusted distribution、設計與測試流程。重點是控制系統整個生命週期，而不是只看上線當下。

ITSEC 補足 Orange Book 對 integrity 與 availability 的不足，因此不只關注 confidentiality。

## 2. TCB、Security Perimeter、Reference Monitor、Security Kernel

TCB 是系統內所有保護機制的總和，包含硬體、韌體與軟體。只要某個元件會影響安全決策或保護能力，就可能屬於 TCB。

Security Perimeter 是 TCB 與非 TCB 的邊界。邊界內是可信元件，邊界外是不被信任或不需信任的元件。

Reference Monitor 是抽象概念：所有 subject 存取 object 的請求，都必須由它檢查是否允許。它是存取控制決策的守門機制。

Security Kernel 是 Reference Monitor 的實作，位於 TCB 核心。它必須符合三個特性：

- Completeness：所有存取都必須經過它。
- Isolation：它本身不能被未授權修改或繞過。
- Verifiability：它足夠小且清楚，可以被測試與驗證。

Trusted system 不是「完全安全」的意思，而是有足夠、有效、可獨立驗證的證據，證明它能執行宣稱的安全功能。

## 3. System Integrity、Configuration Management 與 Data Hiding

System Integrity 在 TCSEC 中常指硬體與韌體的週期性驗證，確認 TCB 的 on-site hardware/firmware 正常運作。

Configuration Management 的重點是控制與稽核 TCB 的變更。目標是 system stability：知道系統目前狀態、誰改了什麼、改動是否被核准。

Data hiding 是系統分層與隔離的概念。某一層的功能不應任意存取其他層資料，藉此限制資訊外洩與未授權干擾。

## 4. 作業系統保護架構

Ring Architecture 使用多個執行層級或 privilege levels 隔離權限。Ring 0 通常最有權限，應只放最可信的核心功能。

Process Isolation 防止一個 process 讀寫另一個 process 的記憶體或資源。它是多使用者、多程序系統的基本安全能力。

Protection Domain 是一組 subject 可存取的 object 與權限邊界。不同 domain 的存在，是為了限制執行干擾與權限擴散。

Object Reuse 是資源重複使用時的資料殘留問題。例如記憶體、buffer、磁碟區塊、印表機記憶體若沒有清除，下一個使用者可能看到前一個使用者的資料。

Memory addressing 概念：

- Direct addressing：直接指定實體或邏輯位置。
- Indirect addressing：透過指標或中介位置找到目標資料。

處理器架構概念：

- Pipelining：把指令處理拆成階段，提高吞吐量。
- CISC：複雜指令集，單一指令可做較多事。
- VLIW：由編譯器安排多個操作並行執行。

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

Security analyst 常是控制評估者與顧問，協助分析風險與建議控制。Security officer 通常負責推動安全計畫、政策與合規。

Information security policy 是高層次方向，說明組織的安全目標、責任、規範基礎與行為要求。它不應寫成太細的操作步驟。

Policy、Standard、Guideline、Procedure 的差異：

- Policy：高層原則與要求，說明必須遵守什麼。
- Standard：具體、強制的最低要求。
- Guideline：建議做法，有彈性。
- Procedure：逐步操作指引。

Security planning 通常先建立目標、列出假設、評估替代方案。建立 audit function 很重要，但不一定是初期規劃的第一步。

## 8. Security by Design 與 SDLC

安全最有效、也最經濟的方式，是在系統設計一開始就納入，而不是系統完成後再補。

Security by Design 的意思是把安全視為整體系統設計的一部分，包括需求、架構、資料流程、權限、稽核、錯誤處理與維運。

Risk analysis 通常在 project initiation and planning 階段進行，因為早期了解風險才能影響設計選擇。

Security requirements 應在 functional design analysis and planning 階段被明確化。Security department 應參與 requirements development，協助把安全政策轉成需求。

User involvement 很重要，因為系統安全不能脫離業務目標。使用者參與 specification and acceptance，能提高系統符合實際需求的機率。

常見開發方法：

- RAD：快速應用開發，重視快速迭代。
- Prototyping：用原型確認需求與介面。
- Spiral model：風險導向的反覆模型，可視為 meta-model。
- CMM：描述軟體流程成熟度。

Verification、Validation、Acceptance、Certification、Accreditation：

- Verification：確認系統是否依規格正確建置。
- Validation：確認系統是否滿足使用者與業務需求。
- Acceptance：使用者或業務方接受系統。
- Certification：技術與安全評估，確認控制是否符合要求。
- Accreditation：管理層正式授權系統投入運作。

## 9. 測試、變更控制與備份

Debugging 是找出並修正 coding flaws。

White-box testing 了解內部邏輯與程式結構，從內部測試。Black-box testing 不看內部實作，只從輸入輸出與外部行為測試。

Regression testing 是變更後重跑測試，確認新修改沒有破壞既有功能。

Security testing 是檢查系統安全控制與安全缺陷，例如 access controls 是否有效。

測試資料不應直接使用未遮蔽的 live data。若需要接近真實負載，應使用 sanitization 後的資料。

Change control 是預防性控制，重點包含：

- 程式設計師不應直接存取 production data。
- 變更要提出 request、核准、測試與記錄。
- emergency change 也要有事後審查。

Source comparison program 可偵測程式碼是否被未授權修改，較偏 detective control。

Backup 的關鍵是先決定哪些 records 需要備份，再決定頻率、保留期、媒體、離站保存與復原測試。

## 10. Database Security

資料庫安全不只靠登入權限，也要處理查詢結果與資料組合造成的風險。

Perturbation 是加入雜訊或調整資料，避免使用者從統計結果還原敏感資料。

Cell suppression 是隱藏特定敏感儲存格或結果。

Partitioning 是把資料分割到不同區域，降低單一查詢取得過多敏感資訊的機會。

Aggregation 是多個低敏感資料組合後形成更高敏感資訊。例如單筆資料不敏感，但大量資料合併後可推導出機密。

Inference 是透過合法可見資料推論出不可直接存取的敏感資訊。

Polyinstantiation 是在多層級資料庫中，允許同一 key 或物件對不同安全層級呈現不同資料，以隱藏高層級資料存在。

Trusted front-end 常用於把多層級安全能力加到原本不支援 MLS 的 DBMS 前方，替後端做安全檢查。

## 11. 攻擊、測試設備與營運控制

Passive attack 是觀察或蒐集資料，例如 sniffing、shoulder surfing、scavenging。Data diddling 是修改資料，屬於 active attack。

Compiled code 的風險是比原始碼更難檢查與理解，惡意邏輯可能不容易被發現。

Communications test equipment 必須被政策控管，因為它可能用來觀察網路上傳輸的資訊。

Pseudo flaw 是刻意設計的假弱點，用來引誘或偵測攻擊者行為。

Operational controls 包含 backup and recovery、contingency planning、operations procedures、personnel security。Auditing 通常不是 operational control 的典型例子。

Management controls 包含安全控制審查、風險管理與治理活動。

Personnel security 是 operational control，因為它處理人員聘用、轉調、離職與職責。

Rotation of duties 可降低長期舞弊與共謀風險。Limiting local access of operations personnel 是職責分離的一種，會迫使高風險操作需要不同職能人員合作。

## 12. Security Modes

Security mode 描述在同一系統中，使用者 clearance、need-to-know 與資料分級之間的關係。

Dedicated Security Mode：所有使用者都有所有資料的 clearance、need-to-know 與正式授權。

System-High Security Mode：所有使用者都有系統內最高等級資料的 clearance，但不一定對所有資料都有 need-to-know。

Compartmented Security Mode：所有使用者有最高等級 clearance，但對不同 compartment 的 need-to-know 不一定相同。

Multilevel Security Mode：不是所有使用者都有所有資料的 clearance，系統必須同時處理不同 clearance 與不同 classification 的資料。

## 13. 概念對照速查

| 概念 | 核心意思 |
| --- | --- |
| TCB | 系統中所有保護機制的總和 |
| Security Perimeter | TCB 與非 TCB 的邊界 |
| Reference Monitor | 檢查 subject 存取 object 的抽象機制 |
| Security Kernel | Reference Monitor 的實作 |
| System Integrity | 驗證 TCB 硬體與韌體正常 |
| Configuration Management | 控制與稽核 TCB 變更 |
| Covert Channel | 利用非正式通道傳資訊 |
| Storage / Timing | 兩大 covert channel 類型 |
| Remanence | 刪除後仍殘留的資料 |
| Clearing | 防一般攻擊復原 |
| Purging | 防實驗室等級復原 |
| Degaussing | 適用磁性媒體 |
| Security by Design | 從設計初期納入安全 |
| Verification | 是否照規格做 |
| Validation | 是否符合使用者需要 |
| Accreditation | 管理層正式授權上線 |
| Aggregation | 低敏感資料合併成高敏感資訊 |
| Inference | 從可見資料推論敏感資訊 |
| Polyinstantiation | 不同安全層級看到不同資料 |
