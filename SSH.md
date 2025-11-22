> [!info] LINK & TAG
> [TryHackMe | Public Key Cryptography Basics](https://tryhackme.com/room/publickeycrypto)
> 
> #cryptography 
### SSH 連線需要兩種認證：

1. **認證伺服器**  
    → 確保你連的是正版主機（避免 MITM）
    
2. **認證用戶端（你）**  
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

### 2 Authenticating the Client

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