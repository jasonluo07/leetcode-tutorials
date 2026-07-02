# LeetCode 互動教學

一組面試前快速複習用的**互動式演算法視覺化**，純靜態 HTML、零框架、零 build，透過 GitHub Pages 公開，任何裝置（含手機 Anki）都能開。

線上：<https://jasonluo07.github.io/leetcode-tutorials/>

## 結構

```
index.html            首頁，列出全部 tutorial
css/tokens.css        共用設計系統（design tokens + 元件）
tutorials/*.html      各題互動教學（link ../css/tokens.css，只放該題獨有視覺化）
AUTHORING.md          撰寫契約（新增 tutorial 照這個寫）
.nojekyll             關掉 GitHub Pages 的 Jekyll 處理
```

## 新增一個 tutorial

1. 照 `AUTHORING.md` 寫 `tutorials/<slug>.html`（`<head>` link `../css/tokens.css`，只寫該題獨有樣式）。
2. 在 `index.html` 加一張卡片。
3. commit、push，等 Pages 更新。
4. 若對應到 Anki 卡片，把該題筆記 frontmatter 的 `notes` 指向公開網址，再跑 `rebuild_anki.py`。
