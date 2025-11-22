> [!info] LINK & TAG
> [TryHackMe | Public Key Cryptography Basics](https://tryhackme.com/room/publickeycrypto)
> 
> #cryptography 
### SSH 連線需要兩種認證：

[[#1. Authenticating the Server]]
    → 確保你連的是正版主機（避免 MITM）
    
[[#2. Authenticating the Client]]
### 1. Authenticating the Server

- SSH 會檢查伺服器公開金鑰 fingerprint
- 第一次連線 → 因為不知道 fingerprint → 跳警告
```sh
The authenticity of host 'xxx.xxx.xxx.xxx' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxx
Are you sure you want to continue connecting?

```
- 你要確認伺服器身分（避免 MITM(Man-in-the-Middle Atttack)）
- 按 yes 後 → SSH 會把這把金鑰記住
```sh
~/.ssh/known_hosts
```
- 未來伺服器「金鑰一致」→ SSH 靜默連線
- 若金鑰變了 → SSH 會警告你可能被中間人攻擊

---

### 2. Authenticating the Client

→ SSH key pair  (帳號 + 密碼 不安全)
→ 公鑰放伺服器、私鑰留在自己電腦  
→ 用 cryptographic challenge 驗證你是否持有私鑰
-  當你連線時，伺服器做一件事：出題目
	伺服器會：
	1. 隨機產生一段字串（challenge）
	2. 用「你的公鑰」加密這段字串
	3. 把密文丟給你
		

- 只有真正擁有「私鑰」的人才能解密
	你的電腦：
	1. 用你的私鑰把密文解開
	2. 得到原本的字串
	3. 把解密後的結果回傳給伺服器（通常會再簽名）

####  generate key pairs
```sh
root@TryHackMe# man ssh-keygen 
[...] 
-t dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa 
Specifies the type of key to create. The possible values are “dsa”, “ecdsa”, “ecdsa-sk”, “ed25519”, “ed25519-sk”, or “rsa”. 
[...]
```
```sh
root@TryHackMe# ssh-keygen -t ed25519 
Generating public/private ed25519 key pair. 
Enter file in which to save the key (/home/strategos/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/strategos/.ssh/id_ed25519 
Your public key has been saved in /home/strategos/.ssh/id_ed25519.pub 
The key fingerprint is: SHA256:4S4DQvRfp52UuNwg++nTcWlnITEJTbMcCU0N8UYC1do strategos@g5000 The key's random art image is: 
+--[ED25519 256]--+ 
| . +XXB. | 
| . . . oBBo | 
| . . . = + o=o | 
| . . * X .o.E | 
| . . o S + o . | 
| . . o .. + o | 
| o +. + o | 
| +. . | 
| .. | 
+----[SHA256]-----+
```
- public key `id_ed25519.pub`
- private key `id_ed25519`

---

### Get a “Better Shell”
#### 1. 反向 Shell (Reverse Shell) 的限制

當攻擊者成功利用系統漏洞在目標機器上獲得一個 Shell 會話時，這通常是一個 **非穩定** 或 **不穩定** 的 Shell。

- **問題：** 這種 Shell 經常缺乏一些基本功能，例如：
    
    - 無法正常使用 $\mathbf{Ctrl+C}$ 組合鍵來終止當前運行的程序。
        
    - 缺少 **Tab 鍵自動補全** (Tab Completion) 功能。
        
    - 在處理文字或全螢幕應用程式時可能會出現顯示問題。
        

#### 2. SSH 密鑰 (SSH Keys) 的優勢

**SSH (Secure Shell)** 是一種加密的網路協議，用於在不安全的網路上安全地運行網路服務，它提供了比不穩定的 Reverse Shell **更優越、更穩定** 的 Shell 體驗。

- 「升級」Shell 的方法： 1. 攻擊者在自己的機器上生成一對 SSH 密鑰（公鑰 和 私鑰）。
    
    2. 將攻擊者機器上的 公鑰 寫入目標機器上某個用戶（例如，已破解密碼或權限提升獲得的用戶）的 .ssh/authorized_keys 文件中。
    
    3. 這樣一來，攻擊者就可以使用其 私鑰 直接通過 SSH 登錄 到目標機器上，無需密碼。
    
- **優點：** * 獲得一個 **完全穩定** 的 Shell。
    
    - 具有 **Tab 鍵自動補全** 功能。
        
    - $\mathbf{Ctrl+C}$ 等控制字符可正常運作。
        
    - 整個連線是 **加密** 的，更加安全。