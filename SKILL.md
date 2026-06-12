---
name: "tiger-office-skill"
description: "办公综合技能工具：Markdown 转 DOCX/PPTX（含 Mermaid 图表渲染）+ 视频号起流量指南。无需 API key。"
---

# tiger-office-skill：办公综合技能工具

## 概述

本技能是办公综合工具集，提供 Markdown 与 Office 文档之间的格式转换能力。

### 当前功能

| 功能 | 说明 |
|------|------|
| **Markdown 转 DOCX** | 将 Markdown 文件（含 Mermaid 图表）通过 Pandoc 转换为 DOCX 文档 |
| **Markdown 转 PPTX** | 将 Markdown 文件（含 Mermaid 图表）通过 Pandoc 转换为 PPTX 演示文稿 |
| **视频号起流量指南** | 从算法原理到每天执行的完整方法论，含社交裂变实操、冷启动7天清单、数据复盘框架 |

---

## 一、环境检查与安装

### 1.1 Pandoc

Pandoc 是文档转换的核心引擎。

**安装：** 从 [GitHub Releases](https://github.com/jgm/pandoc/releases) 下载并安装，确保 `pandoc` 在 PATH 中。

**检查安装（只能用以下方式）：**

```powershell
# Windows
pandoc --version

# Linux/macOS
pandoc --version
```

### 1.2 Mermaid Filter（Mermaid 图表渲染）

用于在 DOCX/PPTX 中渲染 ` ```mermaid ` 图表为图片。

**安装：**

```bash
npm install -g @mermaid-js/mermaid-cli
npm install -g mermaid-filter
```

**检查安装（只能用以下方式）：**

```powershell
# Windows
npm show mermaid-filter

# Linux/macOS
npm show mermaid-filter
```

### 1.3 Mermaid 版本兼容性

当前 mermaid-filter 依赖的 mermaid 版本为 `10.9.6` 及以上，**不支持**以下图表类型（需要 mermaid ≥ 11）：

| 不支持的类型 | Mermaid 关键字 | 替代方案 |
|------------|---------------|---------|
| 象限图 | `quadrantChart` | 改用 `flowchart` + `subgraph` 模拟象限结构 |
| C4 模型图 | `C4Context` / `C4Container` | 改用 `flowchart` + `subgraph` 实现系统分层 |

**支持的图表类型（mermaid 10.9.6 原生支持）：**

`flowchart` · `sequenceDiagram` · `classDiagram` · `stateDiagram-v2` · `erDiagram` · `gantt` · `pie` · `mindmap` · `timeline` · `gitGraph`

---

## 二、Pandoc 转换 Markdown 到 DOCX

### 概述

将 Markdown 文件（含 Mermaid 图表）转换为 DOCX 文档。

### 转换命令

#### 单文件转换

根据操作系统执行对应的命令：

**Windows：**

```powershell
pandoc "<input.md>" -o "<output.docx>" --filter mermaid-filter.cmd
```

**Linux/macOS：**

```bash
pandoc "<input.md>" -o "<output.docx>" --filter mermaid-filter
```

#### 批量转换

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

#### 带额外选项的转换

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

### 使用说明

1. 用户指定要转换的 Markdown 文件路径
2. 技能先检测 pandoc 和 mermaid-filter 是否可用（见第一章）
3. 根据操作系统选择正确的 mermaid-filter 命令
4. 执行转换命令
5. 返回生成的 DOCX 文件路径

### 注意事项

- Mermaid 图表在 Markdown 中使用标准的 ` ```mermaid ` 代码块编写
- 生成的 DOCX 文件默认与输入文件同目录，可通过 `-o` 参数指定输出路径
- 本功能仅提供 Pandoc 转换，不包含其他 Office 转换

---

## 三、Pandoc 转换 Markdown 到 PPTX

### 概述

将 Markdown 文件（含 Mermaid 图表）转换为 PPTX 演示文稿。

### 指导文档

详细教程请参阅：[Markdown 转 PPTX 指南](docs/Markdown转PPTX专项指南.md)

该指南涵盖：
- 快速入门与基本命令
- Markdown 幻灯片语法（分页、YAML 元数据、Slide Level）
- 样式定制（reference-doc 模板制作）
- 高级布局技巧（多列、演讲者备注、增量列表、图片、表格）
- Mermaid 图表支持
- 完整示例模板
- 常见问题排查
- 工具对比与选型
- 完整转换脚本（单文件 + 批量）

### 转换命令

**Windows：**

```powershell
pandoc "<input.md>" -o "<output.pptx>" --slide-level=2 --filter mermaid-filter.cmd
```

**Linux/macOS：**

```bash
pandoc "<input.md>" -o "<output.pptx>" --slide-level=2 --filter mermaid-filter
```

### 使用说明

1. 用户指定要转换的 Markdown 文件路径
2. 技能先检测 pandoc 和 mermaid-filter 是否可用（见第一章）
3. 根据操作系统选择正确的 mermaid-filter 命令
4. 执行转换命令
5. 返回生成的 PPTX 文件路径

### 注意事项

- PPTX 的幻灯片分隔方式与 DOCX 不同，详见指导文档
- 图片和表格在 PPTX 中会独占新幻灯片，如需图文同页可使用 Two Content 布局
- 如需自定义样式，需先制作 `--reference-doc` 模板

---

## 四、视频号起流量指南

### 概述

提供视频号从算法原理到每天执行的完整方法论，帮助用户理解视频号独特的社交推荐机制，掌握冷启动、内容创作、数据复盘等核心技能。

### 指导文档

详细教程请参阅：[视频号起流量指南](docs/微信视频号起流量指南.md)

该指南涵盖：
- 核心认知：视频号 vs 抖音的本质差异
- 算法底层逻辑：社交推荐 + 算法推荐双引擎驱动
- 流量池分级机制与考核指标
- 账号权重体系与基建清单
- 内容创作方法论（选题、3秒开头、视频结构、标题封面）
- 社交裂变实操（冷启动三连、群引导、社交货币设计）
- 新号冷启动7天执行清单
- 日常数据复盘框架与 A/B 测试方法
- 2026年新规与合规红线
- 常见问题 Q&A

### 使用说明

1. 用户提出视频号运营相关问题
2. 技能根据指南内容提供针对性建议
3. 可结合用户的具体账号阶段（冷启动/成长期/稳定期）给出定制化方案
4. 如需输出文档，可配合 Markdown 转 DOCX/PPTX 功能生成报告