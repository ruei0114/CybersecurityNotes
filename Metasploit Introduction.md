> [!info] LINK & TAG
> [TryHackMe | Metasploit: Introduction](https://tryhackme.com/room/metasploitintro)
> [[Metasploit Exploitation]]
> #metasploit #exploitation 


## **1. 啟動 Metasploit**

```bash
msfconsole
```

啟動後提示字會變成：

```
msf6 >
```

Metasploit 本質上是 **Ruby Shell + 指令框架**，能執行部分 Linux 指令。

---

# **2. msfconsole 可以做什麼？**

## **支援的 Linux 指令**

可執行：`ls`, `ping`, `clear`, `pwd` …  
（注意：**不支援 output redirection，例如 `>` 或 `>>`**）

錯誤示例：

```bash
help > help.txt
[-] No such command
```

---

# **3. 常用指令總覽（重點）**

|指令|用途|
|---|---|
|`search <keyword>`|搜尋模組|
|`use <module>`|載入模組|
|`show options`|顯示模組所需參數|
|`set <option> <value>`|設定參數|
|`setg`|設定全域參數|
|`show payloads`|顯示可用的 payload|
|`run` / `exploit`|執行模組|
|`back`|回到主介面、離開模組|
|`info`|查看模組詳細資訊|
|`history`|查看指令紀錄|
|**Tab 補全**|超級重要，操作效率爆提升|

---

# **4. Module Context（模組上下文）**

Metasploit 有「**上下文 (context)**」概念：  
當你使用某個模組時，你的設定只會套用在該模組。

示例：

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

提示字變為：

```
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

此時：

- 可使用 `show options`
    
- 設定的 `RHOSTS`, `RPORT` 等只影響這個 exploit
    
- 換模組後設定會消失（除非用 setg）
    

離開模組：

```bash
back
```

---

# **5. show options — 查看目前模組需要的參數**

示例（MS17-010 EternalBlue）：

```
RHOSTS  ← 必填
RPORT   ← 預設 445
SMBUser / SMBPass ← 可選
Payload 相關設定（如 LHOST / LPORT）
```

`show options` 在不同模組會有不同輸出：

- exploit：通常需要 RHOSTS、RPORT
    
- scanner：通常需要 RHOSTS
    
- post：通常需要 SESSION
    

---

# **6. 設定參數**

## **設定單一模組參數**

```bash
set RHOSTS 10.10.10.5
set LHOST tun0
```

## **設定全域參數（跨模組皆有效）**

```bash
setg LHOST 10.10.10.5
```

---

# **7. 查看模組的詳細資訊：info**

```bash
info
```

或

```bash
info exploit/windows/smb/ms17_010_eternalblue
```

會顯示：

- 模組描述
    
- 作者
    
- 作用方式
    
- 參考來源
    
- 支援的目標平台
    

這不是 help，是完整文件。

---

# **8. 搜尋模組：search**

最重要指令之一。

## **最基本搜尋**

```bash
search ms17-010
```

輸出資訊包含：

|欄位|意義|
|---|---|
|Name|模組完整路徑|
|Rank|exploit 可靠度|
|Description|說明|

## ** Exploit Rank（可靠度等級）**

從低到高：

```
manual → low → average → normal → good → great → excellent
```

Rank 越高＝成功率越高、越不容易 crash。

---

# **9. search 進階搜尋（超實用 filter）**

### **搜尋 payload**

```bash
search type:payload windows
```

### **搜尋 auxiliary 模組**

```bash
search type:auxiliary telnet
```

### **搜尋某個平台**

```bash
search platform:windows smb
```

### **搜尋 CVE**

```bash
search cve:2017-0143
```

---

# **10. 選擇模組**

兩種方式：

### **方法 1：輸入完整名稱**

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

### **方法 2：使用 search 回傳的 index**

```bash
use 2
```

（只要上面那份列表中回傳的序號）

---

# **11. 列出可用 payloads**

```bash
show payloads
```

會顯示與 exploit 相容的 payload，例如：

- `windows/x64/meterpreter/reverse_tcp`
    
- `windows/shell_reverse_tcp`
    
- `generic/shell_bind_tcp`
    

---

# **12. 執行 exploit**

```bash
run
```

或

```bash
exploit
```

---

# **13. 善用 history**

查看你之前用過的指令：

```bash
history
```

---

# **14. Metasploit 不支援的行為（常見錯誤）**

❌ Output redirection：

```
command > file.txt
```

會報錯。

---

# **15. msfconsole 重點整理（超重要）**

### **你一定要記得：**

1. **search**：找模組
    
2. **use**：進入模組
    
3. **show options**：查看該模組需要什麼
    
4. **set / setg**：設定參數
    
5. **show payloads**：選 payload
    
6. **exploit / run**：執行
    
7. **sessions**：後期連線管理（之後課程會用到）
    
8. **info**：看模組文件
    
9. **Tab 補全超好用**
    
10. **上下文非常重要**（set 的設定不會跨模組）
    
