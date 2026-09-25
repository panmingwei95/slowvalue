---
title: 博客搭建完成：Hugo + PaperMod
date: 2026-09-20T15:00:00+08:00
draft: true
author: 潘铭尉
description: 记录本站的技术选型：Hugo 生成静态站，PaperMod 提供主题，并演示主题支持的各项写作能力。
summary: Hugo + PaperMod 的搭建记录，同时作为文章格式的演示模板（目录、代码、图片、折叠、表格、引用、脚注）。
categories:
  - 创作实践
tags:
  - Hugo
  - PaperMod
  - 工具链
keywords:
  - Hugo
  - PaperMod
  - 静态博客
showToc: true
TocOpen: true
comments: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
UseHugoToc: true
cover:
  image: ""
  alt: 站点封面
  caption: ""
  hidden: true
  hiddenInList: true
  hiddenInSingle: false
---


这篇既是上线的第一篇记录，也是**写作格式说明书**——以后写文章，直接照这里的写法抄即可。

<!--more-->

## 一、技术栈

| 环节 | 选择 | 版本 / 说明 |
| --- | --- | --- |
| 静态站点生成 | Hugo（extended） | v0.166.0 extended，主题要求 ≥ v0.146.0 |
| 主题 | [PaperMod](https://github.com/adityatelange/hugo-PaperMod) | 以 git submodule 方式装在 `themes/PaperMod` |
| 配置文件 | `hugo.yaml` | YAML 格式，比 TOML 可读性更好 |
| 部署 | GitHub Pages（可选） | 见 `.github/workflows/hugo.yaml` |

选择 Hugo 的理由很直接：单文件二进制、毫秒级构建、零 npm 依赖，写完 Markdown 直接出静态 HTML，适合长期维护。

## 二、常用命令

```bash
# 本地预览（含草稿，改文件自动刷新）
./hugo.exe server -D

# 生成静态文件到 public/
./hugo.exe --gc --minify

# 新建文章（默认使用 archetypes/post.md 模板）
./hugo.exe new content posts/我的新文章.md
```

> 提示：文章放在 `content/posts/` 下，文件名建议用英文或拼音，标题写在前置参数 `title` 里。

## 三、正文写法示例

### 3.1 提示块与强调

普通文本支持 **加粗**、*斜体*、~~删除线~~、`行内代码`、[超链接](https://gohugo.io/)。

> 引用块适合放原文摘录，比如巴菲特的原话。
>
> ——沃伦·巴菲特

### 3.2 列表与任务清单

- 无序列表项一
- 无序列表项二
  - 嵌套项

1. 有序列表项一
2. 有序列表项二

- [x] 已完成的事项
- [ ] 待办事项

### 3.3 折叠块（适合放长代码或长引用）

{{< collapse summary="点开查看更多" >}}
这里是被折叠的内容，可以放很长的代码、原文或附录。

```python
import math

def compound(principal: float, rate: float, years: int) -> float:
    """复利终值：本金 × (1 + 月利率) ^ 月数"""
    return principal * (1 + rate) ** (years * 12)

print(f"{compound(100_000, 0.10, 500):.3e}")
```
{{< /collapse >}}

> 注意：PaperMod 的折叠短代码用的是**命名参数** `summary`，必须写成 `{{</* collapse summary="标题" */>}}`；
> 写成位置参数 `{{</* collapse "标题" */>}}` 会在构建时打印 `missing value for param 'summary'` 警告，且 `<summary>` 会是空的。
> 需要默认展开时加 `openByDefault=true`。

### 3.4 图片

引入图片有两种方式：

```markdown
<!-- 方式一：放 static/images/ 下，用绝对路径 -->
![图片说明](/images/example.jpg)

<!-- 方式二：article bundle，与文章同目录 -->
![图片说明](example.jpg "鼠标悬停显示的标题")
```

### 3.5 表格

上面「技术栈」那张表就是标准 Markdown 表格，表头 `| --- |` 的数量要与列数一致。

### 3.6 脚注

Hugo 原生支持脚注[^1]。

[^1]: 这是脚注内容，会自动收在正文末尾。

## 四、单篇文章可用的前置参数

写在 `---` 之间（会被 `archetypes/post.md` 自动带出来），常用的有：

| 参数 | 作用 |
| --- | --- |
| `title` | 标题 |
| `date` / `lastmod` | 发布 / 更新日期 |
| `draft` | `true` 时本地 `-D` 可见、正式构建不输出 |
| `description` / `summary` | 摘要，用于列表页和 SEO |
| `categories` / `tags` | 分类与标签 |
| `showToc` / `TocOpen` | 是否显示目录 / 目录是否默认展开 |
| `searchHidden` | 设为 `true` 则不被站内搜索收录 |
| `cover.image` | 封面图路径 |
| `weight` | 置顶排序，数字越小越靠前 |

## 五、下一步

- [ ] 换掉 `hugo.yaml` 里的 `baseURL`、社交图标链接
- [ ] 把 `static/` 下的示例图标换成自己的头像与 favicon
- [ ] 写第一篇正式内容
- [ ] 需要评论时，按 PaperMod 文档接入第三方评论系统
