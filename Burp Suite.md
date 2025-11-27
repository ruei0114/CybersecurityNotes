> [!info] LINK & TAG
> [TryHackMe | Burp Suite: The Basics](https://tryhackme.com/room/burpsuitebasics)
> 
> #tag


雖然 **Burp Suite Community** 相較於專業版功能受限，但它依然像一把多段式折刀，提供許多對 Web 測試極具價值的工具。以下是其核心功能整理：

---

## **🔎 Proxy**

Burp 最經典的功能。  
能攔截、檢查、修改瀏覽器與 Web 伺服器之間的請求與回應。  
是多數 Web 滲透測試流程的起點。

---

## **🔁 Repeater**

用來接住某個請求，修改後重新送出。  
適合：

- 調 payload（例如 SQL Injection）
    
- 測試 endpoint 的參數處理情況
    
- 手動 fuzz 某些敏感功能
    

像反覆敲響同一扇門，觀察對方會給什麼不同反應。

---

## **🚀 Intruder（社群版有限速）**

能向目標大量發送請求，可用於：

- 暴力破解（brute force）
    
- fuzz endpoint
    

雖然社群版速度被壓得像龜速試跑，但仍足以做基本測試。

---

## **🔐 Decoder**

提供編碼與解碼功能，可用於：

- 轉換攔截到的資料
    
- 將 payload 做必要的編碼再送出
    

雖然網路上也有很多工具能做，但在 Burp 內直接處理更有效率。

---

## **🧩 Comparer**

比較兩段資料（字 / byte 等級）。  
適合找出：

- 不同輸入造成回應哪裡不同
    
- API 回應間的細微差異
    

只要按一下就能把資料送進工具，比 copy-paste 優雅多了。

---

## **🎲 Sequencer**

分析隨機值（像 session token）的 randomness。  
如果產生 token 的演算法不夠亂，就可能讓攻擊者推算或預測出有效的 session。

---

# **🧰 Extender（擴充套件）**

因為 Burp 是用 Java 寫成，因此可以發展 extension 來擴充功能。  
支援語言：

- Java
    
- Python（透過 Jython）
    
- Ruby（透過 JRuby）
    

透過 **Extender** 可快速載入外掛，而 **BApp Store** 則提供第三方擴充套件下載。

> 即使社群版也能用一些外掛，例如常見的 **Logger++**（強化內建 log 工具）。

---

如果你想，我也可以幫你做成更簡潔版、考試用版、或課堂講義版（含圖示流程） 😄