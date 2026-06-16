# EPUB 製作規則（Google Play Books 相容）

> 兩個 Kindle 學習專案共用。2026-06-17 實測通過。

## 五條鐵律（踩過才知道）

1. **副檔名 `.xhtml`**（不是 `.html`）— Play Books 嚴格驗證 XHTML
2. **DOCTYPE 用 XHTML 1.1**（不是 HTML5）
3. **不加 `<meta charset>`** — 用 XML 宣告 `<?xml version="1.0" encoding="UTF-8"?>` 就夠
4. **mimetype 剛好 20 bytes，無換行** — 用 `wb` 模式寫，不能用 Write 工具（會加 `\n`）
5. **ZIP 打包：mimetype 第一個且 `ZIP_STORED`**，其餘 `ZIP_DEFLATED`

## 檔案結構

```
lesson-NN.epub
  mimetype                ← ZIP_STORED，20 bytes
  META-INF/container.xml
  OEBPS/
    content.opf           ← EPUB 2 package
    toc.ncx
    chapterNN.xhtml       ← .xhtml 不是 .html
    styles.css
```

## Python 打包腳本

```python
import zipfile, os

def build_epub(source_dir, output_path):
    with zipfile.ZipFile(output_path, 'w', allowZip64=False) as z:
        info = zipfile.ZipInfo('mimetype')
        info.compress_type = zipfile.ZIP_STORED
        with open(f'{source_dir}/mimetype', 'rb') as f:
            z.writestr(info, f.read())
        for root, dirs, files in os.walk(source_dir):
            dirs.sort()
            for fname in sorted(files):
                if fname == 'mimetype':
                    continue
                fpath = os.path.join(root, fname)
                arcname = os.path.relpath(fpath, source_dir).replace(os.sep, '/')
                z.write(fpath, arcname, compress_type=zipfile.ZIP_DEFLATED)
```

## chapter.xhtml 開頭範本

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="zh-TW">
<head>
  <title>章節標題</title>
  <link rel="stylesheet" type="text/css" href="styles.css"/>
</head>
<body>
<!-- 注意：不加 <meta charset> -->
</body>
</html>
```

## 上傳到 Google Drive

```
mcp__claude_ai_Google_Drive__create_file(
  title="檔名.epub",
  base64Content=<base64>,
  contentMimeType="application/epub+zip",
  disableConversionToGoogleType=True,
  parentId="1vbKdqM5HgMn7e5kK_T-lZdVgREdZ4IJI"
)
```
