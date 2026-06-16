# shared-kindle

跨專案共用規則與資源倉庫。供所有 Kindle 書籍學習專案引用。

## 用途

各 Kindle 學習專案（Kindle 19、20、21…）的 CLAUDE.md 透過 `@` 語法引用本倉庫的檔案，
讓 Claude Code CLI 在每個 session 自動載入共同規則，不需在各專案重複維護。

## 目前收錄

| 檔案 | 內容 |
|------|------|
| `epub-techniques.md` | EPUB 2 製作規則（Google Play Books 相容，五條鐵律） |
| `kindle-workflow.md` | 每課標準流程（Q&A → 儲存 → EPUB → carry → commit） |

## 如何在新專案接入

### Step 1：確認本倉庫在本機的位置

```bash
# 若尚未 clone，執行：
git clone https://github.com/chenghyang2001/shared-kindle.git ~/workspace/shared-kindle
```

本倉庫固定放在 `~/workspace/shared-kindle/`，不要移動位置。

### Step 2：在新專案的 CLAUDE.md 末尾加上

```markdown
## 共用規則（shared-kindle）
@../shared-kindle/epub-techniques.md
@../shared-kindle/kindle-workflow.md
```

路徑說明：`../shared-kindle/` 是從 `~/workspace/<你的專案>/` 往上一層再進入 `shared-kindle/`。
只要所有專案都放在 `~/workspace/` 下，這個相對路徑就永遠有效。

### Step 3：更新本倉庫

當規則需要修改時，只改本倉庫的檔案，所有引用它的專案 session 自動生效。

```bash
cd ~/workspace/shared-kindle
# 修改檔案後：
git add .
git commit -m "更新說明"
git push
```

## 擴充計畫

本倉庫未來可依需求新增：

```
shared-kindle/
  epub-techniques.md      ✅ 現有
  kindle-workflow.md      ✅ 現有
  python-rules.md         （Python 專案共用規則）
  web-rules.md            （前端/Next.js 共用規則）
  templates/
    CLAUDE.md.example     （新 Kindle 專案的 CLAUDE.md 範本）
    chapter.xhtml         （EPUB 章節 XHTML 範本）
```

新增檔案後，在需要的專案 CLAUDE.md 加 `@` 引用即可，不影響其他專案。

## 跨機器同步

在不同電腦上使用時：

1. `git clone` 本倉庫到 `~/workspace/shared-kindle/`
2. 各學習專案也 `git clone` 到 `~/workspace/`
3. 相對路徑 `../shared-kindle/` 在所有機器上自動有效

## 目前引用本倉庫的專案

| 專案 | 路徑 | 書名 |
|------|------|------|
| kindle-19-claude-code-pro | `~/workspace/kindle-19-claude-code-pro/` | Claude Code Pro（Ethan Cole） |
| kindle-20-65-automation-patterns | `~/workspace/kindle-20-65-automation-patterns/` | Claude Code in Production（Yosuke Morikawa） |
