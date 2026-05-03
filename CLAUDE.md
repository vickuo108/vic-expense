# CLAUDE.md — expense-app-v1 專案設定

## 版本升級時機

**等 Vic 說「完成此次升級後」**，才執行以下三件事。一次升級可能包含多個功能修改，全部改完再統一升版。

1. **升級 `sw.js` 的 CACHE 版本號**
   - 格式：`vic-v2-{數字}`，每次遞增一號（例如 `vic-v2-15` → `vic-v2-16`）

2. **升級 `index.html` 的 `APP_VERSION`**
   - 小改動：第三位遞增（`1.0` → `1.1` → `1.2`）
   - 大改動：第二位遞增（`1.2` → `1.3`，視改動幅度而定）

3. **在 `index.html` 的 `CHANGELOG` 陣列最前面插入新條目**
   - 格式：
     ```javascript
     {version:'1.1', date:'YYYY-MM-DD', items:[
       '這次改了什麼（簡短描述）',
       '...',
     ]},
     ```
   - 日期使用台北時間（執行 `date` 確認）

---

## 專案結構

```
expense-app-v1/
├── index.html   # 全部邏輯（CSS、HTML、JS 合一）
├── sw.js        # Service Worker（快取版本控制）
├── manifest.json
└── CLAUDE.md
```

## 重要位置（index.html 內）

| 內容 | 位置 |
|------|------|
| APP_VERSION / CHANGELOG | JS 區最底部，`// ─── 版本與更新日誌` 區塊 |
| 固定支出預設清單 | `DEFAULT_FIXED` |
| 財務語言詞典預設值 | `DEFAULT_GLOSSARY` |
| Service Worker 註冊 | `initReportNav` 附近 |
| 每月預算儲存 | `KEY_MONTHLY_BUDGETS = 'vic2_monthly_budgets'` |

## 功能說明

- **更新橫幅**：sw.js 版本升級後，瀏覽器偵測到新 SW 等待時，頂端出現「立即更新」橫幅
- **更新日誌 Modal**：App 啟動時比對 `APP_VERSION` 與 localStorage `vic2_seen_version`，版本不同則彈出更新內容
- **每月預算**：儲存在 `vic2_monthly_budgets`，格式 `{"2026-05": {類別: 金額}}`，固定支出 key 為 `"fixed:項目名"`，貸款 key 為 `"loan"`
- **財務語言**：儲存在 `vic2_glossary`，設定頁底部按鈕進入

## 語言與溝通

- 一律繁體中文回覆
- 修改前若範圍較大，先輸出簡要計劃等確認
- 小修改直接執行，不需要詢問
