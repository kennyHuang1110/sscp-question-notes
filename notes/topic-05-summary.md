# Topic 5 知識概念整理：Cryptography、PKI、Kerberos、IPsec 與加密攻擊

Topic 5 的核心是 cryptography。題目常考的是「某個機制提供哪一種安全性」、「某個演算法屬於哪一類」、「PKI 怎麼建立信任」、「IPsec/IKE 由哪些元件組成」，以及各種密碼分析攻擊的差異。

這章不要只背演算法名稱，要把功能對起來：

- confidentiality：加密，避免未授權者讀取內容。
- integrity：hash、MAC、digital signature 可協助驗證內容未被改。
- authentication：確認身分或訊息來源。
- nonrepudiation：通常靠 digital signature，讓發送者事後難以否認。
- key management：加密最難的部分常不是演算法，而是金鑰產生、配送、保存、撤銷與更新。

## 1. Cryptology、Cryptography、Cryptanalysis

Cryptology 是整個密碼學領域，包含：

- Cryptography：設計與使用加密、雜湊、簽章等保護機制。
- Cryptanalysis：分析、破解或繞過密碼系統。

題目常把 cryptography、cryptology、crypto 混在一起。看到「研究密碼系統整體」通常是 cryptology；看到「設計加密方法」是 cryptography；看到「破解或分析密碼」是 cryptanalysis。

## 2. 對稱式與非對稱式加密

Symmetric key cryptography 又稱 secret key cryptography。加密與解密使用同一把金鑰，速度快，適合大量資料加密，但金鑰配送與管理困難。

常見 symmetric algorithms：

- DES。
- 3DES。
- AES / Rijndael。
- IDEA。
- Blowfish。
- Twofish。
- RC4。
- RC5。
- Skipjack。

Asymmetric key cryptography 又稱 public key cryptography。使用 public key 與 private key 成對運作，通常較慢，但可解決金鑰交換、簽章與信任問題。

常見 asymmetric algorithms：

- RSA。
- Diffie-Hellman，主要用於 key agreement。
- ElGamal。
- ECC。

考點：

- RSA 是 asymmetric。
- DES 是 symmetric / secret key。
- Kerberos 依賴 symmetric / secret key，不是 public key。
- ECC 的優勢是可以用較短 key length 達到相近安全強度。
- 實務上常用 hybrid cryptography：用 asymmetric 保護 session key，再用 symmetric 加密大量資料。

## 3. Confidentiality、Integrity、Authentication、Nonrepudiation

加密本身主要提供 confidentiality。它讓未持有正確金鑰的人看不懂資料。

Hash 主要提供 integrity 檢查。輸入任意長度資料，輸出固定長度 message digest。好的 hash 應該是 one-way，且難以找到 collision。

MAC, Message Authentication Code，通常用共享 secret key 搭配訊息產生驗證碼，可提供 message integrity 與 message authentication，但通常不提供 nonrepudiation，因為雙方都知道同一把 secret key。

Digital signature 可提供：

- authentication。
- integrity。
- nonrepudiation。

簽章的基本概念：

- 發送者用自己的 private key 簽署 message digest。
- 接收者用發送者的 public key 驗證簽章。
- 若驗證成功，代表訊息來源與內容完整性可被驗證。

要保密寄給某人：

- 用 recipient 的 public key 加密。
- recipient 用自己的 private key 解密。

要證明是自己送的：

- 用自己的 private key 簽章。
- 對方用自己的 public key 驗證。

## 4. Hash 與 Message Digest

Hash function 把 variable-length input 轉成 fixed-length digest。題庫常用的敘述是「a fixed length message digest from a variable length input message」。

常見 hash：

- MD5：128-bit digest，已不適合新系統安全用途。
- SHA-1：160-bit digest，題庫仍會考，但現代實務已不建議用於碰撞安全需求。
- SHA family：較新的 SHA-2 / SHA-3 在實務上更常見。

Hash 用途：

- 檢查檔案完整性。
- password file 中保存 password hash，而不是明文密碼。
- digital signature 通常簽 message digest，而不是直接簽整份訊息。

常見攻擊：

- Birthday attack：利用 collision 機率，攻擊 hash 或簽章系統。
- Dictionary / brute force：常用於 password hash 猜測。

注意：hash 不是 encryption。hash 通常不可逆；encryption 是可用 key 解密還原。

## 5. Block Cipher、Stream Cipher 與 Modes

Block cipher 一次處理固定大小區塊。DES、3DES、AES、IDEA、Blowfish、Twofish 都屬於 block cipher。

Stream cipher 產生 keystream，然後通常與 plaintext 做 XOR。RC4 是常見 stream cipher。Stream cipher 常用於連續資料流，但 keystream 與 IV/nonce 重複會非常危險。

XOR, Exclusive-OR，是許多密碼系統中的基本運算。若搭配 truly random、只使用一次、與訊息等長的 key，就形成 one-time pad。

常見 block cipher modes：

- ECB, Electronic Codebook：相同 plaintext block 加上相同 key 會產生相同 ciphertext block，因此會洩漏模式，不適合保護有重複結構的資料。
- CBC, Cipher Block Chaining：前一個 ciphertext block 會影響下一個 block，需要 IV。
- CFB, Cipher Feedback：可讓 block cipher 類似 stream cipher 使用。
- OFB, Output Feedback：產生 keystream 後與 plaintext 結合。

IV, Initialization Vector，用來避免相同 plaintext/key 每次產生完全相同 ciphertext。IV 不一定需要保密，但通常必須不可預測或不可重複，視模式而定。

## 6. DES、3DES、AES 與常見演算法考點

DES：

- Derived from Lucifer。
- symmetric / secret key algorithm。
- 64-bit block。
- key 總長 64 bits，其中有效 key length 是 56 bits，另 8 bits 常作 parity。
- 現代實務已不安全，但題庫常考它的歷史與數字。

3DES：

- 對 DES 做三次運算。
- 常見強度最高的形式是 DES-EDE3，使用三把不同 key。
- 題庫可能問 DES-EEE1 使用一把 key。

AES：

- 由 Rijndael 選為 AES。
- block size 是 128 bits。
- key size 可為 128、192、256 bits。
- 題目看到 256 bits 常是在問 AES 最大 key size。

IDEA：

- block size 64 bits。
- key size 128 bits。

Blowfish：

- symmetric block cipher。
- key length 可變，常見題庫會提到 maximum 448 bits。

Twofish：

- Blowfish 後繼者之一。
- block size 128 bits。
- key size 可到 256 bits。

RC4：

- stream cipher。
- 曾用於 WEP/SSL 等場景，但現代實務已不建議使用。

RC5：

- block cipher。
- 特色是參數可變，例如 word size、rounds、key length。

Skipjack：

- symmetric block cipher。
- key size 80 bits。
- 與 Clipper chip 歷史相關。

## 7. One-Time Pad

One-time pad, OTP，在理論上可以 unbreakable，但條件非常嚴格：

- key 必須 truly random。
- key 必須與 message 一樣長或更長。
- key 只能使用一次。
- key 必須安全配送與保管。

只要 key 重複使用、不是隨機、或被攔截，就不再具備理論上的不可破解性。

題庫常考：

- unbreakable encryption method：one-time pad。
- OTP 常使用 XOR 將 plaintext 與 random key stream 結合。

## 8. Public Key Infrastructure, PKI

PKI 的目的不是讓通道自動安全，而是建立 public key 與某個身分之間的信任關係。也就是讓 sender 可以確認「這把 public key 確實屬於預期的 recipient」。

PKI 元件：

- CA, Certification Authority：簽發與管理 certificates。
- RA, Registration Authority：協助驗證申請者身分。
- Certificate：把 subject identity 與 public key 綁在一起。
- CRL, Certificate Revocation List：列出被撤銷的憑證。
- OCSP：即時查詢憑證狀態的機制。
- CPS, Certification Practice Statement：描述 CA 實際作業方式。

X.509 certificate 常見欄位：

- subject distinguished name。
- subject public key。
- issuer。
- serial number。
- validity period。
- signature algorithm。
- issuer signature。

憑證是 electronic credential，用來證明被簽發者是其所宣稱的身分。

驗證 certificate chain 時，要一路驗到 trusted root certificate，並檢查：

- 每張憑證是否由上一層正確簽署。
- 憑證是否在有效期間內。
- 憑證是否被撤銷。
- 憑證用途是否符合使用情境。

## 9. PKI Trust、Cross-Certification 與 Root CA

PKI 的信任通常來自 root CA。若 root CA certificate 不被信任，下面的憑證鏈也不會被信任。

Root CA key compromise 是非常嚴重的事件，因為整個信任根會被動搖。重新建立信任時，需要把新的 root CA certificate 安全、真實地配送給所有 PKI participants。

Cross-certification 是不同 PKI 之間建立信任的方式。題目問「creating trust between different PKIs」通常選 cross-certification。

Bridge CA 也可用來連接多個 PKI domain，降低每個 CA 彼此交叉簽署的複雜度。

Revocation latency 是撤銷請求到撤銷資訊被發布之間的時間差。這段時間內，已被撤銷的憑證仍可能被某些系統視為有效。

憑證撤銷相關：

- CRL：憑證撤銷清單。
- ARL, Authority Revocation List：撤銷 CA certificate 的清單。
- OCSP：線上查詢單一憑證狀態。

CA revocation mailing list 不是正式 PKI revocation mechanism。

## 10. Digital Certificates、Smart Card 與 Private Key 保護

Public key certificate 是 PKI 的核心物件。它把 subject、public key 與 CA signature 綁在一起。

Smart card 的主要價值是提供 tamper-resistant、mobile storage 與 private key operation。private key 儘量不離開卡片或硬體裝置，可以降低被竊風險。

Private key 保護重點：

- 不可共享。
- 應加密保存。
- 可用 smart card、HSM 或 TPM 保護。
- 必須有備份與恢復策略，但簽章用 private key 是否 escrow 要非常小心。

Key escrow 是把解密金鑰交由第三方或受控機制保管，以便特殊情況下恢復資料。它可支援資料復原，但會帶來信任、濫用與 nonrepudiation 風險。

Split knowledge 是把敏感資訊拆成多份，單一人無法得知完整秘密。常與 dual control 搭配，用於高敏感金鑰管理。

## 11. Kerberos

Kerberos 是第三方認證系統，核心依賴 symmetric key cryptography。它不是 public key cryptography，也不是密碼明文交換。

主要元件：

- KDC, Key Distribution Center。
- AS, Authentication Server。
- TGS, Ticket Granting Server。
- TGT, Ticket Granting Ticket。
- Service ticket。

基本流程：

1. Client 向 AS 驗證身分。
2. AS 發 TGT。
3. Client 用 TGT 向 TGS 要某服務的 ticket。
4. Client 帶 service ticket 存取 server。

Kerberos 優點：

- 使用 tickets，避免反覆傳送密碼。
- 可提供 mutual authentication。
- 適合集中式身份驗證環境。

Kerberos 風險：

- KDC 是關鍵信任點，故障或被攻破影響很大。
- 時間同步很重要，票證通常有有效期限。
- password guess / offline attack 仍可能是風險。

## 12. SSL/TLS、SET、WTLS

SSL/TLS 使用 public key、digital certificates 與 symmetric session key 的 hybrid scheme。非對稱加密通常用於驗證與協商 key，真正的大量資料傳輸用 symmetric session key。

SSL/TLS 主要用途：

- server authentication。
- confidentiality。
- integrity。
- 可選 client authentication。

題庫常見陷阱：SSL 的主要常見用途不是一定要 authenticate client to server，而是讓 client 驗證 server，並建立安全通道。

SET, Secure Electronic Transaction，是早期針對信用卡與電子付款設計的安全交易協定。題庫看到 credit card / e-commerce payment 可能選 SET。

WTLS 是 Wireless TLS，用於早期 WAP 無線環境。

## 13. IPsec、AH、ESP、IKE

IPsec 是在 IP layer 提供安全性的協定組。常用於 VPN。

IPsec 主要協定：

- AH, Authentication Header：提供 integrity、authentication、anti-replay，但不提供 confidentiality。
- ESP, Encapsulating Security Payload：可提供 confidentiality，也可提供 integrity、authentication。

題庫問「Integrity, Authentication and Nonrepudiation」若選項中有 AH，常把 AH 作為 authentication/integrity 的答案，但嚴格說 AH 不提供 nonrepudiation。

IPsec modes：

- Transport mode：保護 payload，原 IP header 保留。常用於 host-to-host。
- Tunnel mode：整個原始 IP packet 被封裝。常用於 site-to-site VPN。

Link encryption vs end-to-end encryption：

- Link encryption：每一段 link 分別加密，資料在中間節點可能被解密再加密；若中間節點被攻破，資料可能曝光。
- End-to-end encryption：從來源到目的地一路保持加密，中間節點不應看到明文。

IKE, Internet Key Exchange：

- 用於 IPsec 的 peer authentication 與 key exchange。
- IKE 建立 Security Association。
- Phase 1 建立安全管理通道。
- Phase 2 協商 IPsec SA。

相關名詞：

- ISAKMP：定義 SA 管理與協商框架。
- Oakley：key exchange protocol，常與 IKE 相關。
- SKIP：Simple Key-management for Internet Protocols，是另一種 key management 機制。

## 14. VPN 與隧道協定

常見 VPN / tunneling 協定：

- IPsec。
- L2TP。
- PPTP。
- L2F。

題庫常把 IPsec 與 L2TP 配在一起。L2TP 本身不提供強加密，通常搭配 IPsec。

CHAP 是 challenge-handshake authentication protocol，比傳送明文密碼安全。它使用 challenge/response，避免密碼直接在網路上傳送。

## 15. Cryptographic Attacks

常見密碼分析攻擊：

- Ciphertext-only attack：攻擊者只有 ciphertext。
- Known-plaintext attack：攻擊者有部分 plaintext 與對應 ciphertext。
- Chosen-plaintext attack：攻擊者可以選 plaintext 並取得 ciphertext。
- Chosen-ciphertext attack：攻擊者可以選 ciphertext 並觀察解密結果。
- Birthday attack：找 hash collision。
- Brute-force attack：嘗試所有可能 key。
- Frequency analysis：分析字母或模式出現頻率，常用於古典密碼。

攻擊能力通常由弱到強大致是：

1. Ciphertext-only。
2. Known-plaintext。
3. Chosen-plaintext。
4. Chosen-ciphertext。

Known plaintext attack 的典型描述是：攻擊者同時擁有 plaintext 與 associated ciphertext。

## 16. 古典密碼與隱寫術

Caesar cipher 是替換式密碼，ROT13 是 Caesar 類型的簡單變體，將字母位移 13 位。

Polyalphabetic cipher 使用多個替換字母表，比單一 Caesar substitution 更難以頻率分析。

Cryptography 是把內容變得不可讀；steganography 是把訊息藏起來，讓人不知道有訊息存在。

Steganography 常見例子：

- 把訊息藏在圖片、音訊或文字中。
- 每隔固定字數取一個字形成真正訊息。

Digital watermarking 是在數位媒體中加入可辨識標記，用於版權、來源追蹤或偵測未授權揭露。

題庫問「detecting fraudulent disclosure」或追蹤資料外洩來源，可能是 watermarking。

## 17. Key Management

加密控制的成敗高度依賴 key management。題庫常見正解是「data encryption requires careful key management」。

Key management 包含：

- key generation。
- key distribution。
- key storage。
- key rotation。
- key destruction。
- key recovery。
- key revocation。

Session key 是短期使用的 symmetric key，通常用於單次連線或一段會話。Hybrid encryption 常見做法是：

- 用 symmetric session key 加密資料。
- 用 recipient public key 加密 session key。
- recipient 用 private key 解出 session key，再解密資料。

Key agreement 是雙方協商共同 secret 的過程。Diffie-Hellman 常用於 key agreement。

Key escrow 可恢復加密資料，但不應隨便用在 digital signature private key，否則會削弱 nonrepudiation。

## 18. 題庫常見演算法速記表

| 名稱 | 類型 | 常見考點 |
| --- | --- | --- |
| DES | Symmetric block cipher | 64-bit block、56-bit effective key、derived from Lucifer |
| 3DES | Symmetric block cipher | DES-EDE3 常見三把 key |
| AES | Symmetric block cipher | Rijndael、128-bit block、128/192/256-bit key |
| RSA | Asymmetric | 加密、簽章，基於大質數分解困難 |
| ECC | Asymmetric | 較短 key 達到相近安全性 |
| Diffie-Hellman | Asymmetric key agreement | 協商 shared secret |
| RC4 | Stream cipher | keystream，現代不建議使用 |
| RC5 | Block cipher | 參數可變 |
| IDEA | Symmetric block cipher | 64-bit block、128-bit key |
| Blowfish | Symmetric block cipher | variable key length |
| Twofish | Symmetric block cipher | 128-bit block、up to 256-bit key |
| Skipjack | Symmetric block cipher | 80-bit key |
| SHA-1 | Hash | 160-bit digest，現代不建議新用 |
| MD5 | Hash | 128-bit digest，已不安全 |

## 19. 題庫常見觀念速記表

| 概念 | 重點 |
| --- | --- |
| Confidentiality | encryption |
| Integrity | hash、MAC、digital signature |
| Authentication | certificate、Kerberos、digital signature、MAC |
| Nonrepudiation | digital signature |
| Hash | variable input to fixed digest |
| MAC | message authentication with shared secret |
| Digital signature | private key sign, public key verify |
| PKI | binds public key to identity |
| CA | issues and signs certificates |
| RA | validates identity before certificate issuance |
| CRL | revoked certificates list |
| OCSP | online certificate status checking |
| Cross-certification | trust between PKIs |
| Kerberos | symmetric tickets, KDC, TGT |
| AH | integrity and authentication, no encryption |
| ESP | encryption plus optional integrity/authentication |
| IKE | IPsec peer authentication and key exchange |
| Link encryption | may expose plaintext at intermediate nodes |
| End-to-end encryption | encrypted from source to destination |
| One-time pad | theoretically unbreakable if used correctly |
| Steganography | hides existence of message |
| Watermarking | embeds ownership/source marker |

## 20. 容易選錯的點

- Kerberos 不是 public key system；它依賴 symmetric secret keys 與 tickets。
- Encryption 不等於 integrity。加密只讓內容不可讀，不必然能證明未被竄改。
- Hash 不等於 encryption。hash 不能解密還原原文。
- MAC 不等於 digital signature。MAC 使用 shared secret，不能提供強 nonrepudiation。
- 用 recipient public key 是為了 confidentiality；用 sender private key 是為了 signature。
- ECB 會洩漏重複模式，看到「same plaintext and key produce same ciphertext」就是 ECB。
- DES 的 key 常有 64-bit total key length 與 56-bit effective key 兩種說法。
- PKI 的重點是驗證 public key 與身分的綁定，不是保證所有傳輸通道都安全。
- Root CA 憑證更新需要安全配送到所有參與者，不能只在 CA 端改完就好。
- CRL 有撤銷資訊發布延遲，這就是 revocation latency。
- Link encryption 在中間節點可能解密，end-to-end encryption 才是一路到目的地都加密。
- One-time pad 只有在 key 真隨機、等長、一次性、安全保存時才是理論不可破。

