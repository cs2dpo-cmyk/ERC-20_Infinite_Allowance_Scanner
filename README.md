---
title: Python 程式設計與專題練習紀錄
tags: Python, 程式設計, 練習專題, 筆記, HackMD筆記
---

# Python 程式設計與專題練習紀錄

---

## 一、 專案簡介 (Overview)

* **學習目標**：紀錄 Python 學習過程中的語法練習、資料結構實作與自主開發的小專題。
* **核心內容**：涵蓋基礎語法、資料處理、第三方 API 串接、自動化腳本與簡易介面開發。
* **專案定位**：作為個人程式開發能力累積的實作庫，同時建立良好的模組化架構與 Git 版本控制習慣。

> :::info
> **學習宣告**：本儲存庫透過持續性的練習與小專題開發，循序漸進地優化程式碼品質，並落實 PEP 8 風格規範。
> :::

---

## 二、 學習範疇與技術棧 (Tech Stack & Topics)

| 類別 | 技術 / 套件名稱 | 學習主題與用途 |
| :--- | :--- | :--- |
| **基礎語法** | Python 3.10+ | 變數型別、控制結構、函式、物件導向 (OOP) |
| **資料處理** | Pandas, NumPy | 資料清洗、陣列運算、CSV / JSON 檔案讀寫 |
| **網路應用** | Requests, Beautiful Soup | Web API 串接、網路爬蟲與資料收集 |
| **介面與工具** | Streamlit / Tkinter | 簡易 GUI / Web 介面開發與數據視覺化展示 |
| **開發規範** | Ruff, Black | 語法風格自動檢查與格式化排版 |

---

## 三、 專案結構 (Project Structure)

```text
Python_Practice/
├── .vscode/               # VS Code 專案專屬設定檔
├── 01_basics/             # 基礎語法與資料結構練習
│   ├── variables.py
│   └── data_structures.py
├── 02_advanced/           # 進階主題 (OOP、例外處理、檔案操作)
│   └── file_handling.py
├── 03_projects/           # 獨立實作小專題
│   ├── web_scraper/       # 爬蟲練習專題
│   └── data_dashboard/    # 數據看板專題
├── .gitignore             # Git 版本控制忽略設定
├── main.py                # 練習程式進入點
├── requirements.txt       # 專案依賴套件清單
└── README.md              # 學習說明文件
