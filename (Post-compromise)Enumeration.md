> [!info] LINK & TAG
> [TryHackMe | Linux Privilege Escalation](https://tryhackme.com/room/linprivesc)
> 
> #tag


## 0. 簡介

**枚舉 (Enumeration)** 是獲得系統存取權限後的第一步。無論是獲得 root 權限還是低權限帳號，枚舉在「後滲透階段 (Post-compromise)」與前期攻擊同樣重要，這有助於了解系統角色並尋找**提權 (Privilege Escalation)** 的機會。

---

## 1. 系統資訊 (System Information)

了解目標機器的作業系統版本、核心版本及其在網路中的角色。

|**命令**|**描述**|**用途**|
|---|---|---|
|`hostname`|顯示主機名稱|識別伺服器角色 (如 `SQL-PROD-01`)|
|`uname -a`|列出所有系統資訊|查核心版本，搜尋潛在的 **Kernel Exploit**|
|`cat /proc/version`|查看進程檔案系統資訊|了解編譯器版本 (如 GCC) 及核心詳細數據|
|`cat /etc/issue`|查看作業系統發行資訊|快速識別 OS 版本 (注意：此檔易被偽造)|
|`env`|顯示環境變數|查看 `PATH` 是否包含可利用的編譯器或腳本語言|

---

## 2. 使用者與權限 (Users & Privileges)

確認目前權限並列出系統上的其他使用者。

- **`whoami` / `id`**: 確認當前使用者與所屬群組。
    
    - 範例：`id [username]` 可查特定用戶。
        
- **`sudo -l`**: 列出當前使用者可以透過 `sudo` 執行的命令（尋找提權路徑的關鍵）。
    
- **`/etc/passwd`**: 讀取此檔案以發現系統使用者。
    
    - 技巧：`grep "home" /etc/passwd` 可過濾出具備家目錄的真實使用者。
        
- **`history`**: 查看歷史指令，可能包含敏感資訊、帳號密碼或系統操作軌跡。
    

---

## 3. 進程與檔案枚舉 (Processes & Files)

### 進程監控 (`ps` Command)

- `ps`：顯示當前 shell 的進程。
    
- `ps -A`：顯示所有執行中的進程。
    
- `ps axjf`：以**樹狀結構**顯示進程。
    
- **`ps aux`**：最常用的組合。
    
    - `a`: 所有使用者。
        
    - `u`: 顯示啟動者的名稱。
        
    - `x`: 顯示未連結至終端機的進程。
        

### 檔案枚舉 (`ls`)

- **建議指令**: `ls -la`
    
- **重要性**: `-a` 可以顯示隱藏檔案（如 `.ssh` 檔案、`.bash_history` 或機密設定檔 `secret.txt`）。
    

---

## 4. 網路資訊 (Networking)

判斷該機器是否可作為**跳板 (Pivoting)** 進一步攻擊內網。

- **`ifconfig` / `ip addr`**: 查看網路介面（如 `eth0`, `tun0`）。
    
- **`ip route`**: 查看網路路由。
    
- **`netstat` (常用參數組合)**:
    
    - `netstat -a`: 顯示所有連線。
        
    - `netstat -at` / `-au`: 分別列出 TCP / UDP。
        
    - `netstat -l`: 列出監聽中 (Listening) 的連接埠。
        
    - `netstat -tp`: 顯示連線對應的服務名稱與 **PID** (需 root 權限才能看完整)。
        
    - `netstat -ano`: 顯示所有通訊端、不解析名稱、顯示計時器。
        

---

## 5. 強大的 `find` 命令

用於搜尋敏感檔案、錯誤權限設定或提權工具。

### 常用搜尋範例

- **依名稱**: `find /home -name flag1.txt`
    
- **依類型**: `find / -type d -name config` (找目錄)
    
- **依權限**:
    
    - `find / -type f -perm 0777` (找所有人皆可讀寫執行的檔案)
        
    - **SUID 權限**: `find / -perm -u=s -type f 2>/dev/null` (尋找具備 SUID 位元的程式，提權必檢測)
        
- **依時間**:
    
    - `find / -mtime 10` (過去 10 天內修改)
        
    - `find / -amin -60` (過去 60 分鐘內存取)
        
- **依大小**: `find / -size +100M` (大於 100MB)
    

> [!TIP]
> 
> 技巧：錯誤處理
> 
> 執行 find 時常會因權限不足產生大量錯誤訊息，建議加上 2>/dev/null 將錯誤重定向到黑洞，保持輸出整潔。
> 
> 範例：find / -writable -type d 2>/dev/null

---

## 6. 其他必備工具

熟練以下命令將大幅提升枚舉效率：

- **`grep`**: 搜尋字串。
    
- **`locate`**: 快速定位檔案。
    
- **`cut` / `sort` / `uniq`**: 處理 `ps` 或 `/etc/passwd` 的輸出結果，製作成使用者名單。
    
