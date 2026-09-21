# 🔍 ERC-20 Unlimited Allowance Scanner & Revoker

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Web3.py](https://img.shields.io/badge/Web3.py-v6.0%2B-orange)

一個基於 Python (`web3.py`) 開發的自動化鏈上安全工具，專為檢測與撤銷（Revoke）高風險或無限授權（Unlimited Allowance）的 ERC-20 代幣合約而設計。幫助使用者與資安研究員主動防範 Web3 釣魚攻擊與 dApp 漏洞帶來的資產風險。

---

## 💡 為什麼需要這個工具？

在互動 DeFi 或 NFT 平台時，許多 dApp 會要求使用者簽署「無限授權（Unlimited Allowance）」，以節省未來交易的 Gas 費。然而，一旦該 dApp 的智慧合約遭駭，或使用者誤簽釣魚合約，攻擊者即可在無須取得私鑰的情況下轉走你錢包中的所有受授權代幣。

本專案提供自動化掃描與撤銷機制，協助使用者盤點並清理隱藏的鏈上授權風險。

---

## ⚙️ 運作原理 (Operating Principles)

### 1. 背景與授權機制 (Background)
在以太坊 ERC-20 標準中，去中心化應用程式（dApp，如 Uniswap、Aave）無法直接扣除使用者錢包內的代幣。使用者必須先執行 `approve(spender, amount)` 函式，授權特定的合約地址（spender）可以動用的最大代幣數量。

為提升用戶體驗並節省頻繁授權的 Gas 費，許多 dApp 會要求使用者一次性授權「無限額度」（即 $2^{256} - 1$ 或 `type(uint256).max`）。當該 dApp 存在安全漏洞或被駭客攻擊時，駭客即可在無須取得私鑰的情況下，直接將使用者錢包內的受授權代幣轉走。

### 2. 核心掃描流程 (Workflow)
本掃描器透過以下四個步驟實現鏈上記錄檢索與即時風險分析：

```text
[使用者錢包地址]
       │
       ▼
1. 擷取歷史 Approval 事件日誌 (Event Logs) ➔ 找出所有歷史互動過的 代幣與 Spender
       │
       ▼
2. 呼叫 ERC-20 合約 allowance() ➔ 查詢即時剩餘授權額度 (Real-time State)
       │
       ▼
3. 數值比對與風險判定 ➔ 判斷是否等於或接近 2^256 - 1
       │
       ▼
4. 輸出風險報告與撤銷建議 (Risk Report)
```

#### Step 1: 擷取歷史授權事件 (Event Retrieval)
掃描器利用 RPC 節點或 Etherscan API 檢索指定錢包地址（`owner`）在區塊鏈上觸發過的所有 ERC-20 `Approval` 事件：

```solidity
event Approval(address indexed owner, address indexed spender, uint256 value);
```

此步驟用於建立「代幣合約（Token） - 授權對象（Spender）」的待查清單。

#### Step 2: 查詢即時授權狀態 (Real-time State Query)
歷史事件僅代表「過去的動作」。為了取得當前真實狀態，掃描器會對各 ERC-20 代幣合約發起唯讀呼叫（`eth_call`），執行 `allowance` 查詢：

```solidity
function allowance(address owner, address spender) external view returns (uint256);
```

此步驟完全不需消耗 Gas 費，能取得鏈上當前剩餘的授權額度。

#### Step 3: 風險門檻判定 (Risk Level Analysis)
將傳回的 `allowance` 數值與風險門檻進行比對：

* **高風險（無限授權）**：授權值等於或接近最大值 $2^{256} - 1$（即 `115792089237316195423570985008687907853269984665640564039457584007913129639935`）。
* **中/低風險（有限授權）**：授權值大於 0 但小於高風險門檻。
* **無風險**：授權值為 0（已撤銷）。

#### Step 4: 整合與報告輸出 (Report Generation)
將數據整合為結構化報告，顯示：

* 受授權代幣名稱與圖示
* 授權對象名稱（比對已知標籤如 Uniswap V3, OpenSea 等）
* 當前授權額度與風險等級
* 提供一鍵前往撤銷（Revoke）的合約互動連結

---

## 🛠️ 技術棧 (Tech Stack)

* **Language**: Python 3.8+
* **Blockchain Interaction**: `web3.py`
* **Network & RPC**: Alchemy / Infura / QuickNode / Etherscan API
* **Environment Management**: `python-dotenv`

---

## 🛡️ 安全建議 (Security Best Practices)

1. **私鑰安全**：建議先在測試網（如 Sepolia）上測試撤銷功能，確保腳本運作正常。
2. **RPC Rate Limits**：檢索大量歷史日誌時，建議使用專業 RPC 節點或 API 限速處理（Rate Limit Handling），避免請求被拒。

---

## 📚 參考資源 (References)

本專案的架構設計與實作參考了以下優秀的資源：

* **核心實務參考**：[freeCodeCamp - Learn Blockchain, Smart Contracts, and Solidity Tutorial](https://www.youtube.com/watch?v=M576WGiDBdQ)
* **EIP Standard**: [EIP-20: Token Standard](https://eips.ethereum.org/EIPS/eip-20)
* **開發文件**: [Web3.py Official Documentation](https://web3py.readthedocs.io/)