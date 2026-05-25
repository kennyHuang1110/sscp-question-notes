# Topic 2 考題重點整理

## 1. Orange Book、TCSEC 與 Assurance

### Orange Book / TCSEC

- Orange Book 主要評估可信系統的安全能力，偏重 confidentiality。
- ITSEC 補足 Orange Book 較不足的 integrity 與 availability。
- TCSEC 中常見概念：
  - Trusted Computing Base, TCB
  - Reference Monitor
  - Security Kernel
  - Assurance
  - Covert Channel Analysis
  - Configuration Management

### System Assurance 兩大類

- Orange Book 的兩種 system assurance：
  - Operational Assurance
  - Life-Cycle Assurance

### Operational Assurance

- 關注系統運作時是否能正確執行安全功能。
- 常見要求包含：
  - System Architecture
  - System Integrity
  - Covert Channel Analysis
  - Trusted Facility Management
  - Trusted Recovery

### Life-Cycle Assurance

- 關注系統從設計、開發、維護到發佈的整個生命週期。
- Topic 2 題目重點：
  - Configuration Management
  - Trusted Facility Management
- Configuration Management 控制並稽核 TCB 的變更。
- 主要目標是 system stability。

### System Integrity

- 指 hardware / firmware 有定期測試，確認其功能正確。
- 題目若出現「periodically validate correct operation of on-site hardware and firmware elements of the TCB」，答案是 System Integrity。

### Data Hiding

- 在 B3 / A1 等級中，data hiding 指系統功能分層。
- 每一層只能存取該層允許的資料，不能跨層任意存取。

## 2. TCB、Reference Monitor、Security Kernel

### Trusted Computing Base, TCB

- TCB 是電腦內所有保護機制的總和，包含 hardware、firmware、software。
- 它負責支撐與執行安全政策。
- 題目若問「sum of protection mechanisms inside the computer」，答案是 TCB。

### Security Perimeter

- Security perimeter 是一道想像線，分隔 TCB 中可信元件與不可信元件。

### Reference Monitor

- Reference Monitor 會比較 subject 與 object 的安全標籤，並決定是否允許存取。
- 它是 access control decision 的核心概念。
- Reference Validation Mechanism 實作的就是 Reference Monitor concept。

### Security Kernel

- Security Kernel 是 TCB 中實作 Reference Monitor 的硬體、韌體與軟體機制。
- 它不是純概念，而是實際負責執行安全政策的元件集合。
- Security Kernel 必須符合三個條件：
  - Completeness：所有存取都必須經過它。
  - Isolation：不能被竄改，且與其他程序隔離。
  - Verifiability：足夠小且可測試、可驗證。

### Trusted System

- Trusted system 需要能執行安全政策，並提供可獨立驗證的證據，證明安全機制足夠且有效。
- 題目中若提到「efficient and reliable」不一定是 trusted system 的定義重點；重點是 sufficient、effective、independently verifiable evidence。

## 3. 系統架構與處理器概念

### Ring Architecture

- 超過兩個 execution domains 或 privilege levels 的架構稱為 Ring Architecture。
- Ring 0 通常是最高權限層。

### Process Isolation

- 防止一個 process 存取另一個 process 資料的是 Process Isolation。
- 題目若說「processes constrained so each cannot access objects outside permitted domain」，就是 secure system 的邏輯隔離。

### Protection Domain

- Protection Domain 用來保護程式不受未授權修改或 execution interference。

### Memory Segmentation / Object Reuse

- 若 OS 允許 memory/object 被多個使用者或程序依序使用，但沒有清除，就會造成 residual data disclosure。
- Object reuse 重點：重新分配前要清除記憶體、buffer、printer/terminal local memory。
- 單純 deleting files 不足以處理 object reuse。

### Memory Addressing

- Direct addressing：指令中直接指定實際記憶體位置。
- Indirect addressing：指令指定的位置內，存放真正目標位置的位址。

### Processor / Architecture

- Pipelining：重疊不同指令的步驟以提升效能。
- VLIW：compiler 或 pre-processor 將指令拆成可同時執行的基本操作。
- CISC：源於早期 instruction fetch 是最長週期的設計背景。

### Fault / Failure

- Fault-tolerant system：能偵測 fault，並修正或繞過 fault 繼續運作。
- Fail-safe：系統失敗或偵測到故障時，自動讓系統處於安全狀態。

## 4. Covert Channel 與資料外洩

### Covert Channel

- Covert channel 是非預期的通訊路徑，且不受正常安全機制保護。
- 兩大類：
  - Storage covert channel
  - Timing covert channel

### Residual Data / Remanence

- Remanence 是媒體刪除後仍殘留的資料。
- Media reuse 的主要問題是 data remanence。
- Shared object/memory 沒有 refresh，最可能造成 residual data disclosure。

### Clearing vs Purging

- Clearing：讓資料無法被一般 keyboard attack 還原。
- Purging：讓資料無法被 laboratory attack 還原。
- Purging 強度高於 clearing。

### Degaussing / Destruction

- Degaussing 適用磁性媒體，例如 floppy disk、video tape、magnetic hard disk。
- Read-only media 不能靠 degaussing 清除。
- CD-ROM 最安全處置方式：physical destruction。

## 5. CIA 與基本安全性質

### Confidentiality

- 防止資訊被未授權的人或程序揭露。
- Opposite of confidentiality：disclosure。
- 「只有應該存取資料的人能存取」是 confidentiality。

### Integrity

- 確保訊息送出與收到一致，未被有意或無意修改。
- Opposite of integrity：alteration。
- 常見 integrity goals：
  - 防止未授權使用者修改。
  - 防止授權使用者不當修改。
  - 維持 internal / external consistency。
- 「防止不當揭露路徑」是 confidentiality，不是 integrity goal。

### Availability

- 授權實體需要時，系統或資源可依效能規格被存取與使用。
- Opposite of availability：destruction。
- Network availability 直接影響 availability。

### IPsec

- IPsec 可提供資料機密性、端點身分確認、流量封包交換計數等特性。
- IPsec 不保證資料按照送出順序抵達。

## 6. 管理責任、政策與控制

### 管理階層責任

- 組織內資訊系統安全的最終責任：Management team。
- 決定資訊系統資源保護等級的主要責任：Senior Management。
- 確保 IT 系統與資料 CIA 控制到位：System and Information Owners。
- 決定資料保護所需技術控制：Data or Information Owner。

### Security Analyst / Security Officer

- Security analyst 在開發或採購專案中的角色：control evaluator & consultant。
- 資安政策開發最適合由 Security Officer 監督。

### Information Security Policy

- 有效政策應：
  - 被所有利害關係人理解與支持。
  - 指定責任與權限。
  - 包含 separation of duties。
  - 能適應環境變化。
- 不應只有 short- to mid-term focus。
- 政策主要目的：告知 users、administrators、managers 保護技術與資訊資產的義務。
- 「詳細規定硬體與軟體如何使用」比較像 standard/procedure，不是 policy 的核心定義。

### Policy / Standard / Guideline / Procedure

- Policy：高層原則與要求。
- Standard：必須遵守的具體規範。
- Guideline：建議性作法。
- Procedure：逐步操作指示，用來滿足控制要求。

### Security Planning

- 初步安全規劃包含：
  - establish objectives
  - list planning assumptions
  - determine alternate courses of action
- Establish a security audit function 不是 preliminary planning step。

### IT Security Organization

- 資安組織較理想的定位：由 Chief Security Officer 領導，直接向 CEO 報告。
- IT security measures 應依組織安全目標量身打造，不是越複雜越好。

## 7. SDLC 與系統開發安全

### Security by Design

- 應從一開始就把必要安全性設計進系統。
- 在系統原始設計階段加入安全最有效、也最經濟。
- Security should be treated as an integral part of overall system design。
- Security department 應在 requirements development 階段就介入。

### Risk Analysis / Risk Reduction

- Risk analysis 最適合在 project initiation and planning 階段套用。
- SDLC 中 risk reduction 應平均套用到所有階段。
- Due care / due diligence 通常在 software plans and requirements 階段處理。

### Security Requirements

- IT system life cycle 中，security requirements 通常在 functional design analysis and planning 發展。
- 建立良好 security policy 作為設計基礎，最關心的是 initiation phase。
- Operation/Maintenance phase 重點之一：維持使用者與程序的 authentication，確保正確 access control decision。

### User Involvement

- 系統開發最能確保符合 business objectives：user involvement in specification and acceptance。
- 資訊系統常無法滿足使用者需求的主因：使用者參與需求定義不足。
- SDLC 方法不足的最大風險：系統無法滿足 business and user needs。

### RAD / Prototyping / Spiral

- RAD 是 development methodology。
- Prototyping 優點：可節省時間與成本。
- Spiral model 是 meta-model，結合多種軟體開發模型。
- CMM：軟體品質取決於開發與維護流程品質。

### Verification / Validation / Acceptance

- Verification：是否正確建構產品。
- Validation：產品是否符合專案目標或使用需求。
- Acceptance：確認供應的解決方案滿足使用者需求。
- Accreditation：指定授權單位正式宣告系統可在特定安全配置與保護措施下運作。
- Certification：對安全特性進行技術性檢查與評估，常是 accreditation 的前置。
- Evaluation：詳細檢查與測試 IT 系統或產品安全功能是否正確有效，且無邏輯漏洞。

## 8. 測試與變更控制

### Debugging / Testing

- Debugging 目的：找出並修正程式 coding flaws。
- White-box testing：檢查程式內部邏輯結構。
- Black-box testing：不看內部結構，從外部輸入輸出測試。
- Regression testing：修改後重跑部分測試，確認沒有引入新錯誤。
- Security testing：確認新系統或修改後系統有適當 access controls，且不引入 security holes。

### Test Data

- 不應使用 live data 覆蓋所有情境。
- Test data 應納入規格。
- Test data generator 可產生隨機測試資料。
- 最佳 stress testing 環境：test environment + sanitized live workload data。

### Change Control

- Preventive controls 範例：
  - 拒絕 programmer 存取 production data。
  - change request 包含日期、描述、成本分析、預期影響。
  - 建立 emergency change procedures。
- 定期執行 source comparison program 屬於 detective control，不是 preventive control。

### Backup

- 備份應先回答：what records to backup。
- 之後才是何時備份、放哪裡、如何保存。

## 9. Database Security

### DBMS Security 技術

- Perturbation：加入雜訊或調整結果，保護敏感資料。
- Cell suppression：隱藏特定儲存格資料。
- Partitioning：分割資料以降低敏感資訊暴露。
- Padded cells 不是 DBMS security 常見控制。

### Aggregation

- Aggregation：組合多個低敏感資料，推得較高敏感資訊。
- 題目若說「collection of information items required to be classified at higher level than individual items」，答案也是 Aggregation。

### Inference

- Inference：從可取得資料推論出受限制資訊。
- Topic 2 中 aggregation 是主要正解，inference 常作為干擾選項。

### Polyinstantiation

- Polyinstantiation：在資料庫中建立多個同鍵但不同安全層級或不同值的資料列，用來隱藏敏感資訊。
- 題目若問「used in database information security to hide information」，答案是 Polyinstantiation。

### Trusted Front-End

- 將 multilevel security retrofitting 到 DBMS 時，常用 trusted front-end。

## 10. 攻擊與惡意程式風險

### Passive vs Active

- Passive attack 範例：
  - scavenging
  - shoulder surfing
  - sniffing
- Data diddling 是主動竄改資料，不是 passive attack。

### Compiled Code

- Compiled code 風險較高，因為惡意程式碼可嵌入其中且較難偵測。

### Test Equipment

- 通訊測試設備需納入安全政策，因為可能被用來瀏覽或攔看網路上的資訊。

### Pseudo Flaw

- Pseudo flaw 是故意植入的表面漏洞，用來誘捕入侵者。

## 11. Operational / Management / Technical Controls

### Operational Controls

- 常見 operational controls：
  - backup and recovery
  - contingency planning
  - operations procedures
  - personnel security
- Auditing 在題目中不是 operational control 的例子。

### Management Controls

- Security controls review 屬於 management control。

### Personnel Security

- Personnel security 最相關的是 operational controls。

### Separation / Rotation of Duties

- Systems programmer 存取實作安全的系統軟體，可能違反 separation of duties。
- Rotation of duties 可中斷或降低 collusion 機會。
- Limiting local access of operations personnel 可能迫使 operator 要與其他職能人員共謀才可取得未授權資料。

## 12. Security Modes

### Dedicated Security Mode

- 所有使用者都具備系統內所有資訊的 clearance、need-to-know，並通過適當授權。

### System-High Security Mode

- 所有使用者都具備系統內所有資訊的 clearance，但不一定對所有資料都有 need-to-know。

### Compartmented Security Mode

- 使用者需有部分資訊的 clearance，且依 compartment / need-to-know 控制。

### Multilevel Security Mode

- 不要求所有使用者都具備系統處理所有資訊的 clearance。
- 題目問「哪個 mode 不要求所有使用者都擁有所有資訊 clearance」，答案是 Multilevel security mode。

## 13. 易混淆陷阱

- Reference Monitor 是存取決策概念；Security Kernel 是實作 Reference Monitor 的 TCB 元件。
- TCB 是保護機制總和；Security Perimeter 是分隔可信與不可信元件的界線。
- Security Kernel 三要件是 Completeness、Isolation、Verifiability，不是 CIA。
- Operational Assurance 與 Life-Cycle Assurance 是 Orange Book 的兩大 assurance。
- Configuration Management 是控制與稽核 TCB 變更，不只是記錄變更。
- System Integrity 在 TCSEC 題目中偏 hardware/firmware periodic testing。
- Covert channel 主要分 storage 與 timing。
- Residual data 與 remanence 是 object/media reuse 的大陷阱。
- Clearing 防 keyboard attack；purging 防 laboratory attack。
- Degaussing 不適用 CD-ROM/read-only media；CD-ROM 最安全是 physical destruction。
- Security by design 最有效；不要等設計完成才加安全。
- Security department 要在 requirements development 就參與。
- White-box 看內部邏輯；black-box 看外部行為。
- Regression testing 是確認修正沒有造成新錯誤。
- Aggregation 是低敏感資料組合出高敏感資訊；polyinstantiation 是資料庫隱藏資訊技術。
- Procedure 是 step-by-step instructions；policy 是高層要求。

## 14. 快速背誦表

| 關鍵字 | 答案方向 |
| --- | --- |
| Compare subject/object labels | Reference Monitor |
| Hardware/firmware periodic validation | System Integrity |
| Operational + Life-Cycle | Orange Book assurance |
| Audit/control TCB changes | Configuration Management |
| Storage and timing | Covert channels |
| Sum of hardware/firmware/software protection | TCB |
| Implements reference monitor | Security Kernel |
| Trusted vs untrusted boundary | Security Perimeter |
| Completeness, Isolation, Verifiability | Security Kernel requirements |
| More than two privilege levels | Ring Architecture |
| Prevent process accessing another process | Process Isolation |
| Used object not refreshed | Residual data disclosure |
| Data left after erase | Remanence |
| Keyboard attack protection | Clearing |
| Laboratory attack protection | Purging |
| CD-ROM disposal | Physical destruction |
| Security built from start | Most effective/economical |
| Requirements development | Security department involvement |
| Project initiation/planning | Risk analysis |
| User needs met | Acceptance |
| Formal approval to operate | Accreditation |
| Internal logic testing | White-box testing |
| Rerun tests after changes | Regression testing |
| Hide DB information | Polyinstantiation |
| Lower data combined into higher sensitivity | Aggregation |
| Step-by-step instructions | Procedure |
| Passive attack exception | Data diddling |
| Personnel security | Operational control |
| Review of security controls | Management control |
| Not all users need all clearances | Multilevel security mode |

