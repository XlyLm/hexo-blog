---
title: Hexo博客网站搭建
abbrlink: 38227
date: 2026-07-05 22:59:40
tags: Hexo
index_img: /img/hexo/hexo.png
cover: /img/hexo/hexo.png
---
# Hexo博客网站搭建

​	**Hexo** 是一个基于 Node.js 的静态博客框架，支持 Markdown 和多种插件，能够快速生成静态页面并部署到 GitHub Pages、Gitee 等平台。以下是从零开始搭建 Hexo 博客的详细步骤。

## 一、环境准备

### 1、安装Node.js

​	**链接**：

### 2、安装Git

​	**链接**：

### 3、安装Hexo CLI

- 运行以下命令安装 Hexo 命令行工具：

```bash
npm install -g hexo-cli
```

## 二、初始化项目

1. 打开cmd输入下面指令，创建一个博客文件夹并初始化 Hexo：

   ```bash
   hexo init myblog
   cd myblog
   npm install
   ```

2. 启动本地服务查看效果：

   ```bash
   hexo g
   hexo server
   ```

3. 新建页面

   ```bash
   hexo new [layout] <title>
   ```

4. 设置网站默认语言

   修改站点配置文件 `_config.yml`

   默认语言是 `en`

   主题支持三种语言

   - `default`(en)
   - `zh-CN` (简体中文)
   - `zh-TW` (繁体中文)

5. 设置文章访问路径

   - 下载依赖：

   ```bash
   npm install hexo-abbrlink --save
   ```

   - 配置：

   ```bash
   #permalink_defaults:
   permalink: posts/:abbrlink/
   abbrlink:
     alg: crc32 #support crc16(default) and crc32
     rep: dec   #support dec(default) and hex
   ```

6. 将 hexo 部署到 GitHub

   - 修改配置将文件 `blog/_config.yml` 中的 deploy 项的内容修改

   ```bash
   deploy:
     # 类型
     type: git
     # 仓库
     repo: 仓库 `http/ssh` 地址
     # 分支
     branch: master
   ```

   **type：git 中的冒号后面有空格**

   - 部署

```bash
# 清空静态页面
hexo clean
# 生成静态页面
hexo generate
# 部署 
hexo deploy
```

## 三、项目结构

- **_config.yml**

  网站的 配置 信息，您可以在此配置大部分的参数。

- **package.json**

  应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

- **scaffolds**

  模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

- **source**

  资源文件夹是存放用户资源的地方。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

- **themes**

  [主题文件夹](https://hexo.io/zh-cn/docs/themes)。Hexo 会根据主题来生成静态页面。Butterfly主题。

## 四、常用指令

| 指令                               | 描述                                                         | 参数                                                         | 参数描述                                                     |
| :--------------------------------- | :----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `hexo init [folder]`               | 初始化一个 Hexo 网站。如果不指定 `folder`，会在当前文件夹创建。 |                                                              |                                                              |
| `hexo new [layout] <title>`        | 创建新文章。例如 `hexo new "我的新文章"`。 `layout` 可指定为 `post`（文章）、`page`（页面）或 `draft`（草稿）。 `title` 有空格时需用引号括起来。此命令有简写 `hexo n`。 | `-p 【--path`】；`-r 【--replace】`；`-s 【--slug】`         | 文章的路径， 自定义文章的路径；如果存在的话，替换当前的文章；文章别名， 自定义文章的 URL。 |
| `hexo generate`                    | 生成静态文件。这是构建网站的核心命令，有简写 `hexo g`。      | `-d 【--deploy】`；`-w 【--watch】`；`-b 【--bail】`；`-f 【--force】`；`-c 【--concurrency】` | 在生成完成后部署；监视文件变动；生成过程中如果发生任何未处理的异常则抛出异常；强制重新生成；要同时生成的文件的最大数量。 默认无限制 |
| `hexo server`                      | 启动本地预览服务器。默认地址为 `http://localhost:4000/`，有简写 `hexo s`。 | `-p 【--port】`；`-s 【--static】`；`-l 【--log】`           | 重设端口；只使用静态文件；启用日志， Override logger format。 |
| `hexo deploy`                      | 将生成的静态文件部署到远程仓库（如 GitHub Pages），有简写 `hexo d`。 | `-g 【--generate】`                                          | 在部署前生成                                                 |
| `hexo clean`                       | 清理缓存文件 (`db.json`) 和已生成的静态文件 (`public` 文件夹)。当网站显示异常时，可以尝试先执行此命令。 |                                                              |                                                              |
|                                    |                                                              |                                                              |                                                              |
|                                    |                                                              |                                                              |                                                              |
| `hexo list <type>`                 | 列出网站的所有路由。`<type>` 可以是 `page`, `post`, `route`, `tag`, `category` 等。 |                                                              |                                                              |
| `hexo version`                     | 显示 Hexo 及环境版本信息。                                   |                                                              |                                                              |
| `hexo config [key] [value]`        | 查看或修改配置。                                             |                                                              |                                                              |
| `hexo publish [layout] <filename>` | 发布草稿。                                                   |                                                              |                                                              |
| `hexo render <file1> [file2]...`   | 渲染文件。                                                   |                                                              |                                                              |
| `hexo migrate <type>`              | 从其他博客系统迁移内容。                                     |                                                              |                                                              |
|                                    |                                                              |                                                              |                                                              |
| hexo --safe                        | 安全模式                                                     |                                                              |                                                              |
| hexo --debug                       | 调试模式                                                     |                                                              |                                                              |
| hexo --silent                      | 简洁模式                                                     |                                                              |                                                              |
| hexo --config custom.yml           | 自定义配置文件的路径                                         |                                                              |                                                              |
| hexo --draft                       | 显示草稿                                                     |                                                              |                                                              |
| hexo --cwd /path/to/cwd            | 自定义 CWD                                                   |                                                              |                                                              |

## 五、常用插件

#### 1、文章管理与优化

| 插件                                                     | 描述                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| **`hexo-admin`**                                         | 提供了一个美观的博客后台管理界面，可以直接在浏览器里创建、编辑和管理文章。 |
| **`hexo-abbrlink`**                                      | 为每篇文章生成一个**唯一且简短的永久链接**（如 `posts/8ddf18fb.html`），解决默认链接含中文、层级深的问题。支持多种算法和进制。 |
| **`hexo-hide-posts`**                                    | 可以隐藏特定文章，使其不在首页、归档等列表中出现，仅通过链接访问。 |
| **`hexo-generator-index`**                               | 一个官方的核心插件，用于生成博客的文章列表（首页）。         |
| **`hexo-generator-archive`**                             | 另一个官方核心插件，用于生成按时间归档的文章页面。           |
| **`hexo-renderer-marked`**                               | Hexo 默认的 Markdown 渲染器。开启 `post_asset_folder` 后，可方便地管理每篇文章的图片等资源。 |
| **`hexo-image-link`**                                    | 解决 Typora 等编辑器与 Hexo 文章图片路径不一致的问题。       |
| **`hexo-admonition`**                                    | 允许在文章中添加带有图标和不同样式的**提示、警告、错误**等块级内容。 |
| **`hexo-renderer-kramed`** 或 **`hexo-renderer-pandoc`** | 替代默认渲染器，以支持在文章中**渲染 LaTeX 数学公式**。      |
| **`hexo-douban`**                                        | 可以在博客中轻松嵌入你的豆瓣读书、电影、音乐等个人主页。     |

#### 2、内容与媒体增强

| 插件                                                     | 描述                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| **`hexo-renderer-marked`**                               | Hexo 默认的 Markdown 渲染器。开启 `post_asset_folder` 后，可方便地管理每篇文章的图片等资源。 |
| **`hexo-image-link`**                                    | 解决 Typora 等编辑器与 Hexo 文章图片路径不一致的问题。       |
| **`hexo-admonition`**                                    | 允许在文章中添加带有图标和不同样式的**提示、警告、错误**等块级内容。 |
| **`hexo-renderer-kramed`** 或 **`hexo-renderer-pandoc`** | 替代默认渲染器，以支持在文章中**渲染 LaTeX 数学公式**。      |
| **`hexo-douban`**                                        | 可以在博客中轻松嵌入你的豆瓣读书、电影、音乐等个人主页。     |

#### 3、SEO 与站点分析

| 插件                               | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| **`hexo-generator-sitemap`**       | 自动生成 `sitemap.xml` 文件，方便搜索引擎（如 Google）抓取你的博客内容。 |
| **`hexo-generator-baidu-sitemap`** | 专门为百度搜索引擎生成 `baidusitemap.xml` 文件。             |
| **`hexo-analytics`**               | 方便地集成 Google Analytics 和 Microsoft Clarity 等网站分析工具。 |
| **`hexo-generator-feed`**          | 生成 RSS 或 Atom 订阅源，方便读者通过订阅器关注你的更新。    |

#### 4、性能与体验提升

| 插件                          | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| **`hexo-all-minifier`**       | 一个综合优化工具，可以压缩网站的 HTML、CSS、JS 甚至图片文件，提升加载速度。 |
| **`hexo-helper-live2d`**      | 为你的博客添加一个可爱的**二次元看板娘（Live2D 模型）**，增加互动趣味。 |
| **`hexo-symbols-count-time`** | 在文章中显示**字数统计和预计阅读时长**。                     |
| **`hexo-external-link`**      | 自动为文章中的**外部链接添加 `rel="noopener external"` 等属性**，提升安全性和 SEO。 |

#### 5、评论与社交

| 插件                                           | 描述                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **`hexo-giscus`**                              | 接入基于 GitHub Discussions 的 Giscus 评论系统。             |
| **`hexo-gitalk`**                              | 接入基于 GitHub Issue 的 Gitalk 评论系统。                   |
| **`hexo-algolia`** 或 **`hexo-algoliasearch`** | 接入 Algolia 搜索引擎，为你的博客提供**高效、即时的站内搜索**功能。 |

#### 6、开发辅助工具

| 插件            | 描述                                                    |
| --------------- | ------------------------------------------------------- |
| **`hexo-fs`**   | 官方工具插件，提供更便捷的文件系统（IO）操作。          |
| **`hexo-util`** | 官方工具插件，包含一系列常用的工具函数。                |
| **`hexo-i18n`** | 官方工具插件，用于实现网站的**国际化（多语言）** 支持。 |

- **安装**：所有插件都可以通过 npm 安装，例如：`npm install hexo-abbrlink --save`。
- **配置**：安装后，通常需要在博客根目录的 `_config.yml` 文件中添加相应的配置项。
- **查找更多插件**：Hexo 官网的 [Plugins](https://hexo.io/plugins/) 页面有最全的列表。也可以在 npm 上搜索以 `hexo-` 开头的包。

## 六、网站简单配置

#### 1、网站基本信息

| 配置项        | 描述                          | 示例                                           |
| ------------- | ----------------------------- | ---------------------------------------------- |
| `title`       | 网站主标题                    | title: 张三的博客                              |
| `subtitle`    | 网站副标题                    | subtitle: 记录技术与生活                       |
| `description` | 网站描述，常用于SEO           | description: 分享Web开发、人工智能等领域的知识 |
| `keywords`    | 网站关键词，用于SEO           | keywords: Hexo, 博客, JavaScript, 人工智能     |
| `author`      | 作者名称                      | author: 张三                                   |
| `language`    | 网站语言，使用 ISO-639-1 代码 | language: zh-CN                                |
| `timezone`    | 网站时区，默认为电脑时区      | timezone: Asia/Shanghai                        |

#### 2、网址与链接

| 配置项               | 描述                                                         | 示例                                 |
| -------------------- | ------------------------------------------------------------ | ------------------------------------ |
| `url`                | **网站根网址**，**必须**以 `http://` 或 `https://` 开头      | url: https://yourname.github.io      |
| `root`               | 网站根目录。如果网站放在子目录（如 `https://example.com/blog/`），则需设置 | `root: /` 或 `root: /blog/`          |
| `permalink`          | 文章的**永久链接**格式                                       | permalink: :year/:month/:day/:title/ |
| `permalink_defaults` | 永久链接中各部分的默认值                                     | permalink_defaults:                  |
| `pretty_urls`        | 用于美化URL，可以去掉`index.html`或`.html`后缀               | 见下方示例                           |

```bash
pretty_urls:
  trailing_index: false # 去除链接尾部的 index.html，如 /foo/index.html -> /foo/[reference:25]
  trailing_html: false  # 去除链接尾部的 .html，如 /foo/bar.html -> /foo/bar[reference:26]
```

#### 3、目录路径

| 配置项         | 描述                                                   | 默认值     |
| -------------- | ------------------------------------------------------ | ---------- |
| `source_dir`   | 资源文件夹，存放你的内容（如文章）                     | source     |
| `public_dir`   | 公共文件夹，存放生成的静态网站文件                     | public     |
| `tag_dir`      | 标签文件夹                                             | tags       |
| `archive_dir`  | 归档文件夹                                             | archives   |
| `category_dir` | 分类文件夹                                             | categories |
| `skip_render`  | 跳过渲染的路径，匹配的文件会被直接复制到 `public` 目录 | 无         |

#### 4、文章与写作

| 配置项                             | 描述                                               | 默认值       |
| ---------------------------------- | -------------------------------------------------- | ------------ |
| `new_post_name`                    | 使用 `hexo new` 创建新文章时的文件名格式           | :title.md    |
| `default_layout`                   | 新建文章时的默认布局                               | post         |
| `titlecase`                        | 是否将标题自动转换为标题格式（每个单词首字母大写） | false        |
| `external_link`                    | 外部链接相关配置，如是否在新窗口打开               | enable: true |
| `post_asset_folder`                | 是否为每篇文章创建一个同名的资源文件夹             | false        |
| `relative_link`                    | 是否使用相对路径的链接                             | false        |
| `future`                           | 是否显示发布日期为未来的文章                       | true         |
| `highlight` / `syntax_highlighter` | 代码高亮相关设置                                   |              |

#### 5、首页与分页

| 配置项                     | 描述                                      | 默认值 |
| -------------------------- | ----------------------------------------- | ------ |
| `index_generator.path`     | 首页的路径                                | ''     |
| `index_generator.per_page` | 首页每页显示的文章数量                    | 10     |
| `index_generator.order_by` | 首页文章的排序方式                        | -date  |
| `per_page`                 | 单个页面上显示的文章数量（0表示禁用分页） | 10     |
| `pagination_dir`           | 分页目录                                  | page   |

#### 6、分类与标签

| 配置项             | 描述         |
| ------------------ | ------------ |
| `default_category` | 默认分类     |
| `category_map`     | 分类别名映射 |
| `tag_map`          | 标签别名映射 |

#### 7、主题与插件

| 配置项         | 描述                                                    | 默认值    |
| -------------- | ------------------------------------------------------- | --------- |
| `theme`        | 网站使用的主题名称                                      | landscape |
| `theme_config` | 主题的特定配置（也可单独放在主题目录的`_config.yml`中） |           |

#### 8、部署

| 配置项   | 描述     | 示例       |
| -------- | -------- | ---------- |
| `deploy` | 部署配置 | 见下方示例 |

```bash
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io.git
  branch: main
```

