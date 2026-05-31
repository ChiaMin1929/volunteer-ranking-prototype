# 排志願功能規格文件
> 版本：v6.0｜適用平台：手機版優先（最大寬度 430px）｜供 Claude Code 產出原型使用
> 更新說明：v6 基於 v5 五星制版本，全面改版推薦排序流程（單步驟化）、導入全局指標（allInds）架構、調整 AllTab／ComboTab 互動設計，並修正多處文案與版面細節。本文件為完整獨立規格，不再參照舊版。

---

## 一、功能概述

在落點分析系統中，使用者可以將感興趣的校系加入志願清單，並在「排志願頁」管理順序、建立不同情境的組合，以及使用自動推薦排序功能。推薦排序以 11 項學校品牌力指標的 PR 值加權計算，結果以**五星指標標籤**呈現在卡片上，供使用者手動微調時參考。

---

## 二、設計規範

### 色彩系統

```css
/* 品牌主色 */
--brand: #D85A30;
--brand-light: #FAECE7;
--brand-border: #F0997B;
--brand-text: #993C1D;

/* AI 功能色（紫色系） */
--ai-bg: #EEEDFE;
--ai-border: #AFA9EC;
--ai-dark: #3C3489;
--ai-mid: #534AB7;

/* 錄取機會色 */
--dream-bg: #EEEDFE;    --dream-text: #3C3489;
--reach-bg: #FAEEDA;    --reach-text: #854F0B;
--standard-bg: #E6F1FB; --standard-text: #0C447C;
--safe-bg: #EAF3DE;     --safe-text: #3B6D11;

/* 指標等級 tag 色（五層） */
--t1-bg: #EEEDFE; --t1-txt: #3C3489; --t1-bd: #AFA9EC;  /* ★★★★★ */
--t2-bg: #E1F5EE; --t2-txt: #085041; --t2-bd: #5DCAA5;  /* ★★★★☆ */
--t3-bg: #E6F1FB; --t3-txt: #0C447C; --t3-bd: #85B7EB;  /* ★★★☆☆ */
--t4-bg: #FAEEDA; --t4-txt: #633806; --t4-bd: #EF9F27;  /* ★★☆☆☆ */
--t5-bg: #F1EFE8; --t5-txt: #5F5E5A; --t5-bd: #B4B2A9;  /* ★☆☆☆☆ */

/* 警示色 */
--danger-bg: #FCEBEB; --danger-bd: #F09595; --danger-txt: #A32D2D;
--warn-bg: #FAEEDA;   --warn-txt: #854F0B;
--info-bg: #E6F1FB;   --info-txt: #0C447C;
```

### 通用元件規範

- 圓角：卡片 `12px`、按鈕 `10px`、badge/pill `20px`
- 卡片邊框：`0.5px solid`，顏色依語意選用
- 卡片 padding：`12px 14px`
- 字體：系統預設 sans-serif
- 行距：說明文字 `1.5–1.7`

### 序號圓圈樣式

| 名次 | 背景色 | 字色 |
|------|--------|------|
| 1 | #534AB7 | #fff |
| 2 | #7F77DD | #fff |
| 3 | #AFA9EC | #fff |
| 4+ | #eee | #888 |

---

## 三、指標系統

### 11 項學校品牌力指標

| key | 名稱 | 說明 | icon |
|-----|------|------|------|
| hire | 學群聘僱力 | 若聘僱限定學群，企業會優先聘僱哪些學校畢業生 | ti-users |
| salary | 學群薪資力 | 2021–2025 最高學歷大學畢業，依學群排序第一份正職工作月薪中位數 | ti-coin |
| intl | 國際力 | 依教育部公開平台，外籍師資、出國交流、跨國合作、全外語課程等 5 項平均 | ti-world |
| job | 職務能力 | 哪些學校畢業生最符合企業聘僱新鮮人最在乎的 11 項職能 | ti-briefcase |
| future | 未來能力 | 哪些學校畢業生最符合企業聘僱新鮮人應具備的 14 項未來能力 | ti-rocket |
| fame | 知名度 | 不提示的情況下，企業最先想到哪些學校 | ti-star |
| acad | 學術聲望 | QS、THE、CWUR 三大國際排名學術聲望平均分數 | ti-school |
| indus | 產學力 | 依教育部公開平台，產學合作經費、衍生企業、合作企業新事業部門等 3 項平均 | ti-building-factory-2 |
| rd | 研發力 | 依教育部公開平台，申請專利授權件數、智財權衍生金額、研究計畫經費等 3 項平均 | ti-flask |
| char | 性格優勢 | 哪些學校畢業生最符合企業聘僱新鮮人最在乎的 27 項性格 | ti-mood-happy |
| leader | 高階領導力 | 各校求職會員曾任 100 人以上企業或上市櫃公司高階主管（總經理、副總等）的比例 | ti-crown |

> **注意**：`hire` 與 `salary` 為學群維度指標，若清單內同一學校有不同科系（跨學群），指標意義為「比較同校不同系的差異」，選擇時須特別留意。

### 星等對應（PR 值 → 星等）

| PR 值範圍 | 星等 | 顯示 | 樣式變數 |
|----------|------|------|---------|
| ≥ 95 | 5 星 | ★★★★★ | `--t1-*` |
| ≥ 85 | 4 星 | ★★★★☆ | `--t2-*` |
| ≥ 70 | 3 星 | ★★★☆☆ | `--t3-*` |
| ≥ 40 | 2 星 | ★★☆☆☆ | `--t4-*` |
| < 40 | 1 星 | ★☆☆☆☆ | `--t5-*` |
| null | 無資料 | ½★（半顆星） | `bg:#f1efe8, color:#888, border:#ccc` |

null 值以 CSS `overflow:hidden; width:0.55em` 的實心星呈現半顆星效果。

### 星等 tag CSS

```css
.tier-tag {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 6px;
  white-space: nowrap;
}
.tier-tag .tag-label { font-size: 11px; font-weight: 500; color: #444; }
.tier-tag .tag-stars { font-size: 13px; letter-spacing: 1.5px; line-height: 1; color: #F5C518; }
.tier-tag .tag-stars .star-empty { color: #ddd; }

/* 雙欄格線容器 */
.tier-tags {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px 6px;
  margin-top: 10px;
  padding: 10px 12px;
  background: #f9f9f9;
  border-radius: 8px;
  border: 0.5px solid #eee;
}
```

---

## 四、7 個在意方向（RecommendFlow 已移除，保留供舊版參考）

> v6 的推薦流程已改為單步驟直接選指標，不再有「在意方向」選題。以下資料保留供後台推薦邏輯參考。

| id | label | 對應指標 |
|----|-------|---------|
| work | 希望畢業後容易找到工作 | job, hire |
| salary | 希望畢業後起薪不輸人 | salary |
| fame | 希望就讀名聲好的學校 | fame, acad |
| industry | 希望在學時能接觸業界、累積實務經驗 | indus, rd |
| research | 希望學校重視研究，能接軌未來讀研究所 | rd, acad |
| intl | 希望有出國交換、接觸國際環境的機會 | intl |
| soft | 希望培養職場所需的軟實力 | job, char, future |

---

## 五、落點分析列表頁（ResultsPage）

### 5.1 整體佈局

- 上方固定 Navbar（104 落點分析 logo + 目前學年度標籤）
- 頁首：頁面標題、筆數副標、「我的志願」按鈕（含志願數 badge）
- 卡片列表：每筆一張，依錄取機率排序
- 固定底部浮動按鈕列（雙按鈕）

### 5.2 校系卡片

每張卡片包含：
1. 錄取機會 badge + 進度點 + 錄取機率 %
2. 學校名稱、科系名稱（代碼）
3. 城市｜學群｜興趣代碼
4. 右上角愛心按鈕（加入/移除志願）
5. 「看動態詳情」chevron 連結

愛心狀態：未加入 = `ti-heart`（灰色）；已加入 = `ti-heart-filled`（品牌色）

移除志願時需顯示**確認 Modal**：
```
標題：移除此志願？
內文：「{學校} {科系}」將從你的志願清單中移除，相關組合也會同步更新。
按鈕：取消 / 確認移除（紅色 #C0392B）
```

### 5.3 品牌力預覽版位（第 10 筆後插入）

目的：導引未登入使用者了解五星指標功能。

**視覺規格**：
```
背景：linear-gradient(135deg, #FAECE7 0%, #F5EEFF 100%)
邊框：0.5px solid var(--brand-border)
圓角：12px；padding：16px 16px 14px
margin：0 16px 12px
```

**內容結構**：
1. 標題（14px 粗體）：「把校系加入志願，就可以看到學校五星指標」
2. 示意卡片（白底）：
   - 學校名稱、科系名稱：**filter: blur(4px)**，禁止選取與點擊
   - 6 個指標星等清晰顯示（示範值如下）：

| 指標 | PR 示範值 | 星等 |
|------|---------|------|
| 知名度 | 75 | ★★★☆☆ |
| 國際力 | 87 | ★★★★☆ |
| 產學力 | 75 | ★★★☆☆ |
| 職務能力 | 87 | ★★★★☆ |
| 學群薪資力 | 96 | ★★★★★ |
| 學群聘僱力 | 87 | ★★★★☆ |

3. 「登入解鎖」按鈕（品牌色，全寬，含 `ti-lock-open` icon）

### 5.4 底部浮動按鈕列

雙按鈕，固定於畫面底部：
- 左：「修改成績」（白底、灰色邊框 #ccc，`flex: 1`）
- 右：「登入排志願」（品牌色，`flex: 1`，點擊進入排志願頁）

---

## 六、排志願頁（WishlistPage）

### 6.1 整體結構

```
┌─ Header（返回 + 「排志願」標題 + 志願數 badge）
├─ Tab bar（全部 | 組合一 | 組合二 … | +）
├─ Content 區（可捲動）
└─ Bottom action bar（依 active tab 不同）
```

### 6.2 Tab bar

- 第一個 tab 固定為「全部」
- 使用者建立的組合依序顯示
- 最末為「+」新增組合（組合數 < 10 時顯示）
- 超出寬度可橫向滾動

### 6.3 全局指標（allInds）架構

**WishlistPage** 持有 `allInds: string[]` 狀態，初始為 `[]`。

- 首次進入「全部」tab 時（`allInds.length === 0`），自動開啟 **RecommendFlow（mode='all'）**，引導使用者選定指標
- 使用者完成後，將選定的指標順序存入 `allInds`
- 若使用者關閉流程未完成，下次重新進入頁面時不再自動觸發
- `allInds` 會傳給 AllTab（`appliedInds={allInds}`）與所有 ComboTab（`defaultInds={allInds}`）

---

## 七、全部志願 tab（AllTab）

### 7.1 空狀態

| 情境 | 標題文字 |
|------|---------|
| `allInds` 已設定，尚無志願 | 加入志願就可幫你排志願囉 |
| `allInds` 未設定，尚無志願 | 還沒有加入任何志願 |

副文：「回到落點結果，點擊校系卡片右上角的 ♡ 加入志願，再來這裡排序。」

### 7.2 有志願時的版面結構

```
┌─ AI 推薦入口卡片
├─ 功能按鈕列（產生志願碼 ｜ 下載 Excel）
├─ 操作提示（長按可拖拉調整順序）
├─ 依加入時間排序說明（含志願數）
├─ 志願卡片 × n
│   └─ ⚠️ 第 5 張卡片後插入「安全保底提示」
└─
```

### 7.3 AI 推薦入口卡片

```
背景：var(--ai-bg)，邊框：var(--ai-border)
icon：ti-sparkles（紫色）
h4（兩行顯示）：
    有幾個猶豫的校系不知道怎麼排序？
    幫我排志願
（無副標題）
右側：ti-chevron-right
點擊：開啟新增組合命名流程
```

### 7.4 安全保底提示

在第 5 張志願卡片下方插入：

```
背景：var(--warn-bg)；邊框：0.5px solid #EF9F27；圓角：10px
icon：ti-alert-triangle（warn 色）
文字：建議至少填 10 個安全保底志願，增加上榜機會
CTA 按鈕：去落點頁加志願（品牌色邊框，點擊返回落點列表頁）
```

判斷條件：只判斷 `safe` 類別的志願是否 < 10，與 standard 無關。

### 7.5 志願卡片（WishlistCard）

卡片顯示：
1. 錄取機會 badge + 進度點 + 錄取機率 %
2. 序號（大字）+ grip icon（垂直排列）
3. 學校、科系（代碼）
4. 「看動態詳情」連結
5. 若 `appliedInds.length > 0`：下方顯示指標星等雙欄格線

若 `hasBrand: false`：顯示「此校系尚無相關指標的資料，不列入指標排序，建議另行參考學校官方資訊。」

### 7.6 底部 action bar（全部 tab）

單一按鈕：「返回列表」（品牌色，全寬）

---

## 八、組合 tab（ComboTab）

### 8.1 指標來源優先順序

```
combo.appliedInds（組合自己跑過推薦後的指標）
  ↓ 若為空
defaultInds（= WishlistPage 的 allInds）
  ↓ 若仍為空
不顯示指標星等
```

### 8.2 未套用推薦時（entry card）

```
背景：var(--ai-bg)，邊框：var(--ai-border)
icon：ti-sparkles（紫色）
h4：調整排序條件
p：用我在意的學校條件，自動排出適合我的志願順序
右側：ti-chevron-right
點擊：開啟 RecommendFlow（mode='apply'）
```

顯示條件：`appliedInds.length === 0`（combo 本身的 appliedInds 為空，且 defaultInds 也為空）

### 8.3 已套用推薦時（applied state）

版面由上至下：

```
1. [CTA 按鈕]「調整排序條件」（ti-sparkles + 紫色 pill 按鈕）
   - 點擊：開啟 RecommendFlow（mode='apply'）

2. [說明文字]（CTA 下方、列表上方）
   「已用你在意的條件幫你排序，請再自行考量錄取機會與就讀意願，拖拉調整排序。」
   font-size: 12px；color: #aaa；line-height: 1.55

3. [排序依據 ？] 文字按鈕
   - 含 ti-help-circle icon
   - 點擊：開啟指標說明 bottom sheet
   - 樣式：color #bbb，underline，font-size 11px

4. [志願列表]
```

> 注意：CTA 按鈕應在說明文字**上方**，說明文字在列表**上方**。

### 8.4 志願列表

- 有品牌力資料的校系顯示指標星等、可拖拉排序
- 無品牌力資料的校系固定排在最下方，不顯示序號，不可拖拉
- **無 section-label**（移除「志願順序仍需考量…」文字）

### 8.5 移除志願（防呆 Modal）

垃圾桶圖示（`ti-trash`）點擊後，需先顯示確認 Modal：

```
標題：從組合移除？
內文：「{學校} {科系}」將從「{組合名稱}」中移除，全部志願清單不受影響。
按鈕：取消 / 確認移除（紅色 #C0392B）
```

> 與結果頁的「移除此志願」不同，這裡只從組合移除，不影響全部志願。

### 8.6 底部 action bar（組合 tab）

雙按鈕：
- 左：「新增志願」（outline 樣式，flex: 1）— 開啟多選志願 picker
- 右：「調整排序條件」（品牌主色，flex: 1，含 ti-sparkles）— 開啟 RecommendFlow（mode='apply'）

### 8.7 新增志願 Picker（bottom sheet）

多選 bottom sheet，含：
- 標題「選擇要新增的志願」
- 副標：可從 N 個志願中選擇，已選 M 個
- 列表：複選框 + 學校名稱 + 科系 + 錄取機會 badge
- 底部：取消 / 加入 M 個志願（disabled 時 opacity 0.45）

---

## 九、推薦排序流程（RecommendFlow）

### 9.1 三種 mode

| mode | 觸發來源 | 行為 |
|------|---------|------|
| `demo` | 結果頁「試試看」/ 「幫我推薦志願序」 | 以前 5 筆示範校系排序，顯示預覽結果（preview 步驟）；提示登入 |
| `apply` | ComboTab「調整排序條件」| 對組合內校系排序，直接套用並關閉 |
| `all` | WishlistPage 首次進入全部 tab | 只儲存指標順序（allInds），不對校系排序 |

### 9.2 步驟結構（單步驟）

**所有 mode 共用同一步驟**（`step === 1`）：

```
┌─ sheet-header
│   ├─ [無標題]
│   ├─ 副標題：按照你在意的學校條件，依重視程度選出推薦的指標（最多 5 個）
│   └─ 右上角 × 關閉按鈕
├─ sheet-body
│   ├─ 已選區（可拖拉排序，顯示序號圓圈）
│   └─ 可選區（點擊加入）
└─ sheet-footer
    └─ 確認，開始推薦（disabled 當 inds.length < 1）
```

### 9.3 已選指標區

- 顯示「已選 N / 5」說明
- 若 N > 1：顯示「拖拉調整重要性 ↕」提示
- 每項顯示：序號圓圈、指標 icon、指標名稱、grip icon、× 移除按鈕
- `hire` 與 `salary` **永遠**顯示「比較同校不同系的差異」tag（橘色）
- 可用 drag & drop 調整優先順序
- 已選滿 5 個時顯示「已選滿 5 個，拖拉上方指標調整重要性」

### 9.4 可選指標區

- 未被選取的指標以 `ind-card` 樣式列出
- 顯示指標 icon、名稱、說明文字、右側空心選取圈
- `hire` 與 `salary` **永遠**顯示「比較同校不同系的差異」tag
- 已選滿 5 個時，未選指標 opacity 0.4，不可點擊

### 9.5 確認後行為

| mode | 行為 |
|------|------|
| `all` | `onApply([], [...inds], null)` 儲存 allInds；關閉 flow |
| `apply` | 對組合校系依指標加權計分排序；`onApply(sortedIds, inds, comboId)`；關閉 flow |
| `demo` | 對前 5 筆示範校系排序；進入 `step === 'preview'` 預覽 |

### 9.6 預覽步驟（demo mode 專屬）

```
┌─ sheet-header
│   ├─ ← 上一步（回到 step 1）
│   ├─ 標題：推薦志願排序
│   └─ 副標：依序列出已選指標（1. 知名度　2. 學術聲望…）
├─ sheet-body
│   ├─ AI 提示區（紫色）：「以下為前 5 志願示範排序。登入後可依全部志願產生專屬推薦順序。」
│   └─ 排序結果卡片 × n（含序號、學校、錄取機率、指標星等）
└─ sheet-footer
    ├─ 登入，排序我的志願（主要）
    └─ 先不用（次要）
```

### 9.7 計分邏輯

```javascript
const WEIGHTS = [5, 4, 3, 2, 1];

function calcScore(school, orderedInds) {
  return orderedInds.reduce((sum, key, i) => {
    const pr = school.pr[key];
    return sum + (pr !== null && pr !== undefined ? pr : 50) * (WEIGHTS[i] || 1);
  }, 0);
}
```

- PR 值缺失（null）時以 50 代入
- 有品牌力資料（`hasBrand: true`）的校系參與排序
- 無品牌力資料（`hasBrand: false`）的校系固定排在最後，不列入指標排序

---

## 十、指標說明 bottom sheet（「排序依據 ？」）

從 ComboTab 的「排序依據 ？」按鈕（`ti-help-circle` icon）開啟。

### 內容結構

1. **概述段落**：說明 11 大指標與《大學品牌力》
2. **資料來源**：104 人力銀行、企業問卷、教育部資訊公開、QS / THE / CWUR
3. **排名方式**：
   - 各指標依原始值由高而低排序，依常態分布採五等級配分
   - 從你選擇的指標及排序加權後計分（若無排序則不另加權），加權總分愈高、排名愈前
   - **（粗體強調）** 此排序是以「大學品牌力」的角度供參考，實際選填志願序仍需考量個人興趣與錄取機率喔！
4. **各指標說明**：11 項指標各附 icon、名稱、詳細描述
5. **外部連結**：「2025 大學品牌力」完整說明連結

---

## 十一、新增組合流程

1. 點擊 tab bar 末端「+」，或 AllTab 的 AI 入口卡片
2. 開啟命名 bottom sheet（輸入組合名稱，最多允許 10 個組合）
3. 確認後建立組合，自動切換至新組合 tab，並立即開啟新增志願 Picker

---

## 十二、邊界狀態

| 情境 | 處理方式 |
|------|---------|
| 志願數達上限（100） | 愛心按鈕 disabled，加入動作無效 |
| 組合數達上限（10） | tab bar 不顯示「+」按鈕 |
| 組合內無志願 | 空狀態：ti-playlist-x icon + 提示文字 |
| 指標全選（5 個）後繼續點選 | 可選區指標 opacity 0.4，不可點擊 |
| 只選 0 個指標按確認 | 確認按鈕 disabled（opacity 0.5）|

---

## 十三、資料結構

### School

```typescript
interface School {
  id: string;
  name: string;
  dept: string;
  code: string;
  city: string;
  cluster: string;
  interests: string;
  chance: 'dream' | 'reach' | 'standard' | 'safe';
  rate: number;          // 錄取機率 %
  popular: boolean;
  hasBrand: boolean;
  pr: {
    hire: number | null;
    salary: number | null;
    intl: number | null;
    job: number | null;
    future: number | null;
    fame: number | null;
    acad: number | null;
    indus: number | null;
    rd: number | null;
    char: number | null;
    leader: number | null;
  };
}
```

### Combo

```typescript
interface Combo {
  id: string;
  name: string;
  schools: string[];           // 排序後的 school id 陣列
  appliedInds: string[];       // 此組合自己跑推薦後選定的指標順序
  recommendedOrder: string[];  // 上次推薦排序的結果（用於判斷是否偏離）
}
```

### WishlistPage state

```typescript
allInds: string[];          // 全局指標順序（AllTab 流程設定）
showAllIndsFlow: boolean;   // 是否自動觸發 allInds 設定流程
```

---

*文件版本：v6.0｜2026-05-31*
