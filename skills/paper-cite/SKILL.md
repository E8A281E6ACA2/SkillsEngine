---
name: paper-cite
description: 生成多种格式的论文引用（APA, MLA, IEEE, BibTeX）
---

# /paper-cite - 论文引用生成器

自动生成标准格式的学术论文引用，支持多种引用格式（APA, MLA, IEEE, Chicago, BibTeX, GB/T 7714）。

## 功能特性

- 📚 支持多种引用格式
  - APA (7th edition)
  - MLA (9th edition)
  - IEEE
  - Chicago (Author-Date & Notes-Bibliography)
  - BibTeX
  - GB/T 7714（中文国标）
- 🔍 自动从 DOI/arXiv 获取元数据
- 📄 支持多种文献类型（期刊、会议、书籍、网页）
- 📋 批量生成引用列表
- 💾 导出到文件（.bib, .txt, .md）

## 使用方法

```bash
# 基本用法 - 从 DOI 生成引用
/paper-cite 10.1234/example.doi

# 从 arXiv 生成引用
/paper-cite https://arxiv.org/abs/2E8A281E6ACA2.12345

# 指定引用格式
/paper-cite paper.pdf --format apa
/paper-cite paper.pdf --format ieee
/paper-cite paper.pdf --format bibtex

# 生成多种格式
/paper-cite paper.pdf --format all

# 批量生成引用
/paper-cite paper1.pdf paper2.pdf paper3.pdf

# 导出到 BibTeX 文件
/paper-cite paper.pdf --format bibtex --output references.bib

# 从文本信息生成引用
/paper-cite --manual
```

## What You Must Do When Invoked

当用户调用 `/paper-cite` 时，按以下步骤执行：

### Step 1 - 识别输入类型

```bash
INPUT="$1"

if [[ "$INPUT" =~ ^10\. ]]; then
    echo "🔍 Detected DOI"
    INPUT_TYPE="doi"
elif [[ "$INPUT" =~ arxiv.org ]]; then
    echo "📄 Detected arXiv link"
 INPUT_TYPE="arxiv"
elif [[ -f "$INPUT" ]]; then
  echo "📁 Detected local file"
    INPUT_TYPE="file"
elif [[ "$INPUT" == "--manual" ]]; then
    echo "✍️  Manual input mode"
    INPUT_TYPE="manual"
else
    echo "❌ Unknown input type"
    exit 1
fi
```

### Step 2 - 获取论文元数据

根据输入类型获取元数据：

**DOI：**
```bash
# 使用 CrossRef API
curl -s "https://api.crossref.org/works/$DOI" | jq '.message'

# 提取字段
TITLE=$(echo "$METADATA" | jq -r '.title[0]')
AUTHORS=$(echo "$METADATA" | jq -r '.author[] | "\(.given) \(.family)"')
YEAR=$(echo "$METADATA" | jq -r '.published."date-parts"[0][0]')
JOURNAL=$(echo "$METADATA" | jq -r '.["container-title"][0]')
VOLUME=$(echo "$METADATA" | jq -r '.volume')
PAGES=$(echo "$METADATA" | jq -r '.page')
```

**arXiv：**
```bash
# 提取 arXiv ID
ARXIV_ID=$(echo "$INPUT" | grep -oP '(?<=arxiv.org/abs/)\d+\.\d+')

# 使用 arXiv API
curl -s "https://export.arxiv.org/api/query?id_list=$ARXIV_ID" > metadata.xml

# 解析 XML
TITLE=$(xmllint --xpath '//entry/title/text()' metadata.xml)
AUTHORS=$(xmllint --xpath '//entry/author/name/text()' metadata.xml)
YEAR=$(xmllint --xpath '//entry/published/text()' metadata.xml | cut -d'-' -f1)
```

**本地文件：**
```bash
# 尝试从 PDF 元数据提取
# 或提示用户手动输入
```

**手动输入：**
```bash
echo "请输入论文信息："
read -p "标题: " TITLE
read -p "作者 (用逗号分隔): " AUTHORS
read -p "年份: " YEAR
read -p "期刊/会议: " VENUE
read -p "卷号: " VOLUME
read -p "页码: " PAGES
read -p "DOI (可选): " DOI
```

### Step 3 - 生成引用格式

根据 `--format` 参数生成相应格式：

#### APA 格式 (7th edition)

```
作者姓, 名首字母. (年份). 论文标题. 期刊名, 卷号(期号), 页码. https://doi.org/...

示例：
Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., 
Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. In Advances 
in neural information processing systems (pp. 5998-6008).
```

#### MLA 格式 (9th edition)

```
作者姓, 名. "论文标题." 期刊名 卷号.期号 (年份): 页码.

示例:
Vaswani, Ashish, et al. "Attention is all you need." Advances in neural 
information processing systems 30 (2017): 5998-6008.
```

#### IEEE 格式

```
[序号] 名首字母. 姓, "论文标题," 期刊名缩写, vol. 卷号, no. 期号, pp. 页码, 月份 年份.

示例:
[1] A. Vaswani et al., "Attention is all you need," in Advances in Neural 
Information Processing Systems, 2017, pp. 5998-6008.
```

#### Chicago 格式 (Author-Date)

```
作者姓, 名. 年份. "论文标题." 期刊名 卷号 (期号): 页码.

示例:
Vaswani, Ashish, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, 
Aidan N. Gomez, Łukasz Kaiser, and Illia Polosukhin. 2017. "Attention Is 
All You Need." In Advances in Neural Information Processing Systems, 5998-6008.
```

#### BibTeX 格式

```bibtex
@inproceedings{key,
  title={论文标题},
  author={作者1 and 作者2 and 作者3},
  booktitle={会议名},
  pages={页码},
  year={年份}
}

示例:
@inproceedings{vaswani2017attention,
  title={Attention is all you need},
  author={Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and Uszkoreit, Jakob and Jones, Llion and Gomez, Aidan N and Kaiser, {\L}ukasz and Polosukhin, Illia},
  booktitle={Advances in neural information processing systems},
  pages={5998--6008},
  year={2017}
}
```

#### GB/T 7714 格式（中文国标）

```
作者1, 作者2, 作者3. 论文标题[J]. 期刊名, 年份, 卷号(期号): 页码.

示例:
VASWANI A, SHAZEER N, PARMAR N, et al. Attention is all you need[C]//
Advances in neural information processing systems. 2017: 5998-6008.
```

### Step 4 - 格式化输出

```bash
# 根据格式参数输出
FORMAT="${FORMAT:-apa}"  # 默认 APA

case "$FORMAT" in
    apa)
 echo "$APA_CITATION"
     ;;
    mla)
     echo "$MLA_CITATION"
        ;;
    ieee)
        echo "$IEEE_CITATION"
        ;;
    chicago)
        echo "$CHICAGO_CITATION"
  ;;
    bibtex)
 echo "$BIBTEX_CITATION"
        ;;
gbt7714)
        echo "$GBT_CITATION"
        ;;
    all)
        echo "=== APA ==="
 echo "$APA_CITATION"
  echo ""
        echo "=== MLA ==="
      echo "$MLA_CITATION"
        echo ""
        echo "=== IEEE ==="
        echo "$IEEE_CITATION"
        echo ""
        echo "=== BibTeX ==="
     echo "$BIBTEX_CITATION"
    ;;
esac
```

### Step 5 - 导出到文件（可选）

```bash
if [[ -n "$OUTPUT_FILE" ]]; then
    echo "$CITATION" > "$OUTPUT_FILE"
 echo "✅ Citation saved to: $OUTPUT_FILE"
    
    # 如果是 BibTeX，添加到现有文件
    if [[ "$FORMAT" == "bibtex" ]] && [[ -f "$OUTPUT_FILE" ]]; then
        echo "" >> "$OUTPUT_FILE"
        echo "$BIBTEX_CITATION" >> "$OUTPUT_FILE"
    fi
fi
```

### Step 6 - 批量处理

```bash
# 如果有多个输入
if [[ $# -gt 1 ]]; then
    echo "📚 Generating citations for $# papers..."
    echo ""
    
    for i in "$@"; do
        echo "[$((++counter))] Processing: $i"
        # 生成引用
        generate_citation "$i"
 echo ""
    done
    
    echo "✅ All citations generated!"
fi
```

## 示例

### 示例 1：从 DOI 生成 APA 引用

**输入：**
```bash
/paper-cite 10.48550/arXiv.1706.03762 --format apa
```

**输出：**
```
🔍 Detected DOI
📥 Fetching metadata from CrossRef...
✅ Metadata retrieved

=== APA Citation ===
Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., 
Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. In Advances 
in neural information processing systems (pp. 5998-6008). 
https://doi.org/10.48550/arXiv.1706.03762
```

### 示例 2：生成 BibTeX 并保存

**输入：**
```bash
/paper-cite https://arxiv.org/abs/1706.03762 --format bibtex --output references.bib
```

**输出：**
```
📄 Detected arXiv link
📥 Fetching metadata from arXiv API...
✅ Metadata retrieved

=== BibTeX Citation ===
@inproceedings{vaswani2017attention,
  title={Attention is all you need},
  author={Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and others},
  booktitle={Advances in neural information processing systems},
  pages={5998--6008},
  year={2017}
}

✅ Citation saved to: references.bib
```

### 示例 3：生成所有格式

**输入：**
```bash
/paper-cite paper.pdf --format all
```

**输出：**
```
📁 Detected local file
🔍 Extracting metadata from PDF...
✅ Metadata extracted

=== APA ===
Vaswani, A., et al. (2017). Attention is all you need...

=== MLA ===
Vaswani, Ashish, et al. "Attention is all you need."...

=== IEEE ===
[1] A. Vaswani et al., "Attention is all you need,"...

=== BibTeX ===
@inproceedings{vaswani2017attention,
  title={Attention is all you need},
  ...
}

=== GB/T 7714 ===
VASWANI A, SHAZEER N, PARMAR N, et al. Attention is all you need[C]//...
```

### 示例 4：批量生成引用

**输入：**
```bash
/paper-cite paper1.pdf paper2.pdf paper3.pdf --format bibtex --output refs.bib
```

**输出：**
```
📚 Generating citations for 3 papers...

[1] Processing: paper1.pdf
✅ Citation generated

[2] Processing: paper2.pdf
✅ Citation generated

[3] Processing: paper3.pdf
✅ Citation generated

✅ All citations saved to: refs.bib
```

## 注意事项

- DOI 查询依赖 CrossRef API（免费，无需 API key）
- arXiv 查询依赖 arXiv API（免费）
- 对于没有 DOI 的论文，建议使用 `--manual` 手动输入
- BibTeX key 自动生成格式：`姓氏年份关键词`
- 中文论文建议使用 GB/T 7714 格式

## 依赖

- curl（用于 API 请求）
- jq（用于 JSON 解析）
- xmllint（用于 XML 解析，可选）

## 配置

创建 `.paper-cite-config.json`：

```json
{
  "default_format": "apa",
  "output_dir": "./citations",
  "bibtex_key_format": "{author}{year}{keyword}",
  "auto_clipboard": true
}
```

## 相关 Skills

- `/paper-summary` - 论文摘要生成
- `/paper-review` - 论文审查

## 相关资源

- [CrossRef API](https://www.crossref.org/documentation/retrieve-metadata/)
- [arXiv API](https://arxiv.org/help/api)
- [Citation Style Language](https://citationstyles.org/)
