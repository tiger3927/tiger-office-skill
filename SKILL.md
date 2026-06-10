---
name: "tiger-office-skill"
description: "办公综合技能工具：Markdown 转 DOCX（含 Mermaid 图表渲染）。无需 API key。"
---



# tiger-office-skill：办公综合技能工具

## 概述

本技能是办公综合工具集，提供 Markdown 与 Office 文档之间的格式转换能力。

### 当前功能

**Pandoc 转换 Markdown 到 DOCX**：将 Markdown 文件（含 Mermaid 图表）通过 Pandoc 转换为 DOCX 文档，根据操作系统自动选择正确的 mermaid-filter 命令。

## 前提条件

| 工具 | 安装方式 |
|------|----------|
| Pandoc | 从 https://github.com/jgm/pandoc/releases 下载，并设置 PATH |
| mermaid-cli | `npm install -g @mermaid-js/mermaid-cli` |
| mermaid-filter | `npm install -g mermaid-filter` |

## 检测安装

在调用转换前，先检测工具是否可用，只能用如下命令检查：

### Windows

```powershell
pandoc --version
npm show mermaid-filter
```

### Linux/macOS

```bash
pandoc --version
npm show mermaid-filter
```

## 转换命令

### 单文件转换

根据操作系统执行对应的命令：

**Windows：**

```powershell
pandoc "<input.md>" -o "<output.docx>" --filter mermaid-filter.cmd
```

**Linux/macOS：**

```bash
pandoc "<input.md>" -o "<output.docx>" --filter mermaid-filter
```

### 批量转换

当需要批量转换一个目录下的多个 Markdown 文件时：

**Windows：**

```powershell
Get-ChildItem "<目录路径>" -Filter *.md | ForEach-Object {
    $output = $_.FullName -replace '\.md$', '.docx'
    pandoc $_.FullName -o $output --filter mermaid-filter.cmd
    Write-Host "已转换: $($_.Name)"
}
```

**Linux/macOS：**

```bash
for f in <目录路径>/*.md; do
    output="${f%.md}.docx"
    pandoc "$f" -o "$output" --filter mermaid-filter
    echo "已转换: $(basename "$f")"
done
```

### 带额外选项的转换

```powershell
# Windows
pandoc "<input.md>" -o "<output.docx" `
    --filter mermaid-filter.cmd `
    --from markdown --to docx `
    --metadata title="文档标题"

# Linux/macOS
pandoc "<input.md>" -o "<output.docx" \
    --filter mermaid-filter \
    --from markdown --to docx \
    --metadata title="文档标题"
```

## 使用说明

1. 用户指定要转换的 Markdown 文件路径
2. 技能先检测 pandoc 和 mermaid-filter 是否可用
3. 根据操作系统选择正确的 mermaid-filter 命令
4. 执行转换命令
5. 返回生成的 DOCX 文件路径

## pandoc 和 filter 的安装

### Pandoc

从 GitHub Releases 下载并安装，然后设置 PATH。

- 下载地址：https://github.com/jgm/pandoc/releases
- 安装后确保 `pandoc` 命令可在终端中直接调用

只能用这个方式验证安装：

```powershell
# Windows
pandoc --version

# Linux/macOS
pandoc --version

```

### Mermaid Filter（Mermaid 图表渲染）

需要安装两个 npm 包：

```bash
npm install -g @mermaid-js/mermaid-cli
npm install -g mermaid-filter
```

只能用这个方式验证安装：

```powershell
# Windows
npm show mermaid-filter

# Linux/macOS
npm show mermaid-filter
```


## 注意事项

- Mermaid 图表在 Markdown 中使用标准的 ` ```mermaid ` 代码块编写
- 生成的 DOCX 文件默认与输入文件同目录，可通过 `-o` 参数指定输出路径
- 本技能仅提供 Pandoc 转换功能，不包含其他 Office 转换

## Mermaid 版本兼容性

当前 mermaid-filter 依赖的 mermaid 版本为 `10.9.6`及以上，**不支持**以下图表类型（需要 mermaid ≥ 11）：

| 不支持的类型 | Mermaid 关键字 | 替代方案 |
|------------|---------------|---------|
| 象限图 | `quadrantChart` | 改用 `flowchart` + `subgraph` 模拟象限结构 |
| C4 模型图 | `C4Context` / `C4Container` | 改用 `flowchart` + `subgraph` 实现系统分层 |

支持的图表类型（mermaid 10.9.6 原生支持）：

`flowchart` · `sequenceDiagram` · `classDiagram` · `stateDiagram-v2` · `erDiagram` · `gantt` · `pie` · `mindmap` · `timeline` · `gitGraph`