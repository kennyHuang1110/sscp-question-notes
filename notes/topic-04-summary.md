# Topic 4 知識概念整理：BCP/DRP、風險管理、事件處理與法律證據

Topic 4 的核心是「當事情出錯時，組織如何降低損失、維持營運、恢復服務，並在事件中保留法律與管理上的可追蹤性」。它把風險管理、Business Continuity Planning、Disaster Recovery Planning、backup、incident response、computer crime investigation 串在一起。

## 1. BCP 與 DRP 的差異

Business Continuity Planning, BCP，關心的是整個企業如何在中斷事件中維持關鍵業務功能。它不是只處理 IT，也不只處理資料中心，而是涵蓋所有會影響企業存續的部門與流程。

Disaster Recovery Planning, DRP，通常比較偏 IT 與技術恢復，重點是災難後如何恢復資訊系統、資料、網路與處理能力。

可以這樣分：

- BCP：讓 business 繼續運作。
- DRP：讓 IT 與基礎設施恢復。
- Incident Response：處理正在發生的安全事件。
- Contingency Planning：事先準備替代做法，讓中斷時仍有路可走。

BCP/DRP 主要處理的是 availability 與 business survivability，但也會牽涉 integrity、confidentiality、法律責任與聲譽。

## 2. Business Impact Analysis, BIA

BIA 是 BCP/DRP 的基礎。它不是先選工具或備援站點，而是先理解業務流程中斷會造成什麼影響。

BIA 會找出：

- 哪些 business functions 對企業存續最重要。
- 哪些系統支援關鍵業務。
- 中斷會造成的財務、營運、法律、聲譽影響。
- 業務流程之間的 dependencies。
- 可以容忍的 outage time。
- recovery priorities。

BIA 的重點在 business processes，所以 dependencies 很重要。單一系統本身不一定是最重要的，真正要看它支援了哪些業務，以及其他流程是否依賴它。

BIA 通常不會列出災難時要聯絡的個人名單，那比較屬於 BCP/DRP 文件內容。BIA 也不是 alternate site selection；站點選擇是在知道業務需求、RTO/RPO 後才做。

常見損失類型：

- revenue loss。
- profit loss。
- productivity loss。
- reputation loss。
- legal or regulatory impact。
- customer service impact。

Loss of skilled workers' knowledge 雖然可能造成營運問題，但不是 BIA 中典型的 specific loss criteria。

## 3. RTO、RPO 與恢復優先順序

RTO Recovery Time Objective 是系統或業務可以容忍多久中斷。它回答：「多久內要恢復？」

RPO Recovery Point Objective 是資料可以容忍遺失到哪個時間點。它回答：「最多可以丟失多少資料？」

如果一個系統交易量高、資料不能丟，RPO 會很短，可能需要 remote journaling、replication 或 near-real-time backup。

恢復優先順序應依 BIA 決定，先恢復最關鍵的 business functions。災難解除後，從 alternate site 回 primary site 時，通常先搬回 least critical components，降低主要服務再次中斷的風險。

### BIA 八個步驟與 RTO/RPO/MTO 範例

BIA 常見八個步驟：

1. 取得管理層支持，確認 BIA 的目的、範圍、資源與負責人。
2. 識別關鍵業務功能，找出哪些流程、系統、資料或服務最重要。
3. 收集業務流程資料，例如部門訪談、問卷、文件審查、系統依賴關係。
4. 評估中斷影響，包括財務、營運、法律、聲譽、客戶服務與合規影響。
5. 決定可接受中斷時間，設定 RTO、RPO、MTO/MTD。
6. 排列恢復優先順序，決定哪些業務或系統要先恢復。
7. 識別恢復所需資源與相依性，例如人員、設備、資料、供應商、網路與場地。
8. 撰寫並提交 BIA 報告，作為 BCP/DRP 的依據。

RTO/RPO/MTO 重點：

| 指標 | 意思 | 範例 |
| --- | --- | --- |
| RPO, Recovery Point Objective | 最多可接受遺失多少資料 | 最多只能遺失 15 分鐘交易資料 |
| RTO, Recovery Time Objective | 目標恢復時間 | 系統要在 4 小時內恢復 |
| MTO/MTD, Maximum Tolerable Outage/Downtime | 最大可容忍中斷時間 | 業務最多只能停 8 小時 |

大小關係：

```text
RTO <= MTO/MTD
```

範例：一間網路銀行的核心交易系統中斷時，業務最多只能停機 8 小時，否則會造成嚴重財務與法規問題，所以 MTO/MTD = 8 小時。公司為了不要超過最大容忍時間，設定 RTO = 4 小時，代表系統必須在 4 小時內恢復。同時，公司最多只能接受遺失 15 分鐘交易資料，所以 RPO = 15 分鐘。

記法：

```text
RPO 看資料遺失
RTO 看恢復時間
MTO/MTD 看最大容忍停機時間

RTO 通常小於或等於 MTO/MTD
RPO 不一定要跟 RTO 比大小
```

## 4. 替代站點與 Recovery Site Strategies

Hot site 是最完整、最快可用、也通常最昂貴的替代站點。它應包含已安裝的 computers、peripherals、climate control、cables、telecommunications、networking equipment、UPS 等。

Warm site 是部分設備已準備好的站點。它比 hot site 便宜，但啟用時間較長。

Cold site 通常只有空間、電力、空調、基本佈線或基礎設施，沒有完整處理設備。它最便宜，但恢復最慢，也最難測試。

Reciprocal agreement 是兩個組織互相承諾災難時提供資源。問題是合約常難以 enforce，且雙方災難時可能同時需要資源。

Internal hot site 是組織內部 standby ready 的替代環境，具備運行應用所需技術與設備。

選擇 alternate facility 時，最重要的是不要被同一個災難影響。距離太近可能啟用快，但若同一場地震、停電、水災或區域事件同時影響兩邊，就失去備援意義。

## 5. DRP 測試類型

BCP/DRP 必須定期測試，至少每年一次，且在重大人員、系統、基礎設施或業務變更後重新檢視。

常見測試由低風險到高風險：

- Checklist review：檢查計畫內容是否完整。
- Structured walkthrough：功能代表一起逐段 review plan。
- Simulation test：模擬災難情境與決策流程。
- Parallel test：在不停止正式營運的情況下，讓替代處理能力與正式系統並行測試。
- Full interruption test：真正切換或中斷正式環境，是最完整也最高風險的測試。

在進行 full interruption test 前，應先成功完成 parallel test。這是因為 full interruption 會真正影響營運，不能直接跳到最高風險測試。

測試結果最好能 quantitative measurement，因為數字化結果更能判斷計畫是否可行，例如實際恢復時間、資料遺失量、錯誤率、人工處理時間。

## 6. DRP 維護與文件控制

DRP 會過時，常見原因包括：

- personnel turnover。
- infrastructure and environment changes。
- business process changes。
- vendor or contact information changes。
- plan 太大、太難維護。

Continuous auditing 不會讓 DRP irrelevant。相反地，稽核與 review 可以幫助計畫保持有效。

Contingency plan 應維持 strict version control。不是每位員工都需要完整最新版計畫，因為計畫可能包含敏感資訊。應提供給 recovery personnel，並以安全方式離線保存於適當位置。

Contingency plan 常包含或附錄：

- vendor contact information。
- offsite storage information。
- alternate site information。
- role and team responsibilities。
- escalation procedures。
- restoration procedures。

BIA 可以作為參考或附錄，但 BIA 本身不是災難時逐步操作手冊。

## 7. Backup 方法與資料復原

Full backup 每次備份所有檔案。它最完整，適合 off-site archiving，也是不管使用 differential 或 incremental 都必須定期做的基礎。

Incremental backup 只備份自上次任何備份以來變更的檔案，通常會 reset archive bit。它每日備份最快、最省空間，但 restore 時需要上次 full backup 加上每一次 incremental backup。

Differential backup 只備份自上次 full backup 以來所有變更的檔案，通常不 reset archive bit。它是 additive，因此越接近下次 full backup，每日備份量越大。Restore 時需要上次 full backup 加上最新 differential backup。

簡單比較：

| 方法 | 備份速度 | 還原速度 | 空間使用 | Archive bit |
| --- | --- | --- | --- | --- |
| Full | 慢 | 快 | 最大 | 通常重設 |
| Incremental | 最快 | 最慢 | 最省 | 會重設 |
| Differential | 中等，逐日變慢 | 中等 | 逐日增加 | 不重設 |

Daily backup 不是常見標準分類，常見分類是 full、incremental、differential。

Remote journaling 是把 transaction journal entries 即時傳到 alternate site。它對 RPO 很有幫助。

Electronic vaulting 是把大量資料傳到遠端中央備份設施，通常不是像 remote journaling 那樣逐筆即時交易日誌。

Physically securing backup tapes 是 BCP/DRP 的重要功能，因為備份資料本身可能包含完整敏感資訊。

## 8. 風險管理基本語言

Threat 是可能造成損害的事件或主體，例如火災、停電、惡意員工、錯誤輸入。

Threat agent 是造成威脅的人、物或力量。

Vulnerability 是可被 threat exploited 的弱點，也就是 safeguard 缺失或不足。

Risk 是 threat agent 利用 vulnerability 造成損害的可能性與影響。

Exposure 是弱點被利用後可能造成損失的狀態或程度。

Exposure Factor, EF，是單次事件造成資產價值損失的百分比。

Residual risk 是控制實施後仍然留下的風險。

Risk management 是把風險降低到可接受程度，不是把風險完全消除。

Controls 的作用是 mitigate risk and reduce potential loss。控制通常無法保證 eliminate risk。

## 9. Quantitative Risk Analysis

定量風險分析用金額與頻率估算損失。

常見公式：

- SLE Single Loss Expectancy = Asset Value x Exposure Factor。
- ARO Annualized Rate of Occurrence = 一年內預期發生次數。
- ALE Annualized Loss Expectancy = SLE x ARO。

如果資產價值為 1,000,000，EF 為 30%，ARO 為 1/5，則：

- SLE = 1,000,000 x 0.3 = 300,000。
- ALE = 300,000 x 0.2 = 60,000。

控制成本不應高於它每年可避免的預期損失。若 countermeasure cost outweighs risk cost，通常應 accept risk。

Automated risk analysis tools 的好處，是工具內建很多資料與方法，可以加快資訊收集與分析；但它不代表使用者不需要風險分析知識。

## 10. Qualitative Risk Analysis

定性風險分析不用精確金額，而是用 high/medium/low、ranking、scenario、expert judgment 等方式排序風險。

它的優點是能快速 prioritize risks，找出需要立即改善的區域。缺點是較難直接做精確 cost-benefit analysis。

Qualitative loss 通常包含聲譽、客戶信任、員工士氣、法律或營運影響等不易直接量化的損失。

Risk assessment 能讓 contingency planning 負責人把資源集中在 identified risks，而不是平均分散。

## 11. Risk Response 與 Policy Exception

常見風險處理方式：

- Risk reduction / mitigation：加控制降低可能性或影響。
- Risk acceptance：接受風險，通常需管理層正式同意。
- Risk transfer / assignment：透過保險或外包轉移部分財務或營運風險。
- Risk avoidance：停止造成風險的活動。

偏離 organization-wide security policy 需要 risk acceptance，因為這代表組織允許某項例外風險存在。

Disaster recovery 不應被視為 discretionary expense。它是維持營運、符合法規、降低法律責任與保護企業存續的一部分。

## 12. Insurance 概念

保險是 risk transfer 的一種方式，但不能取代 BCP/DRP。保險可補償部分金錢損失，不能保證客戶回來、聲譽恢復、資料完整。

Actual Cash Valuation, ACV，通常依資產現值賠償，會考慮折舊。

Replacement Cost Valuation, RCV，通常依替換成同等新設備或資產所需成本賠償，不以折舊後價值為主。

Valuable paper insurance 通常保護重要紙本文件，但不涵蓋所有形式的資料或電子媒體。

Crime insurance policy coverage 通常與犯罪造成的損失有關，例如員工不誠實、竊盜或詐欺，但具體範圍要看保單條款。

## 13. Incident Response 與 CSIRT

Incident handling 的主要目標是讓組織更能準備、應對與降低威脅與災難造成的影響，而不是只為了起訴或收集所有證據。

CSIRT / CIRT 是協調並支援安全事件回應的組織。它通常負責：

- 接收事件通報。
- 評估事件嚴重性。
- 協調資訊分發給相關人員。
- 協助 containment、eradication、recovery。
- 提供事後改善建議。

CIRT 通常不負責制定整個 information security policy，那是治理與管理層面的工作。

Incident response 常見階段：

- Preparation：建立流程、工具、角色與通報管道。
- Detection / Recognition：發現事件。
- Evaluation / Analysis：判斷範圍、嚴重性與 root cause。
- Containment：限制擴散。
- Eradication：移除原因。
- Recovery：恢復服務。
- Lessons learned：事後檢討與改善。

正在處理 incident 時，system imaging 可以進行，因為它有助於保存證據與分析。root cause analysis 通常屬於 evaluation / analysis 階段。

## 14. Computer Crime 與內部威脅

Topic 4 很強調內部人員風險。員工常是 computer crime loss 的主要來源，因為他們有合法存取、熟悉流程，且可能處於 trusted position。

典型 computer fraudster 常是有信任職位的人，而不一定是外部高技術攻擊者。

Disgruntled employees 常被視為高威脅類型，因為他們同時具備內部知識、存取能力與動機。

讓 computer crimes 容易發生的最大因素常是 victim carelessness，例如缺乏審查、弱控制、未注意異常、過度信任。

MOM 用來理解犯罪動機：

- Motive：動機。
- Opportunity：機會。
- Means：手段。

Enticement 是刻意放置看似可利用的弱點或誘因，用來偵測或迷惑入侵者。Entrapment 則可能涉及誘使某人犯罪，法律風險較高。

## 15. 法律責任、Due Care 與 Culpable Negligence

Due care 是合理謹慎行事。管理層若沒有對 computing resources 採取合理保護，可能因 culpable negligence 對損失負責。

法律責任常和「控制成本 C」與「預期損失 L」比較有關。如果保護成本低於可避免損失，卻沒有實施合理控制，可能被認為未盡合理注意。

可以簡化理解：

- 有可預見風險。
- 有合理控制可降低風險。
- 控制成本合理。
- 組織卻沒有採取行動。

這種情況容易形成法律責任。

## 16. Evidence 與 Chain of Custody

Computer evidence 要能被法庭接受，至少要 relevant。它也應可信、完整、依法取得，並能證明未被竄改。

Chain of custody 是證據從收集、保管、移交、分析到呈堂的完整紀錄。它的目的，是讓證據能被 court admissible。

Chain of custody 應記錄：

- who collected evidence。
- what was collected。
- when it was collected。
- where it was stored。
- how it was handled。
- who accessed or transferred it。
- whether it was changed or preserved。

保存 chain of custody 最重要的是可驗證文件，清楚說明 who、what、when、where、how。

如果員工涉案，收集 physical evidence 除了 Legal Department，也要協調 Human Resources，因為牽涉雇傭關係、紀律流程與公司政策。

分析時應盡可能分析 compromised resource 的 replica，而不是原始媒體，以避免無意中改變證據。

Investigator notebook 在美國法律情境中，可用於作證時 refresh investigator's memory，但不能取代證人出庭。

## 17. Evidence 類型與法律規則

Direct evidence 是透過口頭證詞直接證明或否定特定行為，基於證人的五感觀察。

Circumstantial evidence 是從其他中間事實推論出某件事。

Corroborative evidence 是補強主要證據的支持性證據，不能單獨承擔全部證明。

Secondary evidence 是原始證據的副本，例如影本或複製資料。

Computer-generated evidence 常被視為 second hand / hearsay 類型，因此需要證明系統可靠、紀錄是在正常業務過程產生、且資料未被改動。

Business records exception / business exemption rule 關心的是紀錄是否在正常業務過程中、接近事件發生時產生，且能證明資料未被竄改。由 senior management 收集本身不會讓紀錄更可採。

Best evidence rule 關心是否提供原始或最佳可得證據。Exclusionary rule 則關心證據取得是否合法。

Exigent circumstances doctrine 是在有 probable cause 且證據即將被破壞時，允許在沒有 warrant in hand 的情況下進行搜查的例外。

## 18. BCP/DRP 與控制分類

BCP/DRP 會同時用到多種控制：

- Preventive controls：降低災難或事件發生機率，例如安全政策、維護、環境控制。
- Detective controls：發現事件，例如 monitoring、alarm、incident reporting。
- Corrective controls：事件後恢復，例如 backup restore、alternate site activation、repair。

Disaster recovery controls 多數偏 corrective，因為它們處理事件後恢復。

Contingency planning controls 評估時常看：

- backup storage site 是否與 primary site 距離足夠。
- alternate site 是否不受同一災難影響。
- backups location 是否清楚。
- plan 是否被測試與維護。
- recovery personnel 是否有離線副本。

「所有地點共用一份單一計畫」不是好做法。不同 location 可能有不同風險、流程、聯絡人與資源。

## 19. 概念速查：BCP/DRP

| 概念 | 核心意思 |
| --- | --- |
| BCP | 維持 critical business functions |
| DRP | 恢復 IT 與處理能力 |
| BIA | 評估中斷造成的業務影響 |
| Dependency | BIA 中業務流程互相依賴的重點 |
| RTO | 多久內要恢復 |
| RPO | 最多可失去多少資料 |
| Hot site | 設備完整、最快、最貴 |
| Warm site | 部分設備 |
| Cold site | 最便宜、恢復慢、最難測試 |
| Reciprocal agreement | 難 enforce，資源不保證 |
| Parallel test | 不停正式系統並行測試 |
| Full interruption test | 最完整、最高風險，應在 parallel test 成功後 |
| Remote journaling | 即時傳送交易日誌到 alternate site |
| Electronic vaulting | 大量資料傳到遠端備份設施 |

## 20. 概念速查：風險與法律

| 概念 | 核心意思 |
| --- | --- |
| Threat | 可能造成損害的事件或主體 |
| Vulnerability | 可被利用的弱點 |
| Risk | 威脅利用弱點造成損害的可能性與影響 |
| Exposure Factor | 單次事件損失比例 |
| SLE | Asset Value x EF |
| ALE | SLE x ARO |
| Residual risk | 控制後仍留下的風險 |
| Risk acceptance | 接受風險，政策例外需要它 |
| Qualitative analysis | 排序與優先級，較快但不精確 |
| Quantitative analysis | 金額與頻率，利於成本效益 |
| Chain of custody | 證據處理完整紀錄 |
| Relevant evidence | 法庭可採性的基本條件 |
| Direct evidence | 直接證明特定行為 |
| Circumstantial evidence | 從其他事實推論 |
| Corroborative evidence | 支持主要證據 |
| Secondary evidence | 證據副本 |
| Due care | 合理謹慎保護資源 |
| Culpable negligence | 未盡合理注意導致責任 |
