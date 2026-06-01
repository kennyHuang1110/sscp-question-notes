# Topic 6 知識概念整理：網路協定、OSI/DoD、防火牆、VPN、媒體與網路攻擊

Topic 6 的核心是 communications and network security。題目很雜，但主軸可以分成幾組：OSI/DoD 模型、TCP/IP 協定、網路設備與拓撲、防火牆與 DMZ、VPN/遠端存取、傳輸媒體，以及常見網路攻擊。

這章的讀法是把「工作層級、功能、風險、常見數字」對起來。很多題目其實是在問某個技術位於哪一層，或某個控制能不能解決特定威脅。

## 1. OSI 模型

OSI 七層：

1. Physical：bit、電氣/光學/無線訊號、線材、repeater。
2. Data Link：frame、MAC address、switch/bridge、Ethernet、PPP、SLIP。
3. Network：packet/datagram、IP address、routing、router、ICMP、IPsec 常被歸在此層。
4. Transport：segment/datagram、TCP、UDP、port、端對端傳輸。
5. Session：建立、管理、終止 session，checkpoint、dialog control。
6. Presentation：資料格式、編碼、壓縮、加密呈現。
7. Application：使用者可見的網路服務，例如 HTTP、SMTP、DNS、FTP。

常見考點：

- Ethernet frame 是 Data Link 層單位。
- IP datagram / packet 是 Network 層單位。
- TCP/UDP 在 Transport layer。
- Router 在 Network layer。
- Bridge 在 Data Link layer。
- Repeater 在 Physical layer。
- Gateway 可做跨協定或高層轉換，常被視為 application-level 或多層設備。

## 2. DoD / TCP-IP 模型

DoD 模型常見四層：

- Application：應用服務，對應 OSI application/presentation/session。
- Host-to-host / Transport：TCP、UDP。
- Internet：IP、ICMP、IGMP、ARP/RARP 常放在這個附近考。
- Network Access / Link：Ethernet、PPP、SLIP、CSLIP、frame。

題庫常見說法：

- TCP/IP 是 Internet 的主要 protocol suite。
- Host-to-host layer 對應 TCP/UDP。
- Internet layer 對應 IP routing。
- Network Access layer 負責實體網路與 frame 傳輸。

## 3. TCP、UDP 與 IP Protocol Numbers

TCP：

- connection-oriented。
- reliable。
- sequence number。
- acknowledgment。
- flow control。
- 適合需要可靠傳輸的服務。

UDP：

- connectionless。
- overhead 較低。
- 速度較快。
- 不保證順序、送達或重傳。
- 適合語音、影音、查詢型服務或可容忍遺失的場景。

IP header protocol field 常見號碼：

- 1 = ICMP。
- 2 = IGMP。
- 6 = TCP。
- 17 = UDP。

TCP Wrappers 通常控制 TCP service，不適合控制 UDP service。Stateful firewall 對 TCP 連線狀態較容易追蹤。

## 4. 常見基礎協定

ARP, Address Resolution Protocol：

- 將 IP address 對應到 MAC address。
- 通常用 broadcast 詢問同一 LAN。

RARP, Reverse ARP：

- 由 MAC address 反查 IP address，屬於較舊機制。

ICMP：

- 用於錯誤回報與診斷，例如 ping。
- Smurf attack 使用 ICMP echo request + broadcast 放大。

IGMP：

- 管理 IP multicast group membership。

DNS：

- 將 domain name 解析成 IP address。
- DNS spoofing / cache poisoning 會造成導向錯誤主機。

SNMP：

- 用於網路管理，取得設備狀態、封包計數、routing table 等。
- 舊版 SNMP community string 若未保護，風險很高。

BootP / DHCP：

- BootP 可提供無磁碟或開機設備網路設定。
- DHCP 動態發放 IP 設定。
- Rogue DHCP server 可能錯誤配置 client，造成流量導向攻擊者或網路中斷。

SMTP：

- 傳送 email 的 host-to-host protocol。

IMAP4 / POP3：

- 讓使用者從 mail server 讀取郵件。

NNTP：

- Usenet/news protocol，常見 port 119。

RPC：

- Remote Procedure Call，讓程式呼叫遠端程序，但開放 RPC 服務常有安全風險。

NFS：

- Network File System，讓不同系統互通檔案系統。

TFTP：

- 簡單檔案傳輸，常用於網路設備 configuration transfer，缺乏強認證與保護。

## 5. 常見埠號與範圍

Port ranges：

- Well-known ports：0 到 1023。
- Registered ports：1024 到 49151。
- Dynamic/private ports：49152 到 65535。

常見埠：

- FTP control：21。
- SSH：22。
- Telnet：23。
- SMTP：25。
- DNS：53。
- HTTP：80。
- POP3：110。
- NNTP：119。
- IMAP4：143。
- HTTPS：443。

題庫常見：

- POP3 是 port 110。
- NNTP 是 port 119。
- HTTP 是 port 80。

## 6. 私有 IP、NAT 與 PAT

私有 IP 範圍：

- 10.0.0.0/8。
- 172.16.0.0/12，也就是 172.16.0.0 到 172.31.255.255。
- 192.168.0.0/16。

IPv4 address 是 32 bits。

Classful address 常見概念：

- Class A：第一個 octet 1 到 126，預設 /8。
- Class B：128 到 191，預設 /16。
- Class C：192 到 223，預設 /24。

NAT, Network Address Translation：

- 將內部 private IP 轉成外部 public IP。
- static translation：固定一對一轉換。
- dynamic translation：從 pool 動態分配。

PAT, Port Address Translation：

- 多個內部主機共用一個外部 IP，透過 port 區分連線。
- 題目看到「many-to-one NAT」或用 port 區分，就是 PAT。

## 7. 網路設備

Repeater：

- Physical layer。
- 重生訊號，延長距離。

Bridge：

- Data Link layer。
- 依 MAC address 轉發 frame。
- 可分割 collision domain。

Switch：

- 通常 Data Link layer，多埠 bridge。
- 根據 MAC table 轉發 frame。

Router：

- Network layer。
- 根據 IP address 與 routing table 決定路徑。

Gateway：

- 連接不同協定或不同架構網路。
- 可做高層轉換。

Modem：

- modulation/demodulation，讓 digital data 經由 analog line 傳輸。

Multiplexer：

- 將多個訊號合併到同一傳輸通道。

Bastion host：

- 暴露於不信任網路、經過 hardening 的主機。
- 常放在 DMZ 提供 proxy、mail relay、web service 等。

Multi-homed host：

- 有多張 network interfaces，分別連到不同網路。

## 8. Ethernet、Token Ring、FDDI 與拓撲

Ethernet：

- IEEE 802.3。
- 傳統共享媒體使用 CSMA/CD。
- Data Link 層單位是 Ethernet frame。

Token Ring：

- 使用 token passing。
- 題庫看到 token-passing LAN 常選 Token Ring。

FDDI：

- Fiber Distributed Data Interface。
- 使用光纖，常見雙環架構。

LAN topology：

- Bus。
- Star。
- Ring。
- Mesh。

Full mesh topology：

- 每個節點與其他節點都有連線。
- 冗餘高，但成本與複雜度高。

Broadcast：

- 一對多送給同一 broadcast domain 內所有節點。

Packet switching：

- 封包可經不同路徑抵達目的地。
- 題庫看到「packets can take multiple paths」就是 packet switching。

Statistical multiplexing：

- 根據流量需求動態共享通道，比固定分配更有效率。

## 9. 傳輸媒體與通訊方式

常見媒體：

- Twisted pair。
- Coaxial cable。
- Fiber optic。
- Microwave。
- Infrared。

Fiber optic：

- 由 glass fibers 傳送光訊號。
- 抗 EMI。
- 較難竊聽。
- 題庫常選為最抗 interception/eavesdropping 的媒體。

Twisted pair：

- UTP 常見於 Ethernet。
- Category rating 越高，通常效能越好、抗 interference/crosstalk 越強。
- Cat 5 / Cat 5e 常見 100 meters 距離限制。

Coaxial cable：

- 一個中央導體，外層絕緣後有同軸的屏蔽導體。
- Thicknet 是較粗的同軸乙太網路線。

Attenuation：

- 訊號隨距離衰減。

Asynchronous communication：

- bits 以不規則 timing pattern 傳輸，常用 start/stop bits。

Synchronous communication：

- 雙方以同步時脈或一致速率傳輸。

Infrared：

- 需要 direct line-of-sight，因此相對不容易穿牆外洩，但距離與方向受限。

## 10. DSL、WAN 與電信名詞

Local loop：

- 使用者端到電信公司 central office 的連線。

ADSL：

- Asymmetric DSL，下載與上傳速率不同。

HDSL：

- High-bit-rate DSL。

VDSL：

- Very-high-bit-rate DSL。

DS-1 / T1：

- 常見速率約 1.544 Mbps。

WAN connectivity：

- 連接地理距離較遠的網路。

LAN：

- Local Area Network，通常範圍較小、由單一組織控制。

## 11. 防火牆基本觀念

Firewall 用來控制 ingress/egress traffic，依安全策略允許或拒絕流量。它是現代連網環境的必要控制，但不是萬靈丹。

防火牆可以：

- 限制未授權連線。
- 控制服務與 port。
- 提供集中式 policy enforcement。
- 建立 DMZ。
- 記錄與監控流量。

防火牆不能保證：

- 阻止所有病毒。
- 阻止內部人員濫用。
- 修補錯誤配置。
- 阻止所有經允許通道進來的攻擊。

安全原則：

- default deny。
- 只允許必要服務。
- inbound 規則更嚴格。
- outbound 也要管制。
- 規則越複雜越容易 misconfiguration。

題庫常見陷阱：

- 「permit all inbound TCP connections」不是好策略。
- 「transparent allow all inbound/outbound services」不是安全設定。
- 防火牆不等於防毒。

## 12. 防火牆類型

Packet filtering firewall：

- 第一代防火牆。
- 通常在 Network/Transport layer。
- 根據 source/destination IP、port、protocol、direction 過濾。
- 速度快、overhead 低。
- 規則複雜時容易設定錯。

Stateful inspection firewall：

- 第二代防火牆。
- 追蹤 connection state。
- 動態允許回應流量。
- 題目看到「keeps track of the state of a connection」就是 stateful。

Application proxy firewall：

- Application layer / OSI layer 7。
- 代理 client 與 server 之間的應用流量。
- 可以理解應用協定並做更細控制。
- overhead 較高。

Circuit-level firewall：

- 常在 Session layer。
- 驗證 TCP handshake 或 session 是否合法。
- 不一定檢查應用內容。

Dynamic packet filtering：

- 根據連線狀態或需要動態開啟/關閉 port。

Screening router：

- 使用 packet filtering 規則的 router。

Content filtering：

- 根據內容、URL、檔案類型或關鍵字過濾。

## 13. DMZ、Screened Subnet 與 Defense in Depth

DMZ：

- 放置需要被外部存取的服務，例如 web、mail relay、DNS。
- 目標是避免外部直接進入內部 LAN。
- DMZ 中的主機因可被 Internet 存取，所以要假設可能被攻破。

Screened subnet：

- 使用兩層防火牆或多介面防火牆，把 DMZ 夾在外部與內部網路之間。
- 比單一 screening router 更符合 defense in depth。

Defense in depth：

- 使用多層控制，避免單一防線失效就導致整體失守。

遠端存取伺服器若要對內部系統提供 access，通常必須受 firewall/gateway 控制，並要求 authentication。把 Remote Access Server 放在 firewall 外側並強制合法使用者先通過 firewall authentication，是常見隔離做法。

## 14. VPN、隧道與遠端存取

VPN：

- 在不可信網路上建立加密通道。
- 題目看到「encrypted virtual private network」就是 VPN。

Tunneling：

- 將一種協定封裝在另一種協定內傳送。

IPsec：

- 在 IP network 上提供 confidentiality、integrity、authentication。
- 可使用 tunnel mode 或 transport mode。

Tunnel mode：

- 封裝整個原始 IP packet。
- 常用於 gateway-to-gateway 或 remote access VPN。

Transport mode：

- 保護 IP payload，原始 IP header 保留。
- 常用於 host-to-host。

L2TP：

- Layer 2 tunneling protocol，本身不提供加密，常搭配 IPsec。

PPTP：

- Point-to-Point Tunneling Protocol，較舊，安全性不足。
- 題庫陷阱：PPTP 不是 derived from L2TP。

L2F：

- Layer 2 Forwarding，Cisco 早期 tunneling protocol。

PPP：

- Point-to-Point Protocol，Data Link layer。
- 比 SLIP 功能完整，支援多種 network layer protocol 與 authentication。

SLIP / CSLIP：

- Serial Line Internet Protocol / Compressed SLIP。
- 較舊，功能少。

CHAP：

- Challenge Handshake Authentication Protocol。
- 使用 challenge/response，避免直接傳送密碼。

## 15. SSL/TLS、S-HTTP、WTLS

SSL/TLS：

- 常用於保護 web transactions。
- Server authentication 通常是 mandatory。
- Client authentication 是 optional。
- TLS Handshake Protocol 負責協商版本、cipher suite、金鑰交換與驗證。

S-HTTP：

- Secure HTTP，保護個別 HTTP message。
- 與 HTTPS/TLS 保護通道不同。

WTLS：

- Wireless TLS，用於早期 WAP 無線環境。

SSL/TLS 的 session key 通常在 client browser 端與 server 協商產生；資料傳輸多使用 symmetric encryption。

## 16. Link Encryption 與 End-to-End Encryption

Link encryption：

- 每一段 link 分別加密。
- 中間節點可能解密再重新加密。
- 若中間節點被攻破，明文可能曝光。
- 優點是對上層應用透明。

End-to-end encryption：

- 資料從來源到目的地一路保持加密。
- 中間節點不應看到明文。
- 題庫看到「Information stays encrypted from one end of its journey to the other」就是 end-to-end encryption。

## 17. 常見網路攻擊

Sniffer：

- 擷取網路流量供後續分析。
- 可被攻擊者用來監看明文密碼與資料。

Spoofing：

- 偽裝來源，例如 IP spoofing、email spoofing、DNS spoofing。

IP spoofing：

- 偽造來源 IP address。

TCP sequence number attack：

- 猜測或操控 TCP sequence number 以劫持連線。

SYN flood：

- 利用 TCP handshake 半開連線耗盡 server 資源。

Smurf attack：

- ICMP-based。
- 向 broadcast address 發送偽造來源的 ICMP echo request，讓大量主機回應受害者。

Fraggle attack：

- UDP-based，概念類似 Smurf，但使用 UDP echo/chargen 等服務。

Teardrop attack：

- 利用重疊或錯誤 fragment 造成目標重組封包時出錯。

Land attack：

- 封包 source 與 destination 設為同一受害者位址/port，造成系統異常。

Buffer overflow：

- 輸入超過 buffer 邊界，可能覆寫記憶體並執行任意程式碼。

Network address hijacking：

- 攻擊者劫持或冒用網路位址/路由，讓流量導向錯誤位置。

Unauthorized modem / rogue access：

- 常用來繞過組織安全政策與防火牆。

## 18. Communications Security 管理

Communications security 關心資訊傳輸過程中的 confidentiality、integrity、availability。

常見管理重點：

- 使用加密保護 sensitive transmission。
- 對 gateway / remote access 要求 authentication。
- 管理 network architecture。
- 定義可使用的協定與服務。
- 監控與記錄 network traffic。
- 避免不必要服務暴露。

Data classification 相關題目若問第一步，通常是先指定分類標準，也就是 criteria that determine how data is classified。

## 19. 題庫常見速記表

| 概念 | 重點 |
| --- | --- |
| TCP | connection-oriented、reliable |
| UDP | connectionless、fast |
| ICMP | IP protocol 1 |
| IGMP | IP protocol 2 |
| TCP | IP protocol 6 |
| UDP | IP protocol 17 |
| Ethernet | IEEE 802.3、CSMA/CD |
| Ethernet frame | Data Link layer |
| Router | Network layer |
| Bridge | Data Link layer |
| Repeater | Physical layer |
| ARP | IP to MAC |
| RARP | MAC to IP |
| DNS | domain name to IP |
| SNMP | network management |
| Fiber optic | 最抗竊聽、抗 EMI |
| Packet filtering firewall | first generation |
| Stateful firewall | tracks connection state |
| Proxy firewall | OSI layer 7 |
| Circuit firewall | session/circuit level |
| Screened subnet | DMZ 架構、防禦縱深 |
| CHAP | challenge/response authentication |
| PAT | many internal hosts share one public IP by ports |
| Smurf | ICMP-based |
| Fraggle | UDP-based |
| SYN flood | TCP half-open exhaustion |
| Teardrop | fragment reassembly attack |
| Land | source/destination same victim |

## 20. 容易選錯的點

- Bridge 是 Data Link，不是 Network。
- Router 是 Network，不是 Data Link。
- TCP 是 connection-oriented，UDP 不是。
- UDP 較快不代表較安全或可靠。
- Fiber optic 最難竊聽，但不是完全不能被 tap。
- Firewall 不能完全防病毒，也不能取代端點安全。
- Packet filtering 規則越複雜越容易設定錯。
- DMZ 主機因為面向 Internet，要假設可能被攻破。
- L2TP 本身不提供加密，常搭配 IPsec。
- PPTP 不是由 L2TP 衍生。
- Link encryption 中間節點可能看到明文；end-to-end 才是全程加密。
- S-HTTP 是保護 HTTP message；HTTPS/TLS 是保護通道。
- Rogue modem 或 unauthorized remote access 常是為了繞過 security policy。

