# 🤖 Antigravity (AGY) Agent Action Manual & Project Context: {{TRIP_TITLE}}

本檔案（`AGENTS.md`）記錄了「{{TRIP_TITLE}}」的專案脈絡、技術架構、Notion 同步機制以及後續 Agent 的行動指南。當您重新啟動本工作區時，**請務必先閱讀此檔案**，即可掌握前情提要並直接為使用者進行行程修改與同步。

---

## 🗺️ 1. 專案與行程背景 (Project Context)

*   **目的地：** {{DESTINATION}}
*   **行程天數：** {{DURATION}} ({{DATES}})
*   **航班資訊：** {{FLIGHT_INFO}}
*   **入住飯店：** {{HOTEL_INFO}}
*   **核心行程亮點：**
{{DAILY_HIGHLIGHTS}}

---

## 🛠️ 2. 工作區技術架構 (Technical Architecture)

工作區設有完整的本地與 Notion 雙向同步管線，結構如下：

```text
./
├── Notion_Itinerary/             # 本地 Markdown 來源資料夾 (多網頁結構)
│   ├── Trip_Dashboard.md         # 母儀表板首頁 (總覽、快速連結與甘特圖)
│   ├── Day1_<Theme>.md           # 各天細節頁
│   ├── ...
│   ├── Travel_Tips_and_Apps.md   # 實用行前工具與 App 設定說明
│   └── Gourmet_and_Spots.md      # 精選餐廳與地標雙語/三語對照地址表
├── Full_Itinerary.md             # 單頁完整行程備份檔 (Markdown)
├── Trip_Itinerary.ics            # iCalendar 行事曆檔案 (雙擊可一鍵匯入手機/日曆)
├── sync_to_notion.py             # 核心同步程式 (將本地 Markdown 檔案解析同步至 Notion)
├── .env                          # 存放私密 Notion Token 與網址 (Git 忽略，安全第一)
└── .gitignore                    # 防火牆設定，確保 .env 金鑰永不外洩
```

---

## 📋 3. 後續 Agent 行動指南 (Instructions for Future Agents)

當使用者在後續對話中提出修改行程時，請嚴格遵循以下操作規範：

### 🔗 規則一：一律使用相對路徑 (Use Relative Paths Only)
*   **Use Relative Paths Only**: Never use absolute paths (such as `/Users/...` or `file:///...`) in any project documentation, links, or files; always use relative paths (e.g., `./Notion_Itinerary/Day1_Arriving.md`, `./sync_to_notion.py`).

### 🔄 規則二：修改本地 Markdown 檔案與行事曆
*   根據使用者的修改需求，直接修改 `./Notion_Itinerary/` 中對應的 `.md` 檔案。
*   若修改涉及到具體時間或重要預約，同步更新單頁備份 `./Full_Itinerary.md`，並**重新生成 `./Trip_Itinerary.ics` 行事曆檔案**，以保持行事曆最新。

### 🚀 規則三：自動執行同步 (Zero-Click Sync)
*   **不要讓使用者動手！** 本地檔案修改完成後，您必須**主動且自動地**在背景執行以下指令：
    ```bash
    python3 sync_to_notion.py
    ```
*   這會直接讀取 `./.env` 中的金鑰，將所有修改即時推送並渲染至使用者的 Notion 中。
*   同步成功後，告知使用者已修改完畢，提示他們直接重整 Notion App / 網頁看效果！

### 🛡️ 規則四：資安保護意識
*   絕對不要泄露、顯示或將 `./.env` 中的 `NOTION_TOKEN` 列印到終端機日誌或使用者對話中。
*   確保所有憑證操作都嚴格保留在本地環境。
