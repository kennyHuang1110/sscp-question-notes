# SSCP 考題單字整理

這份整理把題目中常見、容易混淆的英文詞彙放在一起。複習時建議先看「考題判斷重點」，再回頭連到各 Domain 的觀念筆記。

## Domain 1: Security Concepts and Practices

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| Confidentiality | 機密性 | 防止未授權揭露資料。看到 disclosure、privacy、encryption 常考這個。 |
| Integrity | 完整性 | 防止資料被未授權修改。看到 alteration、tampering、hash、digital signature。 |
| Availability | 可用性 | 確保系統和資料需要時可用。看到 downtime、DoS、redundancy。 |
| Identification | 識別 | 使用者宣稱自己是誰，例如 user ID。 |
| Authentication | 驗證 | 證明身份，例如 password、token、biometrics。 |
| Authorization | 授權 | 決定已驗證使用者能做什麼。 |
| Accountability | 可歸責性 | 能追蹤行為到特定主體，通常靠 audit logs。 |
| Least Privilege | 最小權限 | 只給完成工作所需的最低權限。 |
| Need-to-Know | 必要知悉 | 即使有 clearance，也只看工作需要的資料。 |
| Separation of Duties | 職責分離 | 避免單一人員完成敏感流程的所有步驟。 |
| Defense in Depth | 縱深防禦 | 多層控制疊加，單一控制失效仍有其他保護。 |
| Due Care | 應有注意 | 採取合理保護措施。 |
| Due Diligence | 盡職調查 | 持續確認風險與控制是否適當。 |
| Security Policy | 安全政策 | 高層級管理方向與要求。 |
| Standard | 標準 | 強制性的具體要求。 |
| Guideline | 指引 | 建議性做法，通常不強制。 |
| Procedure | 程序 | 逐步操作說明。 |
| Baseline | 基準 | 最低可接受安全設定。 |
| Deterrent Control | 嚇阻控制 | 讓攻擊者不想做，例如警告標誌、警衛。 |
| Preventive Control | 預防控制 | 阻止事件發生，例如門鎖、ACL。 |
| Detective Control | 偵測控制 | 發現事件，例如 IDS、監控、稽核。 |
| Corrective Control | 矯正控制 | 事件後修復，例如 restore backup。 |
| Compensating Control | 補償控制 | 原控制不可行時，用替代控制降低風險。 |

## Domain 2: Access Controls

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| DAC | 自主存取控制 | 資源 owner 決定誰能存取。 |
| MAC | 強制存取控制 | 依 classification、clearance、label 決定。 |
| RBAC | 角色型存取控制 | 權限綁在 role，不是直接綁個人。 |
| ABAC | 屬性型存取控制 | 根據 subject、object、environment 等屬性判斷。 |
| Access Control Matrix | 存取控制矩陣 | subject 對 object 的權限表。 |
| ACL | 存取控制清單 | 以 object 為中心列出誰有什麼權限。 |
| Capability Table | 能力表 | 以 subject 為中心列出可存取哪些 object。 |
| Sensitivity Label | 敏感標籤 | MAC 中通常含 classification 與 category/compartment。 |
| Clearance | 安全許可 | subject 的許可等級。 |
| Classification | 分類等級 | object 的敏感等級。 |
| Compartment | 分隔類別 | 用於 need-to-know 的類別限制。 |
| Incomparable Labels | 不可比較標籤 | 兩個 label 的 category set 互不完全包含。 |
| Kerberos | Kerberos 驗證 | Trusted third-party，使用 symmetric cryptography 與 tickets。 |
| KDC | 金鑰發佈中心 | Kerberos 核心，含 AS 與 TGS。 |
| Ticket Granting Ticket | 票證授權票 | Kerberos 用來向 TGS 取得服務票證。 |
| RADIUS | 遠端驗證服務 | AAA 協定，常見於 network access，使用 UDP。 |
| TACACS+ | AAA 協定 | Cisco 常見，分離 authentication、authorization、accounting。 |
| Diameter | AAA 協定 | RADIUS 的後繼，使用 TCP/SCTP。 |
| EAP | 可延伸驗證協定 | 常見於 802.1X、無線網路驗證。 |
| SSO | 單一登入 | 一次驗證後存取多個系統。 |
| Federation | 聯合身份 | 跨組織信任身份，例如 SAML、OIDC。 |
| MFA | 多因素驗證 | 必須來自不同 factor，不是兩個密碼。 |
| FAR | 錯誤接受率 | 生物辨識把錯的人當對的人。安全風險較高。 |
| FRR | 錯誤拒絕率 | 生物辨識把對的人拒絕。便利性問題。 |
| CER | 交叉錯誤率 | FAR 與 FRR 相等處，用於比較生物辨識準確度。 |

## Domain 3: Risk Identification, Monitoring and Analysis

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| Asset | 資產 | 有價值且需要保護的東西。 |
| Threat | 威脅 | 可能造成傷害的來源或事件。 |
| Vulnerability | 弱點 | 可被威脅利用的缺陷。 |
| Risk | 風險 | 威脅利用弱點造成損失的可能性與影響。 |
| Exposure | 暴露 | 弱點被利用後可能造成的損失狀態。 |
| Likelihood | 可能性 | 事件發生機率。 |
| Impact | 影響 | 事件造成的損害程度。 |
| Inherent Risk | 固有風險 | 未套用控制前的風險。 |
| Residual Risk | 殘餘風險 | 控制後仍存在的風險。 |
| Risk Appetite | 風險胃納 | 組織願意承受的風險程度。 |
| Risk Avoidance | 風險避免 | 停止造成風險的活動。 |
| Risk Mitigation | 風險降低 | 加控制來降低可能性或影響。 |
| Risk Transfer | 風險轉移 | 保險、外包、合約轉移財務責任。 |
| Risk Acceptance | 風險接受 | 明知風險仍決定承擔。 |
| ALE | 年化損失期望 | ALE = SLE x ARO。 |
| SLE | 單次損失期望 | SLE = Asset Value x Exposure Factor。 |
| ARO | 年發生率 | 一年預期發生幾次。 |
| Quantitative Analysis | 定量分析 | 用金額、數字衡量風險。 |
| Qualitative Analysis | 定性分析 | 用 high/medium/low 或矩陣評估。 |
| BIA | 業務影響分析 | 找出關鍵流程、影響、RTO、RPO。 |
| RTO | 復原時間目標 | 服務多久內要恢復。 |
| RPO | 復原點目標 | 可接受遺失多少時間範圍內的資料。 |
| MTD | 最大可容忍停機時間 | 業務可忍受的最長中斷時間。 |

## Domain 4: Incident Response and Recovery

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| Incident | 安全事件 | 已發生或疑似違反安全政策的事件。 |
| Event | 事件 | 可觀察到的狀態變化，不一定是資安事件。 |
| Preparation | 準備 | IR 計畫、工具、訓練、通訊名單。 |
| Detection | 偵測 | 發現可疑活動。 |
| Triage | 分流判斷 | 初步分類嚴重度與優先順序。 |
| Containment | 圍堵 | 限制事件擴散。短期先止血，長期再穩定。 |
| Eradication | 根除 | 移除惡意程式、後門、弱點來源。 |
| Recovery | 復原 | 恢復系統服務並監控是否復發。 |
| Lessons Learned | 經驗回顧 | 事件後改善流程與控制。 |
| Chain of Custody | 證物保管鏈 | 記錄證據由誰、何時、如何處理。 |
| Forensic Image | 鑑識映像 | 對媒體做 bit-level copy，保留證據。 |
| Volatile Data | 揮發性資料 | 斷電會消失，如 RAM、process、network connections。 |
| Evidence Integrity | 證據完整性 | 通常用 hash 證明未被修改。 |
| DRP | 災難復原計畫 | 著重 IT 系統復原。 |
| BCP | 營運持續計畫 | 著重維持關鍵業務運作。 |
| Hot Site | 熱備援站 | 幾乎可立即接手，成本最高。 |
| Warm Site | 溫備援站 | 有部分設備與設定，需要一些時間。 |
| Cold Site | 冷備援站 | 只有場地或基本設施，復原最慢。 |
| Full Backup | 完整備份 | 備份所有資料，還原最簡單。 |
| Incremental Backup | 增量備份 | 只備份上次任何備份後的變更；還原需 full + all incrementals。 |
| Differential Backup | 差異備份 | 備份上次 full 後的變更；還原需 full + latest differential。 |

## Domain 5: Cryptography

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| Symmetric Encryption | 對稱式加密 | 同一把 key 加解密，速度快，key distribution 是問題。 |
| Asymmetric Encryption | 非對稱式加密 | public/private key pair，較慢，可做 key exchange、signature。 |
| Hash | 雜湊 | 單向、固定長度，提供 integrity，不提供 confidentiality。 |
| Digital Signature | 數位簽章 | 私鑰簽、公開金鑰驗，提供 integrity、authentication、non-repudiation。 |
| Non-repudiation | 不可否認性 | 事後不能否認曾做過某行為。 |
| PKI | 公開金鑰基礎建設 | 管理 certificate、CA、CRL、OCSP。 |
| Certificate | 憑證 | 綁定 public key 與身份。 |
| CA | 憑證授權中心 | 簽發與信任憑證。 |
| RA | 註冊授權中心 | 驗證申請者身份，不簽發憑證。 |
| CRL | 憑證撤銷清單 | 查詢已撤銷憑證，可能不即時。 |
| OCSP | 線上憑證狀態協定 | 即時查詢憑證狀態。 |
| Key Escrow | 金鑰託管 | 第三方保存 key，常牽涉 recovery 或法遵。 |
| Diffie-Hellman | 金鑰交換 | 在不安全通道協商 shared secret。 |
| AES | 對稱式演算法 | 現代常用 block cipher。 |
| DES | 舊式對稱演算法 | 56-bit key，已不安全。 |
| 3DES | 三重 DES | 比 DES 強但較慢，逐漸淘汰。 |
| RSA | 非對稱演算法 | 可加密、簽章、key exchange。 |
| ECC | 橢圓曲線密碼 | 較短 key 達到相近安全強度。 |
| Stream Cipher | 串流密碼 | 逐位元或逐位元組加密。 |
| Block Cipher | 區塊密碼 | 固定區塊加密，需 mode of operation。 |
| Initialization Vector | 初始向量 | 增加隨機性，通常不需要保密但不可重用於特定模式。 |
| Salt | 鹽值 | 加到密碼雜湊，防止 rainbow table。 |
| Rainbow Table | 彩虹表 | 預先計算的 hash 查找表。 |

## Domain 6: Network and Communications Security

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| OSI Model | OSI 七層模型 | 題目常用 layer 判斷設備與協定。 |
| TCP | 傳輸控制協定 | Connection-oriented，可靠傳輸。 |
| UDP | 使用者資料包協定 | Connectionless，速度快但不保證可靠。 |
| TLS | 傳輸層安全 | 保護傳輸中的資料，HTTPS 使用。 |
| IPSec | IP 層安全 | 可用 AH/ESP，transport/tunnel mode。 |
| AH | Authentication Header | 提供完整性與驗證，不提供加密。 |
| ESP | Encapsulating Security Payload | 可提供加密、完整性與驗證。 |
| VPN | 虛擬私人網路 | 在不安全網路上建立安全通道。 |
| Firewall | 防火牆 | 依規則允許或阻擋流量。 |
| Packet Filtering | 封包過濾 | 依 IP、port、protocol 判斷。 |
| Stateful Inspection | 狀態檢查 | 追蹤連線狀態，比單純 packet filter 更聰明。 |
| Proxy Firewall | 代理防火牆 | 代表 client 與 server 溝通，可檢查應用層。 |
| IDS | 入侵偵測系統 | 偵測與告警，不主動阻擋為主。 |
| IPS | 入侵防禦系統 | 可主動阻擋或丟棄惡意流量。 |
| DMZ | 非軍事區 | 放置對外服務，隔離內網。 |
| NAT | 網路位址轉換 | 私有 IP 與公有 IP 轉換。 |
| VLAN | 虛擬區域網路 | Layer 2 邏輯分割。 |
| Segmentation | 網路分段 | 限制橫向移動與縮小影響範圍。 |
| Honeypot | 蜜罐 | 誘捕或觀察攻擊者行為。 |
| DoS | 阻斷服務 | 使服務不可用。 |
| DDoS | 分散式阻斷服務 | 多來源 DoS。 |
| Spoofing | 偽冒 | 偽造來源或身份。 |
| Sniffing | 竊聽 | 攔截網路流量。 |
| Replay Attack | 重放攻擊 | 重送已截獲的有效訊息。 |
| Man-in-the-Middle | 中間人攻擊 | 攔截並可能修改雙方通訊。 |

## Domain 7: Systems and Application Security

| Term | 中文理解 | 考題判斷重點 |
| --- | --- | --- |
| Secure SDLC | 安全軟體開發生命週期 | 把安全放進需求、設計、開發、測試、部署。 |
| Threat Modeling | 威脅建模 | 系統化找出可能攻擊路徑與控制。 |
| Input Validation | 輸入驗證 | 防止惡意或不合法輸入。 |
| SQL Injection | SQL 注入 | 未妥善處理輸入導致查詢被操控。 |
| XSS | 跨站腳本 | 攻擊者讓瀏覽器執行惡意腳本。 |
| CSRF | 跨站請求偽造 | 利用已登入狀態誘使使用者送出請求。 |
| Buffer Overflow | 緩衝區溢位 | 寫入超過記憶體邊界，可能執行任意程式碼。 |
| Patch Management | 修補管理 | 測試、部署、追蹤修補程式。 |
| Hardening | 強化 | 移除不必要服務、套用安全設定。 |
| Configuration Management | 組態管理 | 控制與追蹤系統設定變更。 |
| Change Management | 變更管理 | 正式審核、批准、紀錄變更。 |
| Malware | 惡意軟體 | 病毒、蠕蟲、木馬、勒索軟體等總稱。 |
| Virus | 病毒 | 需附著宿主檔案並靠執行傳播。 |
| Worm | 蠕蟲 | 可自行透過網路傳播。 |
| Trojan Horse | 木馬 | 偽裝成合法程式，誘使用戶執行。 |
| Rootkit | Rootkit | 隱藏攻擊者存在並維持高權限。 |
| Sandbox | 沙箱 | 隔離執行環境，限制影響範圍。 |
| Virtualization | 虛擬化 | 多個虛擬系統共享實體資源。 |
| Container | 容器 | 應用與依賴封裝，和主機共享 kernel。 |
| EDR | 端點偵測與回應 | 監控端點活動並支援調查與回應。 |

## 快速辨識口訣

| 題目線索 | 優先想到 |
| --- | --- |
| disclosure、privacy、encryption | Confidentiality |
| tampering、hash、signature | Integrity |
| downtime、DoS、redundancy | Availability |
| user ID | Identification |
| password、token、biometric | Authentication |
| role、permission、access right | Authorization |
| audit trail、log review | Accountability / Detection |
| owner decides access | DAC |
| label、classification、clearance | MAC |
| job function、role | RBAC |
| private key signs | Digital Signature |
| public key encrypts or verifies | Asymmetric Cryptography |
| full + all changes since full | Differential Backup |
| full + every later increment | Incremental Backup |
