---
id: papermod-dependency-review
title: "themes/PaperMod 依賴關係與優化建議(2026-11-24 專案檢查)"
category: reference
status: active
tags: [papermod, hugo, theme, optimization, review]
created: "2026-11-25T07:32:37"
updated: "2026-11-25T07:32:37"
---

## compiled_truth

本頁是 2026-11-24 專案檢查的完整記錄。檢查只做讀取,沒有改任何檔案。待辦摘要與進度見 [[optimization-backlog]]。

## `themes/PaperMod` 與本專案的依賴關係

**版本落後:** submodule 固定在 `5a46517`(v8.0-32),上游 `origin/master` 是 v8.0-152,落後 **120 個 commit**。落後的內容有三類。

1. **目錄結構改了:** `layouts/_default/*` 搬到 `layouts/` 根目錄,`layouts/partials/*` 改名為 `layouts/_partials/*`,`shortcodes/` 改名為 `_shortcodes/`。上游新增了 `layouts/llms.txt`、`layouts/index.json` 和 `taxonomy.html`。
2. **最低 Hugo 版本提高:** 上游 `theme.toml` 的 `min_version` 是 0.158.0,目前固定版本是 0.146.0。本機與 CI 的 Hugo 是 0.166.0,所以升級後仍符合。
3. **有修正與效能改進:** 其中 `a5d59c7` 是 Hugo 0.158 deprecation 的修正,應該能清掉建置時那兩行 WARN(`.Language.LanguageDirection`、`.Language.LanguageCode`)。只確認了現況有這兩行,沒有試過升級後是否消失。

## 覆寫檔案的風險

專案有 9 個檔案整份取代主題檔。與目前固定版本相比,有 8 個和主題不同,`layouts/partials/share_icons.html` 是例外。

| 檔案 | 與固定版本的差異 | 上游有改動 |
|---|---|---|
| `layouts/_default/list.html` | +17 −12 | 已搬到 `layouts/list.html`(+1/−1) |
| `layouts/_default/single.html` | +3 −3 | 已搬到 `layouts/single.html`(+2/−2) |
| `layouts/robots.txt` | +41 −6 | 沒改動 |
| `layouts/partials/header.html` | +32 −2 | 搬到 `_partials/` 並有改動 |
| `layouts/partials/index_profile.html` | +85 −49 | 搬到 `_partials/`,有改動 |
| `layouts/partials/share_icons.html` | **和主題完全相同** | 搬到 `_partials/` |
| `layouts/partials/post_nav_links.html` | +43 −4 | 搬到 `_partials/` 並有改動 |
| `layouts/partials/extend_head.html` | +11 −4 | 搬到 `_partials/`,有改動 |
| `assets/css/common/post-single.css` | +12 −6 | 上游大幅重寫(+68/−263) |

- 專案的檔案還放在舊路徑。預期升級後 Hugo 的相容機制仍會讓專案檔優先,但沒有實際驗證。
- 專案的檔案會一直停在舊版,拿不到上游的修正,這是長期的維護成本。
- `post-single.css` 差異最大。AGENTS.md 說「只有分享按鈕規則是刻意差異」,但上游已經改了很多,直接重套那幾條規則的風險最高。

## 建議的優化項目

1. **刪除 `layouts/partials/share_icons.html`。** 它和主題完全一樣,沒有作用,卻會擋住主題之後的更新。同時從 AGENTS.md 的覆寫清單移除。
2. **升級 PaperMod。** 先在 `.worktrees/papermod-revisions` 上做,逐一比對上面的覆寫檔,再跑 `hugo --minify`,確認 WARN 消失。
3. **縮小覆寫範圍。** `extend_head.html` 是主題提供的 hook,保留它很合理。`header.html`、`post_nav_links.html`、`single.html`、`list.html` 和 `post-single.css` 可以評估能否改用 `assets/css/extended/` 或其他 hook,不再整份覆寫。
4. **CI 的 Dart Sass 步驟看起來用不到。** 專案裡沒有 `.scss` 或 `.sass` 檔,`snap install dart-sass` 可以省掉。同一份 workflow 的 `Install Node.js dependencies` 在沒有 `package-lock.json` 時也什麼都不做,可以一併移除。這兩項是用搜尋判斷的,移除前最好在 CI 實際跑一次。
5. **CI 下載 Hugo 沒有校驗。** 用 `wget` 抓 `.deb` 後直接安裝。可以改用 `peaceiris/actions-hugo` 之類的 action,或加上 checksum 驗證。
6. **Hugo 版本寫了三處。** `deploy2pages.yml`、`README.md` 的 mermaid 圖和 AGENTS.md 都有。可以考慮讓 README 少寫具體版號,避免下次升級又要同步。
7. **靜態資源較大。** `static/pdf` 有 11 個檔案,共 21M,`static/` 總共 22M。可以檢查是否有未引用或可壓縮的 PDF。
8. **已知問題:** AGENTS.md 提到 `llms-full.txt` 會出現 `Venue: Venue:`。這可以在 `layouts/index.llmsfull.txt` 修掉。
9. **版本控管:**
   - `.bob/` 和 `AGENTS.md` 當時未追蹤,決定要不要 commit。(後續已處理:`AGENTS.md` 已 commit,`.bob/` 已加入 `.gitignore`。)
   - 當時在 `papermod` 分支,`main` 才會部署。升級主題前請先確認分支策略。


## timeline

- time: 2026-11-25T07:32:37
  kind: decision
  summary: "Created this page: themes/PaperMod 依賴關係與優化建議(2026-11-24 專案檢查)"
  source: "Project review on 2026-11-24 (read-only investigation)"
  affects: [papermod-dependency-review]

- time: 2026-11-25T07:32:37
  kind: decision
  summary: "Record the full 2026-11-24 review text: PaperMod dependency analysis and recommended optimizations"
  source: Project review on 2026-11-24
  affects: [papermod-dependency-review]
