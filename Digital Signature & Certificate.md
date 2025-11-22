> [!info] LINK & TAG
> [TryHackMe | Public Key Cryptography Basics](https://tryhackme.com/room/publickeycrypto)
> 
> #cryptography 


### Digital signarure
![[Pasted image 20251122102530.png]]
### Certificate
#### 1. 憑證的目的

- 用來 **證明伺服器的身份**（像 tryhackme.com 是真的那個）。
    
- 讓瀏覽器知道「你連到的網站不是假冒的」。
    

---

#### 2. 誰來保證憑證是真的？

**憑證有一條「信任鏈」（Chain of Trust）：**

1. **Root CA（根憑證機構）**
    
    - 已內建在你的作業系統 / 瀏覽器中
        
    - 你自動信任它們
        
2. **中繼 CA（Intermediate CA）**
    
    - 被 Root CA 信任
        
    - 用來簽發網站憑證
        
3. **網站（Server Certificate）**
    
    - 你的網站憑證由中繼 CA簽名  
        → 因此被瀏覽器信任
        

**瀏覽器信任網站，是因為信任上層簽它的 CA。**

---

#### 3. 為什麼需要？

- HTTPS 需要憑證才能建立加密連線
    
- 沒憑證 → 瀏覽器會顯示「不安全」、「可能是冒充網站」
    

---

#### 4. 憑證怎麼取得？

- 傳統方式：向 CA 付費購買（每年費用）
    
- 現代方式：使用 **Let’s Encrypt 免費取得**（自動更新）
    

---

#### 5. 一句話總結

**TLS/HTTPS 的安全性依靠證書與可信任 CA 所建立的「信任鏈」，用來驗證你連的伺服器是真的。**