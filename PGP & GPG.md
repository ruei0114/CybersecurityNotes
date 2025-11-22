> [!info] LINK & TAG
> [TryHackMe | Public Key Cryptography Basics](https://tryhackme.com/room/publickeycrypto)
> 
> #cryptography 


### PGP/GPG 密鑰與應用重點筆記

#### 1. 什麼是 PGP/GPG？

- **PGP (Pretty Good Privacy)：** 一種軟體，用於實現檔案加密、數位簽章等加密功能。
    
- **GnuPG 或 GPG：** **OpenPGP 標準** 的開源實作，常用於 Linux 環境。
    
- **主要用途：**
    
    - 保護電子郵件的 **機密性 (Confidentiality)**（加密）。
        
    - 透過 **數位簽章 (Digital Signing)** 確認郵件的 **完整性 (Integrity)**。
        

---

#### 2. GPG 密鑰生成過程 (`gpg --full-gen-key`)

|**步驟**|**選擇/輸入內容**|**目的**|
|---|---|---|
|**用途**|**(9) ECC (sign and encrypt) _default_** (簽章與加密)|決定密鑰對的功能：僅簽章，或兼具簽章與加密。|
|**演算法**|**(1) Curve 25519 _default_** (橢圓曲線)|選擇密鑰使用的加密演算法，例如 ECC 或 RSA。|
|**有效期限**|$\mathbf{0}$ (Key does not expire at all)|設定密鑰的有效期限，到期後需更新或延長。|
|**用戶 ID**|Real name: strategos, Email: strategos@tryhackme.thm|建立用戶 ID 來識別此密鑰，通常包含名稱和電子郵件。|
|**結果**|**主密鑰** (pub) 具備 $\mathbf{[SC]}$ (Sign, Certify) 功能；**子密鑰** (sub) 具備 $\mathbf{[E]}$ (Encrypt) 功能。||

---

#### 3. 密鑰安全性與使用

- **密碼短語 (Passphrase)：**
    
    - 私鑰可以像 **SSH 私鑰** 一樣使用密碼短語來保護（加密私鑰文件）。
        
    - 若密鑰受保護，攻擊者可使用 $\mathbf{gpg2john}$ 配合 $\mathbf{John\ the\ Ripper}$ 嘗試破解密碼短語。
        
- **公鑰與私鑰的交互：**
    
    - **公鑰 (Public Key)：** 可分享給聯繫人。用於 **加密** 傳給您的訊息。
        
    - **私鑰 (Private Key)：** 必須保密。用於 **解密** 聯繫人發給您的訊息。
        
- **備份與恢復：**
    
    - **重要性：** 必須在安全位置備份您的 GPG 密鑰對。
        
    - **匯入指令：** 使用 `gpg --import backup.key` 將備份的密鑰匯入新系統。
        
- **解密指令：** 要解密收到的加密文件，使用 `gpg --decrypt confidential_message.gpg`
    