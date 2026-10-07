---
name: paper-summary
description: 自动总结论文要点，提取关键信息
---

# /paper-summary - 论文摘要生成器

自动分析学术论文（PDF、arXiv 链接或文本），提取并总结关键信息，包括研究目的、方法、结果和结论。

## 功能特性

- 📄 支持多种输入格式（PDF 文件、arXiv 链接、文本粘贴）
- 🎯 智能提取论文结构（摘要、引言、方法、结果、结论）
- 📊 生成结构化摘要（中英文）
- 🔍 识别关键贡献和创新点
- 📝 提取核心方法和技术细节
- 💡 生成研究启发和应用场景
- 🏷️ 自动标注研究领域和关键词

## 使用方法

```bash
# 基本用法 - 分析当前目录的 PDF
/paper-summary paper.pdf

# 从 arXiv 获取并总结
/paper-summary https://arxiv.org/abs/2E8A281E6ACA2.12345

# 指定输出语言
/paper-summary paper.pdf --lang zh
/paper-summary paper.pdf --lang en

# 详细模式（包含更多细节）
/paper-summary paper.pdf --detailed

# 生成 Markdown 报告
/paper-summary paper.pdf --output summary.md

# 对比模式（对比多篇论文）
/paper-summary paper1.pdf paper2.pdf --compare
```

## What You Must Do When Invoked

当用户调用 `/paper-summary` 时，按以下步骤执行：

### Step 1 - 识别输入类型

```bash
# 检查输入是文件、URL 还是文本
INPUT="$1"

if [[ "$INPUT" =~ ^https?://arxiv.org ]]; then
    echo "📥 Detected arXiv link"
    INPUT_TYPE="arxiv"
elif [[ -f "$INPUT" ]]; then
    echo "📄 Detected local file: $INPUT"
    INPUT_TYPE="file"
elif [[ "$INPUT" =~ ^https?:// ]]; then
    echo "🔗 Detected URL"
    INPUT_TYPE="url"
else
    echo "📝 Treating as text input"
    INPUT_TYPE="text"
fi
```

### Step 2 - 读取论文内容

根据输入类型读取内容：

**arXiv 链接：**
```bash
# 提取 arXiv ID
ARXIV_ID=$(echo "$INPUT" | grep -oP '(?<=arxiv.org/abs/)\d+\.\d+')

# 获取论文元数据
curl -s "https://export.arxiv.org/api/query?id_list=$ARXIV_ID" > metadata.xml

# 下载 PDF（可选）
wget "https://arxiv.org/pdf/$ARXIV_ID.pdf" -O paper.pdf
```

**本地 PDF 文件：**
```bash
# 使用 Read 工具读取 PDF
# Claude Code 支持直接读取 PDF
```

**URL：**
```bash
# 使用 WebFetch 工具获取内容
```

### Step 3 - 分析论文结构

识别论文的主要部分：

1. **标题和作者**
2. **摘要 (Abstract)**
3. **引言 (Introduction)**
4. **相关工作 (Related Work)**
5. **方法 (Method/Approach)**
6. **实验 (Experiments)**
7. **结果 (Results)**
8. **讨论 (Discussion)**
9. **结论 (Conclusion)**
10. **参考文献 (References)**

### Step 4 - 提取关键信息

分析并提取以下内容：

**研究背景：**
- 研究领域
- 要解决的问题
- 现有方法的局限性

**核心贡献：**
- 主要创新点（通常 2-4 个）
- 与现有工作的区别

**方法论：**
- 核心技术/算法
- 模型架构
- 关键参数

**实验设置：**
- 数据集
- 评估指标
- 基线方法

**主要结果：**
- 性能数据
- 与基线的对比
- 消融实验结果

**结论和启发：**
- 主要发现
- 局限性
- 未来工作方向

### Step 5 - 生成结构化摘要

根据 `--lang` 参数生成中文或英文摘要：

```markdown
# 📄 论文摘要

## 基本信息
- **标题**: [论文标题]
- **作者**: [作者列表]
- **发表**: [会议/期刊, 年份]
- **链接**: [arXiv/DOI]

## 🎯 研究目标
[1-2 句话描述研究目的]

## 💡 核心贡献
1. [贡献点 1]
2. [贡献点 2]
3. [贡献点 3]

## 🔬 方法概述
[3-5 句话描述核心方法]

### 关键技术
- [技术点 1]
- [技术点 2]

## 📊 实验结果
- **数据集**: [数据集名称]
- **主要指标**: [指标名称]
- **性能**: [具体数值]
- **对比**: [与基线的对比]

## 🔍 关键发现
1. [发现 1]
2. [发现 2]

## ⚠️ 局限性
- [局限性 1]
- [局限性 2]

## 🚀 未来方向
- [方向 1]
- [方向 2]

## 🏷️ 关键词
[关键词1], [关键词2], [关键词3]

## 💭 个人评价
[可选：你对这篇论文的评价和思考]
```

### Step 6 - 输出结果

根据参数决定输出方式：

**默认输出（终端）：**
```bash
# 直接在终端显示摘要
echo "$SUMMARY"
```

**保存到文件：**
```bash
# 如果指定了 --output
if [[ -n "$OUTPUT_FILE" ]]; then
  echo "$SUMMARY" > "$OUTPUT_FILE"
    echo "✅ Summary saved to: $OUTPUT_FILE"
fi
```

**对比模式：**
```bash
# 如果是对比模式，生成对比表格
if [[ "$COMPARE" == "true" ]]; then
    # 生成对比表格
    echo "| 维度 | 论文1 | 论文2 |"
    echo "|------|-------|-------|"
    # ... 填充对比内容
fi
```

### Step 7 - 可选：生成引用

如果用户需要，自动生成引用格式：

```bash
# 提示用户是否需要生成引用
echo ""
echo "💡 需要生成引用格式吗？(y/n)"
read -r NEED_CITE

if [[ "$NEED_CITE" == "y" ]]; then
    # 调用 /paper-cite skill
    /paper-cite "$INPUT"
fi
```

## 示例

### 示例 1：总结本地 PDF

**输入：**
```bash
/paper-summary attention-is-all-you-need.pdf
```

**输出：**
```markdown
# 📄 论文摘要

## 基本信息
- **标题**: Attention Is All You Need
- **作者**: Vaswani et al.
- **发表**: NeurIPS 2017
- **链接**: https://arxiv.org/abs/1706.03762

## 🎯 研究目标
提出一种完全基于注意力机制的序列转换模型 Transformer，
摆脱传统的循环和卷积结构。

## 💡 核心贡献
1. 提出 Transformer 架构，完全基于自注意力机制
2. 引入多头注意力（Multi-Head Attention）机制
3. 在机器翻译任务上达到 SOTA，且训练速度更快

## 🔬 方法概述
Transformer 使用编码器-解码器架构，完全依赖自注意力机制
来计算输入和输出的表示。模型使用位置编码来保留序列顺序信息，
通过多头注意力捕获不同位置的依赖关系。

### 关键技术
- 自注意力机制（Self-Attention）
- 多头注意力（Multi-Head Attention）
- 位置编码（Positional Encoding）
- 残差连接和层归一化

## 📊 实验结果
- **数据集**: WMT 2014 English-German, English-French
- **主要指标**: BLEU score
- **性能**: EN-DE: 28.4 BLEU, EN-FR: 41.8 BLEU
- **对比**: 超越所有之前的模型，训练时间减少 10 倍

## 🔍 关键发现
1. 注意力机制足以建模序列依赖，无需 RNN/CNN
2. 并行化训练显著提升效率
3. 模型可解释性强（可视化注意力权重）

## ⚠️ 局限性
- 对于非常长的序列，计算复杂度为 O(n²)
- 需要大量数据才能充分训练

## 🚀 未来方向
- 探索更高效的注意力机制（如稀疏注意力）
- 应用到其他序列任务（如图像、音频）

## 🏷️ 关键词
Transformer, Attention, Neural Machine Translation, Deep Learning

✅ Summary complete! Use /paper-cite to generate citations.
```

### 示例 2：从 arXiv 总结

**输入：**
```bash
/paper-summary https://arxiv.org/abs/2E8A281E6ACA2.12345 --lang zh --output summary.md
```

**输出：**
```
📥 Detected arXiv link
📄 Fetching paper metadata...
✅ Paper downloaded: 2E8A281E6ACA2.12345.pdf
🔍 Analyzing paper structure...
📝 Generating summary in Chinese...
✅ Summary saved to: summary.md

💡 需要生成引用格式吗？(y/n)
```

### 示例 3：对比多篇论文

**输入：**
```bash
/paper-summary bert.pdf gpt.pdf --compare
```

**输出：**
```markdown
# 📊 论文对比

| 维度 | BERT | GPT |
|------|------|-----|
| **模型类型** | 双向 Transformer 编码器 | 单向 Transformer 解码器 |
| **预训练任务** | MLM + NSP | 语言建模 |
| **优势** | 理解上下文，适合分类 | 生成能力强 |
| **应用场景** | 问答、分类、NER | 文本生成、对话 |
| **参数量** | 110M (base), 340M (large) | 117M (GPT-1) |
```

## 注意事项

- 对于非英文论文，可能需要额外的语言处理
- PDF 解析质量取决于 PDF 格式（扫描版可能效果较差）
- 对于超长论文（>50页），建议使用 `--detailed` 模式
- arXiv 下载需要网络连接

## 依赖

- Claude Code 1.0+（支持 PDF 读取）
- curl（用于 arXiv API）
- wget（用于下载 PDF，可选）

## 配置

可在项目根目录创建 `.paper-summary-config.json`：

```json
{
  "default_lang": "zh",
  "output_dir": "./summaries",
  "auto_cite": true,
  "detailed_mode": false,
  "save_pdf": true
}
```

## 相关 Skills

- `/paper-cite` - 生成论文引用
- `/paper-translate` - 翻译论文
- `/paper-review` - 论文审查

## 相关资源

- [arXiv API](https://arxiv.org/help/api)
- [Semantic Scholar API](https://www.semanticscholar.org/product/api)
