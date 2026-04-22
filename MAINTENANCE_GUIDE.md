# 网站维护指南

本文档介绍如何更新个人学术网站的四类内容：**个人简介（Bio）**、**新闻（News）**、**论文（Publications）**、**研究方向页（Projects）**，以及它们如何自动同步到网站各页面。

---

## 目录

1. [自动同步原理](#自动同步原理)
2. [更新个人简介（Bio）](#更新个人简介bio)
3. [新增或修改 News 条目](#新增或修改-news-条目)
4. [新增或修改 Publication](#新增或修改-publication)
   - [BibTeX 字段说明](#bibtex-字段说明)
   - [topic 字段与页面映射](#topic-字段与页面映射)
   - [让论文出现在主页精选列表](#让论文出现在主页精选列表)
5. [管理 Projects 页（研究方向）](#管理-projects-页研究方向)
   - [Projects 页的工作原理](#projects-页的工作原理)
   - [将论文归入某个研究方向](#将论文归入某个研究方向)
   - [新增一个研究方向分类](#新增一个研究方向分类)
   - [修改或删除某个研究方向](#修改或删除某个研究方向)
6. [推送到 GitHub 后的流程](#推送到-github-后的流程)

---

## 自动同步原理

网站由 **Jekyll** 驱动，部署在 GitHub Pages 上。每当你将改动推送到 `master` 分支，GitHub Actions 会自动触发构建流程，将最新内容编译并发布到线上。

```
你修改文件  →  git commit + push  →  GitHub Actions 自动构建  →  网站更新（约 2–5 分钟）
```

不需要手动编译或上传任何文件。

---

## 更新个人简介（Bio）

**文件路径：** `_pages/about.md`

用任意文本编辑器打开该文件，正文部分（`---` 分隔线以下）即为个人简介，直接编辑即可。支持 Markdown 和内联 HTML。

**常见修改场景：**

| 需要改的内容 | 对应文本位置 |
|---|---|
| 论文数量（如 80+ → 90+） | `published 80+ peer-reviewed articles` |
| 引用数量（如 14300+ → 15000+） | `14300+ citations in Google Scholar` |
| Stanford Top 2% 年份 | `Stanford Top 2% Worldwide Scientist in 2023/2024/2025` |
| ACL 获奖年份 | `ACL 2024/2025 Outstanding Paper Award` |
| 招生信息 | 文件末尾 `<i class="fa-solid fa-thumbtack"...>` 段落 |

**修改示例：**

```markdown
# 修改前
She has published 80+ peer-reviewed articles ... with 14300+ citations

# 修改后
She has published 90+ peer-reviewed articles ... with 15000+ citations
```

---

## 新增或修改 News 条目

**文件夹路径：** `_news/`

每条 News 是一个独立的 Markdown 文件（如 `announcement_8.md`）。

### 新增一条 News

1. 在 `_news/` 文件夹中新建一个文件，命名格式建议为 `announcement_N.md`（N 为顺序编号）。

2. 文件内容模板如下：

```markdown
---
layout: post
date: 2025-05-01 00:00:00+0800
inline: true
related_posts: false
---

这里写新闻内容，支持 Markdown 和 HTML 链接。例如：

[ACL 2025 Outstanding Paper Award](https://2025.aclweb.org/program/awards/)
```

3. **`date` 字段决定排列顺序**，日期越新显示越靠前。

### 删除或隐藏一条 News

- 直接删除对应的 `.md` 文件即可从网站移除。
- 如果只想暂时隐藏，可以在文件开头 front matter 中添加 `hidden: true`（视主题支持情况而定）。

---

## 新增或修改 Publication

**文件路径：** `_bibliography/Publication.bib`

所有论文以 **BibTeX 格式**存储在这一个文件中。网站的 Publications 页、Projects 页、About 主页精选列表均自动从此文件读取。

### 新增一篇论文

在文件**开头**（最顶部）添加新条目，这样它会出现在所有论文的最前面（便于管理；排列顺序由 `year` 字段控制）。

**完整模板（AI 安全方向论文）：**

```bibtex
@article{你的引用键},
  author    = {Author One and Author Two and Shao, Jing},
  title     = {论文完整标题},
  journal   = {会议/期刊缩写，如 ACL 或 NeurIPS 或 Arxiv},
  year      = {2025},
  topic     = {attack},
  selected  = {true},
  order     = {1},
  abstract  = {这里粘贴摘要文本（可选，点击 Abs 按钮展开）},
  website   = {https://论文主页或 arXiv 链接},
  arxiv     = {2501.12345},
  code      = {https://github.com/xxx/xxx},
  award     = {ACL 2025 Outstanding Paper Award},
  award_name = {🏆 Outstanding Paper},
}
```

> **引用键**（如 `zhang2024psysafe`）须全文件唯一，建议格式：`第一作者姓+年份+关键词`。

### BibTeX 字段说明

| 字段 | 必填 | 说明 |
|---|---|---|
| `author` | ✅ | 作者列表，用 `and` 分隔；带 `*` 或 `†` 符号直接写在姓名后 |
| `title` | ✅ | 论文标题 |
| `journal` / `booktitle` | ✅ | 发表期刊或会议名称（`@article` 用 `journal`，`@inproceedings` 用 `booktitle`） |
| `year` | ✅ | 发表年份，控制页面中的年份分组 |
| `topic` | ✅ | 研究主题标签（见下方说明），决定论文出现在 Projects 页哪个分类 |
| `selected` | — | 填 `{true}` 则出现在主页精选论文列表 |
| `order` | — | 配合 `selected`，控制精选列表中的显示顺序（数字越小越靠前） |
| `abstract` | — | 摘要文本，网站上点击 **Abs** 按钮可展开 |
| `website` | — | 论文主页链接，显示为 **Website** 按钮 |
| `arxiv` | — | arXiv ID（只填数字部分如 `2501.12345`），自动生成 **arXiv** 按钮 |
| `pdf` | — | PDF 链接，显示为 **PDF** 按钮 |
| `code` | — | 代码仓库链接，显示为 **Code** 按钮 |
| `blog` | — | 博客/解读文章链接，显示为 **Blog** 按钮 |
| `slides` | — | Slides 链接，显示为 **Slides** 按钮 |
| `award` | — | 获奖说明文本（Markdown 格式），点击奖项按钮展开 |
| `award_name` | — | 奖项按钮上显示的文字，不填默认显示 "Awarded" |
| `bibtex_show` | — | 填 `{true}` 则展示 **Bib** 按钮，供读者复制引用 |

### topic 字段与页面映射

`topic` 字段控制论文在 **Projects 页**的归属分类，同时决定论文条目左上角显示的标签：

| `topic` 值 | 页面显示标签 | 出现在 Projects 页哪个分类 |
|---|---|---|
| `attack` | Attack | Projects → Attack |
| `evaluation` | Evaluation | Projects → Evaluation |
| `frontier` | Frontier AI Risk Analysis | Projects → Frontier AI Risk Analysis |
| `mitigation` | Risk Mitigation | Projects → Risk Mitigation |
| `cv` | CV | 不出现在 Projects 页（仅显示标签） |

> **说明：** 早期 CV 方向论文统一使用 `topic={cv}`，AI 安全方向的新论文根据研究内容选择 `attack` / `evaluation` / `frontier` / `mitigation` 之一。如有新的研究方向需要在 Projects 页新增分类，参见下方"扩展 Projects 分类"。

### 让论文出现在主页精选列表

在 BibTeX 条目中添加：

```bibtex
selected = {true},
order    = {1},
```

- `selected = {true}`：让论文进入主页精选（About 页 Selected Publications 区域）。
- `order`：数字越小排越前，建议按希望的展示顺序从 1 开始编号。

---

## 管理 Projects 页（研究方向）

### Projects 页的工作原理

Projects 页（`_pages/projects.md`）**不需要手动维护论文列表**。它通过 `topic` 字段自动从 `Publication.bib` 中筛选对应论文并展示。整个流转关系如下：

```
Publication.bib 中的 topic 字段
        ↓
Projects 页按 topic 自动分类展示
        ↓
每篇论文条目左上角也会显示对应的 topic 标签，点击可跳转到 Projects 页对应分类
```

也就是说，**给论文加上正确的 `topic` 值，它就自动出现在 Projects 页的对应分类下**，无需其他操作。

---

### 将论文归入某个研究方向

只需在 `Publication.bib` 对应条目中设置 `topic` 字段即可：

```bibtex
@article{your_key,
  author = {...},
  title  = {...},
  year   = {2025},
  topic  = {attack},   ← 改这里
}
```

当前支持的分类及效果：

| `topic` 值 | Projects 页显示位置 | 论文标签显示 |
|---|---|---|
| `attack` | Attack 分类 | Attack（可点击） |
| `evaluation` | Evaluation 分类 | Evaluation（可点击） |
| `frontier` | Frontier AI Risk Analysis 分类 | Frontier AI Risk Analysis（可点击） |
| `mitigation` | Risk Mitigation 分类 | Risk Mitigation（可点击） |
| `cv` | 不出现在 Projects 页 | CV（纯文本标签） |

---

### 新增一个研究方向分类

如果未来有新的研究方向（例如 `alignment`），需要同步修改 **两个文件**：

#### 第一步：在 `_pages/projects.md` 中添加新分类区块

打开 `_pages/projects.md`，在现有分类后面添加：

```html
<h2 id="Alignment" style="border-bottom: 1px solid var(--global-divider-color); padding-bottom: 0.5rem; margin-top: 2rem;">Alignment</h2>
<div class="publications">
{% bibliography -q @*[topic=alignment] --group_by none %}
</div>
```

> 注意：`id="Alignment"` 用于页面内锚点跳转，`topic=alignment` 与 BibTeX 中的 `topic` 值一致。

#### 第二步：在 `_layouts/bib.liquid` 中添加标签映射

打开 `_layouts/bib.liquid`，找到 `{% if entry.topic %}` 块，在现有 `elsif` 分支中添加新条目：

```liquid
{% elsif entry.topic == 'alignment' %}
  {% assign topic_label = 'Alignment' %}
  {% assign topic_url = '/projects/#Alignment' %}
```

完整示例（找到这段并在其中插入）：

```liquid
{% if entry.topic == 'attack' %}
  ...
{% elsif entry.topic == 'mitigation' %}
  ...
{% elsif entry.topic == 'alignment' %}        ← 新增这三行
  {% assign topic_label = 'Alignment' %}
  {% assign topic_url = '/projects/#Alignment' %}
{% elsif entry.topic == 'cv' %}
  ...
{% endif %}
```

#### 第三步：在论文条目中使用新 topic

```bibtex
topic = {alignment},
```

完成后推送，新分类即自动生效。

---

### 修改或删除某个研究方向

**重命名分类：**
1. 在 `_pages/projects.md` 中修改对应 `<h2>` 的标题文字和 `id` 属性
2. 在 `_layouts/bib.liquid` 中同步修改 `topic_label` 和 `topic_url`
3. 在 `Publication.bib` 中批量替换相关条目的 `topic` 值（如从 `attack` 改为 `adversarial`）

**删除某个分类：**
1. 在 `_pages/projects.md` 删除对应的 `<h2>` 和 `{% bibliography %}` 区块
2. 相关论文的 `topic` 值保留或改为 `cv`（不影响 Publications 页显示）

---

## 推送到 GitHub 后的流程

完成文件修改后，在终端执行：

```bash
git add _bibliography/Publication.bib   # 或其他修改的文件
git commit -m "Add paper: 论文标题"
git push origin master
```

推送成功后：

1. GitHub Actions 自动触发（可在仓库的 **Actions** 标签页查看进度）。
2. 约 **2–5 分钟**后网站完成更新。
3. 更新内容会自动同步到以下所有页面：

| 修改的内容 | 自动更新的页面 |
|---|---|
| `Publication.bib` 中任意条目 | Publications 页（全部论文按年份） |
| `topic={attack/evaluation/frontier/mitigation}` | Projects 页对应分类 + 论文条目上的可点击标签 |
| `topic={cv}` | 论文条目上显示 CV 纯文本标签（不进入 Projects 页） |
| `selected={true}` | About 主页精选论文列表 |
| `_pages/about.md` 正文 | About 主页个人简介 |
| `_news/` 下的文件 | About 主页 News 列表 |
| `_pages/projects.md` | Projects 页结构和分类标题 |

---

## 附：本地预览（可选）

如需在推送前本地验证效果，确保已安装 Docker，然后在项目根目录执行：

```bash
docker build -t al-folio .
docker run -p 8080:8080 --rm -v $(pwd):/srv/jekyll al-folio
```

打开浏览器访问 `http://localhost:8080` 即可预览。
