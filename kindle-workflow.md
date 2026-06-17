# Kindle 書籍學習工作流（兩專案共用）

## 互動式學習模式（鐵律）

- **一題一答**：出一道題 → 等回答 → 給回饋 → 才出下一題
- **禁止**：一次把全部答案寫完
- 原因：互動作答的學習效果遠優於直接看解答

## 每課完成後的標準流程

```
1. 儲存解答到 answer/
2. 製作 EPUB（見 epub-techniques.md）
3. carry：把 answer/ 複製到下一課 starter/
4. git commit + push
```

carry 指令格式：

```bash
cp -r demo/[NN-課程名]/answer/. demo/[NN+1-課程名]/starter/
```

## 解答命名規則

| 練習 | 解答檔名 |
|------|---------|
| exercise-01 | answer/ex01-answer.md |
| exercise-02 | answer/ex02-answer.md |
| exercise-03 | answer/ex03-answer.md |

## EPUB 命名規則

| 專案 | 格式 |
|------|------|
| Kindle 19（Claude Code Pro） | `epub/lesson-NN-claude-code-pro.epub` |
| Kindle 20（65 Automation Patterns） | `epub/chapter-NN-65-patterns.epub` |

## writer-qa-iron-rule 豁免

學習練習的 .py 範例不需走完整 writer → QA → reviewer 流程。
繞過方式：Bash + Python 內嵌（`python -c "..." > file.py`）
或：`FORCE_DIRECT_WRITE=1` 環境變數（用完即撤）
