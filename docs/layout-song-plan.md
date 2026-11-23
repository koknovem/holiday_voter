# NHK《排版之歌》網站改善計劃

依據 NHK Eテレ《デザインあ》〈ならべうた／Layout Song〉四原則：

| 日文 | 中文 | 網站規則 |
|------|------|----------|
| そろえる | 對齊 | 同一左緣／欄位；唔好左右亂飄 |
| 大きく | 放大 | 主標題／主 CTA 明顯大過次要資訊 |
| あける | 留白 | 區塊之間留呼吸；唔好塞滿 |
| わける | 分隔 | 狀態、動作、內容分組 |

## 現況

- **holiday_voter**：只有 `README.md`，冇前端分頁可審計。
- **可審計對象**：現有網站 [buy-stuffs](https://buy.brian-li.com)（群組協作 UI，結構接近未來 holiday voter）。

互動版計劃：[NHK 排版之歌 · 網站改善計劃](https://cursor.com/canvas/store-7eb9b862-6448-4180-8d52-d3b94179f1eb/cadb8fbe-fdc5-47d1-84c8-05b8f5583442)

## 逐頁審計（buy-stuffs）

| Route | 評分 | 主要問題 | 改善動作 |
|-------|------|----------|----------|
| `/login` | B | 品牌夠大；eyebrow／Google 掣未成清晰對齊欄 | 品牌→說明→主 CTA 三段；弱化 sub |
| `/nickname` | C | 兩 wide 掣並排層級唔清；emoji 代文字 | 確認唯一 primary；跳過改文字連結 |
| `/` Home | C | 四段擠埋；刪除掣同 tile 錯位 | topbar 一條線；列表獨立加大間距 |
| `/join` | B- | 輸入／動作留白不足；掃描區突兀 | 輸入→動作→掃描三段；掣加文字 |
| `/trip/:id` | **D** | 頂欄堆疊；成員／QR／菜式／編輯混晒 | 頂欄只留返回+碼；編輯用 sheet |
| `/trip/:id/add` | C+ | emoji grid 噪音；儲存／生成層級模糊 | 圖示收合；儲存唯一 primary |

### Trip 頁（優先）

**問題（わける 失敗）**

1. 頂欄同時：返回、邀請碼、同步、重置、加菜
2. 菜式頭、AI 狀態、編輯、食材、行內面板垂直混疊
3. `shop-item` 三欄旁再加編輯掣 → 對齊線斷裂

**建議結構**

1. **そろえる**：16px gutter；ListRow = `icon | 名稱 | avatar`；編輯改長按／滑出
2. **大きく**：邀請碼／菜式名用 display；食材 body；meta 再細
3. **あける**：菜式間 24px、食材間 8px、頂欄下 20px
4. **わける**：A 邀請／成員（可摺）· B 清單（主）· C 編輯（sheet）

## Holiday Voter 由零起藍圖

| 頁面 | 單一任務 | 大きく | わける |
|------|----------|--------|--------|
| Landing / Login | 登入／了解 | 品牌名 | 品牌｜說明｜CTA |
| Home | 睇／開投票 | 新投票 CTA | 進行中｜已結束 |
| Create | 設日期選項 | 下一步 | 標題｜選項｜動作 |
| Vote room | 投票／結果 | 勝出日期／進度 | 選項｜票數｜成員 |
| Invite | 分享 | 邀請碼 | 碼｜QR｜複製 |
| Result | 確認假期 | 確定日期 | 結果｜摘要｜分享 |

## 落地階段

0. **決策**：改 buy-stuffs，定起 holiday_voter  
1. **Layout Token**：gutter、space、type、tap  
2. **全局殼層**：Topbar、PageHeader、Section、ListRow、PrimaryButton  
3. **逐頁重排**：Trip → Home → Add → Join → Login  
4. **驗收**：對齊線、字階、間距、三分開；375／390 實機  

## 下一步（請揀）

- **A**：開 buy-stuffs branch，先收 Trip + Home  
- **B**：喺 holiday_voter 用 Layout Token + 六頁藍圖由零起  
