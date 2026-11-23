# Holiday Voter × NHK《排版之歌》改善計劃

**對象：** [https://holiday.brian-li.com](https://holiday.brian-li.com)（Holiday Voter）  
**原則：** そろえる（對齊）／大きく（放大）／あける（留白）／わける（分隔）

互動版：[Holiday Voter 排版計劃 Canvas](https://cursor.com/canvas/store-7eb9b862-6448-4180-8d52-d3b94179f1eb/cadb8fbe-fdc5-47d1-84c8-05b8f5583442)

## 現況

| 項目 | 狀態 |
|------|------|
| Live 網站 | `holiday.brian-li.com` 有完整 SPA |
| 本 repo `holiday_voter` | 只有 README（空殼）— **源碼未入 repo** |
| Landing | 已實機審計（desktop 1280 + mobile 390） |
| Trip 內頁 | 需 Google 登入；未登入深鏈因 nested asset path 會白屏 |

## Landing 逐區審計

| 區塊 | 評分 | 問題 | 改善動作 |
|------|------|------|----------|
| Hero / Header | C+ | 品牌名出現兩次；雙 CTA 高度接近；手機 header 擠 | Hero 改價值主張；品牌只留 nav；主 CTA 加大；手機 Sign in 改 icon |
| **Create a New Trip** | **D** | 左側 1-2-3 同 How it works 重複；disabled CTA +「sign in above」弱 | 只留一套步驟；卡內「Sign in with Google to create」；timezone 預設收合 |
| How it works | C | 同 Create 步驟重複；區塊偏薄 | 合併或刪除；若留就加示意 UI |
| Examples | C | 標題 14px 同正文一樣；長 gutter；像詞條 | 標題 18–20px；2×2 card |
| FAQ + Footer | B- | FAQ 寬度同其他區唔齊；偏密 | 統一 max-width；footer 加連結 |
| Mobile landing | C | logo+EN+完整 Google 掣擠爆；CTA 並排 | Sign in 縮短；CTA 直向 full-width |

### Create 區（最高優先）

1. **わける** — Create 迷你步驟 vs How it works：全站只留一套  
2. **大きく** — 未登入唔好用半透明 disabled；卡內直接主掣登入  
3. **あける** — timezone／currency 預設收合；欄位間距 20–24px  
4. **そろえる** — label 改 sentence case；同左欄共用內容寬

## App 內頁藍圖（登入後）

| Route / 模組 | 排版重點 |
|--------------|----------|
| `/trip` My trips | 列表同「再建一個」分開；搜尋／legacy 收合 |
| `/trip/:code` Trip room | Topbar + tabs；預設 Dates；其餘模組唔好一頁塞晒 |
| Dates / Vote | 日曆＝主視覺；Participants／Share 用側欄或 sheet |
| Itinerary | 日卡片列表；新增用 sheet |
| Bookings | 列表｜新增分開；金額右對齊 |
| Budget | 總額大きく；明細同 settlement 分兩區 |
| Tasks / Notes | 單欄清單；新增 sheet |
| `/albums` | 全寬媒體網格 |
| `/audit-*` | 單一主視圖；下載歸頂欄 |
| `/booking-import` | 上傳→預覽→確認 一步一屏 |

## 落地階段

0. **源碼入 repo** — 將 Holiday Voter frontend push 入 `koknovem/holiday_voter`  
1. **Layout Token** — gutter、content-max、space scale、type scale、CTA 層級  
2. **Landing 重排** — 合併步驟、Create 登入 CTA、Hero、Examples、手機 header  
3. **Trip room 殼層** — Topbar + tabs；Dates 為主  
4. **深頁 + 驗收** — Albums／Audit／Import；修 nested route asset base；375／1280 檢查  

## 下一步（請確認）

1. 把 Holiday Voter **源碼**放進呢個 repo（或告訴我源碼喺邊）  
2. 我跟 Phase 2 先改 **Landing**（唔使登入都可以驗收）
