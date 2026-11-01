# 百分比計算器 — Percentage Calculator

全螢幕百分比計算器 PWA。多欄位加權總和（金額 × 百分比），支援負值輸入。

來源：Gemini 分享 <https://share.gemini.google/Jg7bptY8Gqle>（share id `4a265479289c`）

## 功能

- 全螢幕單頁版，大字體、數字優先
- 頂部獨立顯示加權總和（正數綠／負數紅）
- 預設 4 欄（100% / 75% / 100% / 75%），可新增、刪除、清空
- 金額欄位支援負值輸入
- 百分比下拉選單 5% ~ 100%（每 5% 一階）

## 本機記憶（localStorage 持久化）

原始碼**完全沒有持久化**——所有欄位值都在記憶體的 `entries` 陣列，重新整理即歸零。
本次改為寫入 `localStorage`，重新整理、關閉再開、離線都會保留。

| 項目 | 值 |
|---|---|
| key 前綴 | `pct_calc_v1:` |
| 欄位資料 key | `pct_calc_v1:entries` |
| nextId key | `pct_calc_v1:nextId` |
| 保存內容 | `[{ id, amount, percentage }]` + `nextId` |
| 寫入時機 | 金額變更 / 百分比變更 / 新增 / 刪除 / 清空 / 首次載入 |

畫面上有「本機記憶已開啟」狀態指示，以及「重設」按鈕（清除記錄並還原預設 4 欄）。

> **範圍說明**：`localStorage` 是瀏覽器／網站 origin／裝置層級的儲存。
> 清除網站資料、換瀏覽器或換裝置不會保留；同一瀏覽器開同網址則會保留。

## PWA

- `manifest.webmanifest` — standalone、`#0b0f19` 主題、5 種 icon（含 maskable）
- `sw.js` — app shell cache-first、導航 network-first（離線回退 `index.html`）、靜態 stale-while-revalidate
- Service Worker cache 名：`pct-calc-v1`

## 離線

所有第三方資源已本地化到 `vendor/`，**線上殘留外部 URL 0**：

| 檔案 | 來源 |
|---|---|
| `vendor/tailwind.js` | Tailwind CDN（407 KB） |
| `vendor/fontawesome.css` + `vendor/webfonts/*.woff2` | Font Awesome 6.4.0 |
| `vendor/inter.css` + `vendor/fonts/inter-*.woff2` | Google Fonts Inter（latin + latin-ext） |

## 部署

GitHub Actions 自動部署到 GitHub Pages（`build_type=workflow`）。

```bash
# 本機預覽
python3 -m http.server 8080

# 手動觸發部署
gh workflow run deploy.yml
```

線上：<https://travisebill.github.io/percentage-calculator/>
