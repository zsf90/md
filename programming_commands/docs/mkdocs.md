# MkDocs 是什么？

> MkDocs是一个快速、简单、华丽的静态站点生成器，用于构建项目文档。文档源文件是用Markdown编写的，并使用单个YAML配置文件进行配置。首先阅读介绍性教程，然后查看用户指南以获得更多信息。

## 安装

```shell
$ pip install mkdocs
```

## 创建一个 `MkDocs` 项目

```shell
$ mkdocs new my-project
$ cd my-project
```

目录结构：

```shell
zsf90@DESKTOP-N56R3I7:$ tree
.
├── docs
│   └── index.md
└── mkdocs.yml
```

## 启动本地服务器

```shell
$ mkdocs serve
INFO     -  Building documentation...
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.36 seconds
INFO     -  [16:23:17] Serving on http://127.0.0.1:8000/
```

## 配置站点信息

```yaml
site_name: 我的 MkDocs 站点 # 站点标题

site_url: https://www.zsf90.com # 网站URL
```

> **site_name** 和 **site_url** 配置选项是配置文件中仅有的两个必需选项。当您创建一个新项目时，**site_url** 选项被分配占位符值:https://example.com。如果知道最终的位置，现在可以更改设置来指向它。或者你可以选择暂时不去管它。只是要确保在将站点部署到生产服务器之前对其进行编辑。

## 添加页面

```shell
curl 'https://jaspervdj.be/lorem-markdownum/markdown.txt' > docs/about.md
```

添加导航：

```yaml
site_name: MkLorum
site_url: https://example.com/
nav:
    - Home: index.md
    - About: about.md
```

## 修改 favicon.ico

`MkDocs` 默认使用的是自带的 **favicon.ico**，修改方法是在 **docs** 目录新建一个 **img** 目录，然后把自己的 **favicon.ico** 放在该目录下即可。



## 编译项目（生产静态站点）

```shell
$ mkdocs build
```

编译后的项目结构：

```shell
zsf90@DESKTOP-N56R3I7:$ tree
.
├── docs
│   ├── about.md
│   ├── img
│   │   └── favicon.ico
│   ├── index.md
│   └── mkdocs.md
├── mkdocs.yml
└── site
    ├── 404.html
    ├── about
    │   └── index.html
    ├── css
    │   ├── theme.css
    │   └── theme_extra.css
    ├── fonts
    │   ├── Lato
    │   │   ├── lato-bold.eot
    │   │   ├── lato-bold.ttf
    │   │   ├── lato-bold.woff
    │   │   ├── lato-bold.woff2
    │   │   ├── lato-bolditalic.eot
    │   │   ├── lato-bolditalic.ttf
    │   │   ├── lato-bolditalic.woff
    │   │   ├── lato-bolditalic.woff2
    │   │   ├── lato-italic.eot
    │   │   ├── lato-italic.ttf
    │   ├── modernizr-2.8.3.min.js
    │   ├── theme.js
    │   └── theme_extra.js
    ├── mkdocs
    │   └── index.html
    ├── search
    │   ├── lunr.js
    │   ├── main.js
    │   ├── search_index.json
    │   └── worker.js
    ├── search.html
    ├── sitemap.xml
    └── sitemap.xml.gz

12 directories, 53 files
```

## Git 忽略

让 `git` 忽略 **site** 目录。

```shell
$ echo "site/" >> .gitignore
```

## 查看帮助

```shell
$ mkdocs --help
$ mkdocs build --help
```

## 内部链接

使用 Markdown 语法 `[链接](https://www.google.com)`。

## 链接页面

```markdown
Please see the [project license](license.md) for further details.
```

```markdown
Please see the [project license](about.md#license) for further details.
```

## 元数据

示例1

```markdown
---
title: My Document
summary: A brief description of my document.
authors:
    - Waylan Limberg
    - Tom Christie
date: 2018-07-10
some_url: https://example.com
---
This is the first paragraph of the document.
```

示例2

```markdown
Title:   My Document
Summary: A brief description of my document.
Authors: Waylan Limberg
         Tom Christie
Date:    January 23, 2018
blank-value:
some_url: https://example.com

This is the first paragraph of the document.
```

## 表格

A simple table looks like this:

```no-highlight
First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell
```

If you wish, you can add a leading and tailing pipe to each line of the table:

```no-highlight
| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |
```

Specify alignment for each column by adding colons to separator lines:

```no-highlight
First Header | Second Header | Third Header
:----------- |:-------------:| -----------:
Left         | Center        | Right
Left         | Center        | Right
```

```
Fenced code blocks are like Standard Markdown’s regular code blocks, except that they’re not indented and instead rely on start and end fence lines to delimit the code block. 
```

## 提示快 Admonition

```
!!! note "custom title or blank"
    text

# 可折叠，+默认打开
???+ danger highlight blink "custom title or blank"
    text vtext text, text, v<br>
    text vtext text, text, v
    ```python
    text1 = "Hello, "
    text2 = "world!"
    print text1 + text2
```

类型说明

```
??? abstract "摘要，总结" abstract, summary, tldr
??? tip "贴士" tip, hint, important
??? note "注释，代码片段，说明" note, snippet, seealso
??? example "举例，列表" example
??? quote "引用，参考链接" quote, cite
??? info "提示，TODO" info, todo
??? warning "警告" warning, caution, attention
??? danger "危险" danger, error
??? success "成功，勾选，完成" success, check, done
??? fail "失败" failure, fail, missing
??? faq "问题，疑问，帮助" question, help, faq
??? bug "BUG" bug
```

### 列表

- 一级列表使用`-`
- 二级列表使用`*`
- 三级列表使用`+`
- 子级列表缩进4个空格
- 使用复选框：`- [x] item`
- 列表内容换行：行尾2个空格。Admonition中的换行也是一样，结尾2个空格

复选框列表示例，其中`[ ]`表示不打勾，`[x]`表示打勾

```\
- [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
- [x] Curabitur elit nibh, euismod et ullamcorper at, iaculis feugiat est
- [ ] Vestibulum convallis sit amet nisi a tincidunt
    - [x] In hac habitasse platea dictumst
    - [x] Sed egestas felis quis elit dapibus, ac aliquet turpis mattis
    - [ ] Praesent sed risus massa
- [x] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque
- [ ] Nulla vel eros venenatis, imperdiet enim id, faucibus nisi
```

