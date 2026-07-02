# Tutorial 撰寫契約

每個 tutorial 是一個獨立、自包含（除了共用 CSS）的 HTML 檔，放在 `tutorials/`。目標：面試前快速複習用的**互動式演算法視覺化**。語言 zh-TW（美式英文技術詞），深色主題。

## 硬性規則

1. **樣式**：`<head>` 一定要 `<link rel="stylesheet" href="../css/tokens.css">`（相對路徑，不要用絕對 `/css/`）。
2. **顏色一律用 `var(--token)`**，不要在頁內重新定義色碼。頁內 `<style>` **只放該題獨有的視覺化 layout**（如陣列磚塊、籃子、指標排列），共用元件（卡片、按鈕、code block、callout、表格、log/status、accordion）已在 tokens.css，直接用其 class。
3. **零外部依賴**：不 import 任何 CDN、字型、JS 套件、圖片。純 HTML + inline `<style>` + inline `<script>`。要能離線、單檔開啟就能跑。
4. **手機可用**：`<meta name="viewport" content="width=device-width, initial-scale=1.0">`。視覺化容器用 flex + `flex-wrap`，窄螢幕不爆版、不需橫向捲動（程式碼 `<pre>` 例外，可自己橫捲）。這些會從 Anki 手機版點開。
5. **不要觸發 alert/confirm/prompt**。除錯用 `console.log`。
6. **互動要能重複操作**：至少「下一步」逐步推進 + 「重設」歸零；步驟走到底要 disable 下一步並顯示完成。狀態機要乾淨，reset 後能重跑。

## 頁面骨架

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>題目名 互動教學</title>
  <link rel="stylesheet" href="../css/tokens.css">
  <style>/* 只放該題獨有的視覺化樣式 */</style>
</head>
<body>
  <div class="container">
    <h1>題目名</h1>
    <p class="subtitle">一句話講這題在幹嘛</p>

    <div class="section">
      <h2><span class="step-num">1</span>核心觀察</h2>
      ...
    </div>

    <div class="section">
      <h2><span class="step-num">2</span>視覺化過程</h2>
      ... 互動區（.controls + 視覺化容器 + .log 或 .status）...
    </div>

    <div class="section">
      <h2><span class="step-num">3</span>程式碼</h2>
      <pre>...語法上色的 Python...</pre>
      <div class="note"><strong>最容易踩的雷</strong>：...</div>
    </div>

    <div class="section">
      <h2><span class="step-num">4</span>複雜度</h2>
      <table class="cx">...</table>
    </div>
  </div>
  <script>/* 視覺化狀態機 */</script>
</body>
</html>
```

章節數量與內容依題目調整（例如 Binary Search 有「眉角逐一拆解」用多個 `<details>` accordion）。骨架精神固定：**核心觀察 → 互動視覺化 → 程式碼(含踩雷) → 複雜度**。

## 可用的共用元件（都在 tokens.css）

- **卡片**：`.section` + `.section h2` 內含 `.step-num` 圓形編號徽章。次標題 `<h3>`。
- **文字**：`code`（行內）、`.mono`、`strong`/`b`、`.hl`（琥珀強調）、`.muted`、`.phase-label`（視覺化階段小標）。
- **程式碼**：`<pre>` + span class `.kw`（關鍵字）`.fn`（函式/內建）`.str`/`.num`（字面值）`.op`（運算子）`.cm`（註解）。逐行 highlight 用 `.ln` / `.ln.active`（互動標示執行行）。
- **callout**：`.note`（琥珀，放踩雷）、`.key`（藍，放核心心智模型/不變量）。
- **控制**：`.controls` 容器；按鈕 `.btn`（主）、`.btn.ghost`（次）、`.btn-reset`（重設）；`input[type=number]`。
- **狀態**：`.log`（等寬逐步日誌，內含 `.ok`/`.new`/`.stop` 上色）、`.status`（一般狀態框，變體 `.status.found`/`.status.done`）。
- **表格**：`table.cx`（兩欄複雜度表，左欄等寬藍字）；`table.grid`（一般置中格線表）。
- **深入**：`<details><summary>…</summary><div class="body">…</div></details>` accordion。
- **標籤**：`.tag.good` / `.tag.bad`。
- **指標徽章**：`.ptr.l` / `.ptr.r` / `.ptr.m`（陣列類視覺化）。指標色 token `--ptr-l/-r/-m`。
- **動畫**：`@keyframes pop` 已定義，新元素進場可 `animation: pop .3s ease`。

## 程式碼區塊慣例

- 用 Python，貼近對應 LeetCode 筆記的解法，語法用 span 上色。
- 邏輯清楚、語言中立優先：密集慣用法（list comprehension 過濾+轉換）展開成明確迴圈；`dict.get(k,0)`、`[::-1]` 這種單一動作慣用法可留。
- 程式碼要能對得上視覺化每一步在做什麼。

## 產出要求

回傳**完整單一 HTML 檔內容**（從 `<!DOCTYPE html>` 到 `</html>`），可直接存檔在 `tutorials/` 下、配 `../css/tokens.css` 就能開。
