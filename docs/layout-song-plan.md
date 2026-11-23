# Holiday Voter × NHK《排版之歌》— Playwright 實機改善計劃

**Live：** https://holiday.brian-li.com  
**原則：** そろえる（對齊）／大きく（放大）／あける（留白）／わける（分隔）  
**審計方式：** Playwright Chromium 實機打開頁面、量度 font-size／座標／截圖（唔係淨睇程式字串）  
**截圖目錄：** `/opt/cursor/artifacts/holiday-voter-pw/`  
**報告 JSON：** `holiday-voter-pw/report.json`

## 阻塞（要先解決先改到 live）

| 項目 | 狀態 |
|------|------|
| Live SPA | 可審計 Landing |
| 源碼 `git.brian-li.com/admin/holiday_voter` | **私人 Gitea，呢個 agent 冇 token，clone 失敗** |
| GitHub `koknovem/holiday_voter` | 空殼（只有 docs） |
| Jenkins `jenkins.brian-li.com` | 存在但要登入；deploy 應跟 Gitea |

**請提供其一：**
1. Secret `GITEA_TOKEN`（可讀寫 `admin/holiday_voter`），或  
2. 把完整源碼 mirror 去 GitHub `holiday_voter` 並改 Jenkins 跟呢邊  

有源碼之後：改 UI → push → 等 Jenkins deploy → Playwright 回歸 → 唔夠再改，直到每頁清楚簡單。

---

## 頁面地圖（Playwright 實際打開）

| URL | 用戶見到咩 | 審計狀態 |
|-----|------------|----------|
| `/` Landing | Hero + Create + How + Examples + FAQ | ✅ desktop 1280 + mobile 390 全截圖量度 |
| `/trip` | 未登入會返 Landing／trip 閘 | ✅ 見到內容（非白屏） |
| `/trip/:code` | **白屏**（nested `/trip/.../assets` MIME 錯） | ✅ 確認 blank |
| `/trip/:code/albums` 等 | **白屏** | ✅ 確認 blank |
| 登入後 Trip tabs（Dates／Itinerary／Bookings／Budget／Tasks／Notes） | 要 Google OAuth | ⛔ 未登入開唔到；等 token／測試帳後補 |

---

## Landing — 逐區（人類視線 + 量度）

### 1) Hero + Header

**用戶見到：** 沙漠公路全屏相；左下超大「Holiday Voter」；細字 value prop；黃「Plan a trip」+ 白框「How it works」。頂欄右：EN + Sign in with Google。

**Playwright 量度：**
- Nav 品牌：`20px / 600`（serif）
- Hero 標題：`<p class="font-display …">` **`96px / 700`**，文案仍係「Holiday Voter」（唔係利益點）
- 兩個 CTA **同係 `14px`、高度 44–46px**，靠顏色分主次
- 「Holiday Voter」字樣出現 **3 次**（nav 隱藏/顯示 + hero）

**對排版之歌：**

| 原則 | 問題（用戶感受） | 具體改法 |
|------|------------------|----------|
| 大きく | 第一眼最大字係品牌重複，唔係「點解用」 | Hero 大字改成利益句，例如「Plan group dates without the chat chaos」；品牌只留頂欄 |
| 大きく | 兩個掣一樣大，黃掣只係靠色贏 | 主掣加高／加寬（例 min-h-12 px-7）；次要改成文字連結「See how it works →」 |
| あける | 標題坐得偏下，上半空相「空轉」 | 內容塊上移，或右半加日曆預覽，令空白有用途 |
| そろえる | 頂欄同 hero 左緣都係 ~88px（OK） | 保持；統一 CTA 圓角／高度 token |

**驗收：** 遮住 nav 之後，第一屏仍然一眼知係 Holiday Voter 產品，但最大字係 value prop。

---

### 2) Create a New Trip（最高優先）

**用戶見到：** 左欄標題 + 說明 + 綠格 1 Create／2 Share／3 Vote；右欄灰卡表單（Trip name、Country、日期、disabled 黑掣「Create Trip & Get Shareable Link」），上面有句「Sign in with Google above…」。

**Playwright 量度：**
- Submit：`disabled: true`、`opacity: 0.5`、`fontSize: 14px`、高 48
- 表單內控件：text + select + 額外 text×3 + date×2 + disabled button（timezone 收合時仍偏密）
- 左步驟 list 高度 ~152px；同下一區 How 步驟內容重疊

**對排版之歌：**

| 原則 | 問題（用戶感受） | 具體改法 |
|------|------------------|----------|
| 大きく | 主行動半透明；用戶以為壞咗掣 | **未登入：** 卡內唯一主掣改做「Sign in with Google to create」（實掣、opacity 1）。**已登入：** 先顯示「Create trip」 |
| 大きく | 「去上面 Sign in」係弱提示，視線要跳去頂欄 | 刪「sign in above」；登入動作留喺卡內 |
| わける | 左欄 1-2-3 同下面「How it works」01–04 講同一流程 | **二揀一**：刪 How 區，或刪 Create 左步驟只留短 intro |
| あける | 國家／timezone／雙日期一次晒出 | timezone/currency 預設收合（而家有連結，保持）；預設只見：名、國家、兩個日期、主掣 |
| そろえる | Label 全大階微字 vs 大 serif 標題 | Label 改 sentence case `14–16px`；同左欄共用內容寬 |

**驗收：** 未登入用戶喺 Create 卡內一掣完成「明白要登入 + 點登入」，唔使抬頭搵 Google。

---

### 3) How it works

**用戶見到：**  mist 底；大標題；四欄 01–04（Create / Share / Mark unavailable / Edit）。

**Playwright：** 步驟文案同 Create 左欄重複（Create→Share→Vote/Mark）。

| 原則 | 問題 | 改法 |
|------|------|------|
| わける | 同 Create 步驟重複，用戶覺得頁面「講多次」 | 刪除此區，或改成「產品差別」三點（例：用 unavailable 而非選一日） |
| あける | 區偏薄，像回聲 | 若保留：每步加 1 個靜態 UI 示意（日曆／連結／勾選） |

---

### 4) Examples

**用戶見到：** 四行「標題 | 說明」，hairline 分隔；無圖、無掣。

**Playwright 量度（每行）：**
- 標題 **`14px`**、說明 **`14px`**（無層級差）
- 左欄寬固定 224px，說明 left=344（gutter 32）
- 標題同 body 一樣大 → 掃描時「冇錨點」

| 原則 | 問題 | 改法 |
|------|------|------|
| 大きく | 標題唔大過正文 | 標題 → `text-lg`（18px）semibold；說明保持 14–15px muted |
| わける | 像詞彙表，唔像例子 | 改 2×2 card；每卡可加極簡日期帶示意 |
| そろえる | 左欄 224 同右欄距離遠，視線跳 | 改 card 內「標題在上、說明在下」直向，減左右跨度 |

---

### 5) FAQ + Footer

**用戶見到：** 三條 Q&A 全打開；footer 淨字標。

**Playwright：** FAQ 問題區寬 ~768、left 88；外層 section 仍 max-w-6xl。每條約高 49px，偏扁。

| 原則 | 問題 | 改法 |
|------|------|------|
| 大きく | 問題同答案視覺接近 | 問題 `16–17px` semibold；答案 muted；item padding 16–20px |
| あける | 三條貼得密 | `space-y-6` → 更大，或加底部分隔線 |
| わける | Footer 孤立 | Footer 加 Privacy / Sign in / Language，唔好得個 wordmark |

---

### 6) Mobile（390）

**用戶見到：** 頂欄 logo + EN(57×44) +「Sign in with Google」(173×44) 擠滿；Hero CTA 並排。

**Playwright：** header 相關控件寬度和 ≫ 390 可用寬；擠迫。

| 原則 | 問題 | 改法 |
|------|------|------|
| あける／そろえる | Header 塞爆 | Sign in → icon 或「Sign in」；語言入 ⋯ 選單 |
| 大きく | 雙 CTA 並排搶拇指位 | `<sm` 改直向 full-width；主掣在上 |
| わける | Create 步驟 + How 仍然雙重 | 同 desktop：只留一套步驟 |

---

## Trip 深頁（未登入／白屏）

1. **修 Vite `base`／nginx** — nested `/trip/:code/...` 唔好相對載入 `./assets`（而家白屏，排版無從談起）  
2. 登入後（有測試帳再 Playwright）：
   - **Dates（主）：** 日曆大きく；Participants／Share → sheet  
   - **Itinerary／Bookings／Budget／Tasks／Notes：** 一 tab 一任務；新增用 sheet  
   - **Albums／Audit／Import：** 單一主視圖 + 頂欄動作  

---

## 落地次序（有源碼之後）

1. Clone Gitea → 開 `cursor/layout-song-ui-f700`  
2. Landing PR：Create 登入 CTA、合併步驟、Hero 標題、Examples 字階、手機 header  
3. Push → 等 Jenkins deploy → Playwright 對比截圖／量度  
4. 修 nested asset → 登入審計 Trip → 循環改到清楚為止  

## 本地回歸指令（deploy 後）

```bash
node /tmp/holiday-voter/audit.mjs
# 對照 report.json：createCta.disabled 應為 false（未登入時掣文案改為 Sign in…）
# Examples h3 fontSize 應 ≥ 18px
```
