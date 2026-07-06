---
title: Butterfly主题配置
abbrlink: 600736463
date: 2026-07-06 21:13:59
tags: Hexo
index_img: /img/hexo/butterfly.png
cover: /img/hexo/butterfly.png
---
# Butterfly主题配置

## 一、安装

- **Git**方式：

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

​	将对应的butterfly复制到你的根目录的themes里面。

- **npm**方式(hexo站点目录下)：

```bash
npm i hexo-theme-butterfly
```

## 二、配置

### 1、启用主题

 修改站点配置文件`_config.yml`，把主题改为 `Butterfly` 。

```bash
theme: Butterfly
```

### 2、安装插件

 如果你沒有 pug 以及 stylus 的渲染器，请重新安装 。

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 3、升级建议

 为了主题的平滑升级。

 推荐在hexo根目录创建一个文件`_config.butterfly.yml`,并把主题目录的`_config.yml`内容复制到新建的`config`文件里去。

**注意：**

- 不要把主题目录的`_config.yml`文件删除。
- 之后的主题配置，在`_config.butterfly.yml`中进行。
- hexo会自动合并主题的两个配置文件，`_config.butterfly.yml`优先级高。

### 4、主题页面

**Front-matter**

- page Front-matter

  ```bash
  //必需
  title: 页面标题
  date: 页面创建时间
  type: 标签、分类和友情链接页面需要配置
  
  //可选
  updated: 页面更新时间
  description: 页面描述
  keywords: 页面关键字
  comments: 显示页面评论模块(true)
  top_img: 页面顶部图片
  aside: 显示侧边栏(true)
  ```

- post Front-matter

  ```bash
  //必需
  title: 页面标题
  date: 页面创建时间
  
  //可选
  updated: 页面更新时间
  tags: 文章标签
  categories: 文章分类
  top_img: 页面顶部图片
  cover: 缩略图/顶部图
  description: 页面描述
  keywords: 页面关键字
  comments: 显示页面评论模块(true)
  aside: 显示侧边栏(true)
  toc:  是否显示toc （除非特定文章设置，可以不写）
  toc_number: 是否显示toc数字 （除非特定文章设置，可以不写）
  copyright: 是否显示版权 （除非特定文章设置，可以不写）
  ```

- 标签页

  1. 前往你的 `Hexo` 博客的根目录
  2. 输入 `hexo new page tags`
  3. 你会找到 `source/tags/index.md` 这个文件
  4. 添加：`type: "tags"`字段

- 分类页

  1. 前往你的 `Hexo` 博客的根目录
  2. 输入 `hexo new page categories`
  3. 你会找到 `source/categories/index.md` 这个文件
  4. 添加：`type: "categories"`字段

- 友情链接

  1. 前往你的 `Hexo` 博客的根目录

  2. 输入 `hexo new page link`

  3. 你会找到 `source/link/index.md` 这个文件

  4. 添加：type: "link"字段

  5. 添加友链

      在 `Hexo` 博客目录中的 `source/_data`，创建一个文件 `link.yml` 

     ```javascript
     - class_name: 友情鏈接
       class_desc: 那些人，那些事
       link_list:
         - name: Hexo
           link: https://hexo.io/zh-tw/
           avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
           descr: 快速、簡單且強大的網誌框架
     
     - class_name: 網站
       class_desc: 值得推薦的網站
       link_list:
         - name: Youtube
           link: https://www.youtube.com/
           avatar: https://i.loli.net/2020/05/14/9ZkGg8v3azHJfM1.png
           descr: 視頻網站
         - name: Weibo
           link: https://www.weibo.com/
           avatar: https://i.loli.net/2020/05/14/TLJBum386vcnI1P.png
           descr: 中國最大社交分享平臺
         - name: Twitter
           link: https://twitter.com/
           avatar: https://i.loli.net/2020/05/14/5VyHPQqR6LWF39a.png
           descr: 社交分享平臺
     ```

      由 2.2.0 起，友情鏈接界面可以由用戶自己自定義，只需要在友情鏈接的md檔設置就行，以普通的Markdown格式書寫。 

### 5、导航菜单

```js
Home(主页): / || fas fa-home
Archives(时间轴): /archives/ || fas fa-archive
Tags(标签): /tags/ || fas fa-tags
Categories(分类): /categories/ || fas fa-folder-open
List(清单)||fas fa-list:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
Link(友情链接): /link/ || fas fa-link
About(关于笔者): /about/ || fas fa-heart
```

>  菜单名称可自己修改 
>
> 名称： /路径/ || icon

### 6、代码

#### 代码高亮

 `Butterfly` 支持6重代码高亮样式：

- darker
- pale night
- light
- ocean
- mac
- mac light

 修改 `主題配置文件` 

```bash
highlight_theme: light
```

####  代码复制

主题支持代码复制功能

 修改 `主題配置文件` 

```bash
highlight_copy: true
```

#### 代码框展开/关闭

在默认情况下，代码框自动展开

- `true`：全部代码框不展示，需手动点击展示
- `false`：代码框展开，可手动关闭
- `none`：不展示`>`按钮

 修改 `主題配置文件` 

```bash
highlight_shrink: true/false/none #代碼框不展開，需點擊 '>' 打開
```

#### 代码换行

在默认情况下，`hexo`在编辑的时候是不会为代码换行的，如果你不想在代码框出现滚动条，则可设置：

```bash
code_word_wrap: true
```

 找到你站点的 Hexo 配置文件`_config.yml`，将 `line_number` 改成 `false` ：

```bash
highlight:
  enable: true
  line_number: false # <- 改這裏
  auto_detect: false
  tab_replace:
  
or

prismjs:
  enable: false
  preprocess: true
  line_number: false # <- 改這裏
  tab_replace: ''
```

#### 代码高度限制

可配置代码高度限制，超出部分隐藏，并显示展开按钮

```bash
highlight_height_limit: false # unit: px
```

### 7、社交图标

 Butterfly支持 [font-awesome v6](https://fontawesome.com/icons?from=io)图标。

格式：`图标明：url || 描述性文字`

```bash
social:  
	fab fa-github: https://github.com/xxxxx || Github  
	fas fa-envelope: mailto:xxxxxx@gmail.com || Email
```

### 8、主页文章节选

 因为主题 UI 的关系，主页文章节选只支持自动节选和文章页 description。优先选择自动节选。 

 在 `_config.butterfly.yml` 里可以开启 `auto_excerpt` 的选项，你的文章将会在自动截取部分显示在主页。 

1. **description：** 只顯示description
2. **both：** 優先選擇description，如果沒有配置description，則顯示自動節選的內容
3. **auto_excerpt：**只顯示自動節選
4. **false：** 不顯示文章內容

```
index_post_content:  
	method: 3  
	length: 500 # if you set method to 2 or 3, the length need to config
```

### 9、顶部图

#### page页

在主题配置文件 `_config.butterfly.yml` 设置。

| 配置             | 解释                                                         |
| ---------------- | ------------------------------------------------------------ |
| index_img        | 主页的 top_img                                               |
| default_top_img  | **默认的 top_img，当页面的 top_img 沒有配置时，会显示 default_top_img** |
| archive_img      | 时间轴页面的 top_img                                         |
| tag_img          | tag 子页面 的 默认 top_img                                   |
| tag_per_img      | tag 子页面的 top_img，可配置每個 tag 的 top_img              |
| category_img     | category 子页面 的 默认 top_img                              |
| category_per_img | category 子页面的 top_img，可配置每个category 的 top_img     |

 其它页面 （tags/categories/自建页面）和 文章页 的 `top_img` ，请到对应的 md 页面设置`front-matter`中的`top_img` 

#### post页

 `post` 页的顶部图会优先显示各自 `front-matter` 中的 `top_img`, 如果没有设置，则会缩略图（即各自 `front-matter` 中的 `cover`)，如果没有则会显示显示 `default_top_img` 图片 

### 10、文章置顶

 [`hexo-generator-index`](https://github.com/hexojs/hexo-generator-index)从 2.0.0 开始，已经支持文章置顶功能。你可以直接在文章的`front-matter`区域添加`sticky: 1`属性来把这篇文章置顶。数值越大，置顶的优先級越大。 

### 11、文章封面

文章的markdown文档上,在`Front-matter`添加`cover`,并填上要显示的图片地址。
如果不配置`cover`,可以设置显示默认的`cover`.

如果不想在首页显示cover,可以设置为`false`，修改 `主題配置文件`

```bash
cover:  # 是否显示文章封面  
index_enable: true  
aside_enable: true  
archives_enable: true  
# 封面显示的位置  
# 三個值可配置 left , right , both  
position: both  	//轮换左右
# 當沒有設置cover時，默認的封面顯示  
default_cover: 
```

### 12、文章页相关配置

#### 文章meta显示

 这个选项是用来显示文章的相关信息的。 

 修改 `主題配置文件` 

```bash
post_meta:  
	page:    
		date_type: both #created or updated or both 主頁文章日期是创建日或者更新日或都显示    
		date_format: relative # date/relative 显示日期还是相对日期    
		categories: true # true or false 主页是否显示分类    
		tags: true # true or false 主页是否显示标签    
		label: true # true or false 显示描述性文字  
	post:    
		date_type: both # created or updated or both 文章页日期是创建日或者更新日或都顯示    
		date_format: relative # date/relative 显示日期还是相对日期    
		categories: true # true or false 文章頁是否顯示分類    
		tags: true # true or false 文章页是否显示标签    
		label: true # true or false 显示描述性文字
```

#### 文章版权

 为你的博客文章展示文章版权和许可协议。配置 `butterfly.yml` 。

```bash
post_copyright:  
	enable: true  	#显示版权权
	decode: false  
	author_href:  
	license: CC BY-NC-SA 4.0  
	license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

 如果有文章（例如：转载文章）不需要显示版权，可以在文章 `Front-matter` 单独设置 

```bash
copyright: false
```

 从`3.0.0`开始，支持对单独文章设置版权信息，可以在文章`Front-matter`单独設置 

```bash
copyright_author: xxxx
copyright_author_href: https://xxxxxx.com
copyright_url: https://xxxxxx.com
copyright_info: 此文章版权归xxxxx所有，如有转载，请注明來自原作者
```

#### 文章打赏

 在你每篇文章的结尾，可以添加打赏按钮。相关二维码可以自行配置 `butterfly.yml` 

```bash
reward:  
	enable: true  
	QR_code:    
		- img: /img/wechat.jpg      
			link:      
			text: 微信    
		- img: /img/alipay.jpg      
			link:      
			text: 支付宝
```

### 13、TOC

 在文章页，会有一个目录，用于显示 TOC。 手机端默认显示按钮。 

 配置 `butterfly.yml` 

```
toc:  
	post: true  
	page: false  
	number: true  #显示张数
	expand: false  
	style_simple: false  # for post
```

| 属性         | 解释                 |
| ------------ | -------------------- |
| post         | 文章页是否显示 TOC   |
| page         | 普通页面是否显示 TOC |
| number       | 是否显示章字数       |
| expand       | 是否展开 TOC         |
| style_simple | 简洁模式             |

- **为特定的文章配置**

   在你的文章`md`文件的頭部，加入`toc_number`和`toc`，並配置`true`或者`false`即可。 (优先级高)

- **相关文章**

   相关文章推荐的原理是根据文章tags的比重來推荐。 修改 `主題配置文件` 

  ```bash
  related_post:  
  	enable: true  
  	limit: 6 # 显示推荐文章数目  
  	date_type: created  # or created or updated 文章日期显示创建日或者更新日
  ```

- [文章锚点](https://butterfly.js.org/posts/4aa8abbe/)

- [文章过期提醒](https://butterfly.js.org/posts/4aa8abbe/)

- [文章编辑按钮](https://butterfly.js.org/posts/4aa8abbe/)

- [文章分页按钮](https://butterfly.js.org/posts/4aa8abbe/)

### 14、头像

 修改 `主題配置文件` 

```bash
avatar:  
	img: /img/avatar.png  #img为主题文件下/source/img
	effect: true 	# 頭像會一直轉圈
```

### 15、图片描述

可开放图片Figcaption描述文字描示

首先显示圖图片的 title 屬性，然后是 alt 屬性

修改 `主題配置文件`

```bash
photofigcaption: true
```

### 16、复制相关配置

 可配置网站是否可以复制、复制的內容是否添加版权信息 

```bash
# copy settings
# copyright: Add the copyright information after copied content (複製的內容後面加上版權信息)
copy:  
	enable: true  
	copyright:    
		enable: true    
		limit_count: 50
```

| 配置        | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| enable      | 是否开放网站复制权限                                         |
| copyright   | 复制的內容后面加上版权信息                                   |
| enable      | 是否开放复制版权信息添加                                     |
| limit_count | 字数限制，当复制文字大于这个字数限制时，將在复制的內容后面加上版权信息 |

### 17、Footer设置

#### 博客年份

 since 是一个来展示你站点起始时间的选项。它位于页面的最底部。 

 配置 `_config.butterfly.yml` 

```bash
footer:  
	owner:    
		enable: true    
		since: 2018
	custom_text: 
	copyright: true
```

#### 页脚自定义文字

 `custom_text` 是一个给你用来在页脚自定义文本的选项。通常你可以在这里写声明文本等。支持 HTML。  修改 `主題配置文件` 

```html
custom_text: Hi, welcome to my <a href="https://butterfly.js.org/">blog</a>!
```

#### ICP

对于部分有备案的域名，可以在 ICP 配置显示。

配置 `_cofig.butterfly.yml`

```bash
ICP:  
	enable: true  
	url: http://www.beian.miit.gov.cn/  
	text: 粤ICP备xxxx  
	icon: /img/icp.png 
```

### 18、右下角按钮

#### 简繁转换

右下角会有简繁转换按钮。

配置 `_config.butterfly.yml`

```bash
translate:  
	enable: true  # 默认按钮显示文字
	default: 简  #网站是简体，应设置为'default: 繁'
	defaultEncoding: 1  #网站默认语言，1: 繁体中文, 2: 简体中文
	translateDelay: 0  #延迟时间,若不在前, 要设定延迟翻译时间, 如100表示100ms,默认为0
	cookieDomain: "https://jerryc.me/"	#博客网址
	msgToTraditionalChinese: "繁"  #当文字是简体时，按钮显示的文字
    msgToSimplifiedChinese: "簡"		#当文字是繁体时，按钮显示的文字
```

#### 夜间模式

右下角会有夜间模式按钮

配置 `_config.butterfly.yml`

```bash
# dark mode
darkmode:
  enable: true
  # dark mode和 light mode切換按鈕
  button: true
  autoChangeMode: false
```

#### 阅读模式

阅读模式下会去掉除文章外的内容，避免干扰阅读。

只会出现在文章页面，右下角会有阅读模式按钮。

配置 `_config.butterfly.yml`

```bash
readmode: true
```

#### [按钮排序](https://butterfly.js.org/posts/4aa8abbe/)

### 19、侧边栏设置

 可自行决定哪个项目需要显示，可决定位置。（至少需要显示一个） 

#### 侧边排版

配置 `_config.butterfly.yml`

```bash
aside:  
	enable: true  
	hide: false  
	button: true  
	mobile: true # 手機頁面（ 顯示寬度 < 768px ）是否顯示aside內容  
	position: right # left or right  
	archives: true  
	card_author:    
		enable: true    
		description:    
		button:      
			enable: true	  
			icon: fab fa-github      
		text: Github      
		link: https://github.com/jerryc127/hexo-theme-butterfly  
	card_announcement:    
		enable: true    
		content: This is my Blog  
	card_recent_post:    
		enable: true    
		limit: 5 # if set 0 will show all    
		sort: date # date or updated  
	card_categories:    
		enable: true    
		limit: 8 # if set 0 will show all    
		expand: none # none/true/false  
	card_tags:    
		enable: true    
		limit: 40 # if set 0 will show all    
		color: false  
	card_archives:    
		enable: true   
        type: monthly # yearly or monthly    
        format: MMMM YYYY # eg: YYYY年MM月    
        order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending    
        limit: 8 # if set 0 will show all  
     card_webinfo:    
     	enable: true    
     	post_count: true    
     	last_push_date: true
```

#### 访问人数

 修改 `主題配置文件` 

```bash
busuanzi:  
	site_uv: true  
	site_pv: true  
	page_pv: true
```

#### 运行时间

修改 `主題配置文件`

```bash
runtimeshow:
	enable: true
  	publish_date: 6/7/2018 00:00:00  
  	##網頁開通時間
  	#格式: 月/日/年 時間
  	#也可以寫成 年/月/日 時間
```

#### [最新评论](https://butterfly.js.org/posts/4aa8abbe/)

### 20、标签外挂

 修改 `主題配置文件` 

```bash
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```

 `icons`和`light_bg_offset`只对*方法一*生效 

### [Gallery相册 and other](https://butterfly.js.org/posts/4aa8abbe/)

### 21、评论

 支持双评论显示，只需要配置兩個评论（第一個為默认显示） 

```bash
comments:
  # Up to two comments system, the first will be shown as default
  # Choose: Disqus/Disqusjs/Livere/Gitalk/Valine/Waline/Utterances/Facebook Comments/Twikoo
  use: Valine,Disqus
  text: true # Display the comment name next to the button
  # lazyload: The comment system will be load when comment element enters the browser's viewport.
  # If you set it to true, the comment count will be invalid
  lazyload: true
  count: true # Display comment count in top_img
  card_post_count: false # Display comment count in Home Page
```

| 參數            | 解釋                                                         |
| --------------- | ------------------------------------------------------------ |
| use             | 使用的評論（請注意，最多支持兩個，如果不需要請留空） *注意：雙評論不能是 Disqus 和 Disqusjs 一起，由於其共用同一個 ID，會出錯* |
| text            | 是否顯示評論服務商的名字                                     |
| lazyload        | 是否為評論開啟lazyload，開啟後，只有滾動到評論位置時才會加載評論所需要的資源（*開啟 lazyload 後，評論數將不顯示*） |
| count           | 是否在文章頂部顯示評論數 livere、Giscus 和 utterances 不支持評論數顯示 |
| card_post_count | 是否在首頁文章卡片顯示評論數 gitalk、livere 、Giscus 和 utterances 不支持評論數顯示 |

### [在线聊天](https://butterfly.js.org/posts/ceeb73f/)

### 22、分享

 修改 `主題配置文件` 

```bash
addThis:
  enable: true # or false
  pubid: 你的pub-id
```

### 23、搜索系统

1. 你需要安裝 [hexo-algolia](https://github.com/oncletom/hexo-algolia)或 [hexo-algoliasearch](https://github.com/LouisBarranqueiro/hexo-algoliasearch). 根據它們的説明文檔去做相應的配置。
2. 修改 `主題配置文件`

```bash
algolia_search:  
	enable: true  
	hits:    
		per_page: 6
```

### others

- [网站验证](https://butterfly.js.org/posts/ceeb73f/)
- [分析系统](https://butterfly.js.org/posts/ceeb73f/)
- [广告](https://butterfly.js.org/posts/ceeb73f/)
- [Math数学](https://butterfly.js.org/posts/ceeb73f/)

## 24、美化/特效

### 自定义主题色

可以修改大部分UI顏色

修改 `主題配置文件`

```bash
theme_color:
  enable: true
  main: "#49B1F5"
  paginator: "#00c4b6"
  button_hover: "#FF7242"
  text_selection: "#00c4b6"
  link_color: "#99a9bf"
  meta_color: "#858585"
  hr_color: "#A4D8FA"
  code_foreground: "#F47466"
  code_background: "rgba(27, 31, 35, .05)"
  toc_color: "#00c4b6"
  blockquote_padding_color: "#49b1f5"
  blockquote_background_color: "#49b1f5"
  scrollbar_color: "#49b1f5"
```

### 25、网站背景

默认显示白色，可設置图片或者颜色

修改 `主題配置文件`

```bash
# 圖片格式 url(http://xxxxxx.com/xxx.jpg)
# 顏色（HEX值/RGB值/顔色單詞/漸變色)
# 留空 不顯示背景
background:
```

### 26、footer背景

修改 `主題配置文件`

```bash
# footer是否显示图片背景(與top_img一致)
footer_bg: true
```

### 27、打字效果

打字效果[activate-power-mode](https://github.com/disjukr/activate-power-mode)

修改 `主題配置文件`

```bash
打字效果activate-power-mode
修改 主題配置文件
# Typewriter Effect (打字效果)
# https://github.com/disjukr/activate-power-mode
activate_power_mode:
  enable: true
  colorful: true # open particle animation (冒光特效)
  shake: true #  open shake (抖動特效)
  mobile: false
```

### 28、背景特效

好看的彩帶背景，可設置每次刷新更换彩帶，或者每点击更换彩帶
修改 `主題配置文件`

```bash
canvas_ribbon:
  enable: false
  size: 150
  alpha: 0.6
  zIndex: -1
  click_to_change: false  #設置是否每次點擊都更換綵帶
  mobile: false # false 手機端不顯示 true 手機端顯示
```

### 29、鼠标点击效果

`zIndex`建议只在`-1`和`9999`上选
`-1` 代表焰火效果在底部
`9999` 代表焰火效果在前面

修改 `主題配置文件`

```bash
fireworks:
  enable: false
  zIndex: 9999 # -1 or 9999
  mobile: true
  
click_heart:
  enable: true
  mobile: true
  
ClickShowText:
  enable: false
  text: 
    - 富强
    - 民主
	...
  fontSize: 15px
```

### 30、页面美化

会改变 ol、ul、h1-h5 的样式

修改 `主題配置文件`

```bash
# 美化頁面顯示
beautify:
  enable: true
  field: site # site/post
  title-prefix-icon: '\f0c1'
  title-prefix-icon-color: "#F47466"
```

 `title-prefix-icon` 填写的是 fontawesome 的 icon 的 Unicode 数 

### 31、自定义字体和大小

#### 全体字体

可自行设置字体的`font-family`
**如不需要配置，请留空**

修改 `主題配置文件`

```bash
# Global font settings
# Don't modify the following settings unless you know how they work (非必要不要修改)
font:
  global-font-size:
  code-font-size:
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Lato, Roboto, "PingFang SC", "Microsoft JhengHei", "Microsoft YaHei", sans-serif
  code-font-family: consolas, Menlo, "PingFang SC", "Microsoft JhengHei", "Microsoft YaHei", sans-serif
```

#### blog标签字体

可自行设置字体的`font-family`
**如不需要配置，请留空。**
**如不需要使用网络字体，只需要把font_link留空就行**

修改 `主題配置文件`

```bash
# Font settings for the site title and site subtitle
# 左上角網站名字 主頁居中網站名字
blog_title_font:
  font_link: https://fonts.googleapis.com/css?family=Titillium+Web&display=swap
  font-family: Titillium Web, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft JhengHei', 'Microsoft YaHei', sans-serif
```

#### 网站副标题

可设置主页中显示的网站副标题或者喜欢的座右銘。

修改 `主題配置文件`

```bash
# 主頁subtitle
subtitle:
  enable: false
  # Typewriter Effect (打字效果)
  effect: true
  # loop (循環打字)
  loop: true
  # source 調用第三方服務
  # source: false 關閉調用
  # source: 1  調用一言網的一句話（簡體） https://hitokoto.cn/
  # source: 2  調用一句網（簡體） http://yijuzhan.com/
  # source: 3  調用今日詩詞（簡體） https://www.jinrishici.com/
  # subtitle 會先顯示 source , 再顯示 sub 的內容
  source: false
  # 如果關閉打字效果，subtitle 只會顯示 sub 的第一行文字
  sub:
    - 今日事&#44;今日畢
    - Never put off till tomorrow what you can do today
```

#### 主页top_img显示大小

 默認的顯示為全屏。site-info的區域會居中顯示 

```bash
# 主頁設置
# 默認top_img全屏，site_info在中間
# 使用默認, 都無需填寫（建議默認）
index_site_info_top: # 主頁標題距離頂部距離  例如 300px/300em/300rem/10%
index_top_img_height:  #主頁top_img高度 例如 300px/300em/300rem  不能使用百分比
```

 注意：`index_top_img_height`的值不能使用百分比。 

#### 页面加载动画

 當進入網頁時，因為加載速度的問題，可能會導致top_img圖片出現斷層顯示，或者網頁加載不全而出現等待時間，開啟preloader後，會顯示加載動畫，等頁面加載完，加載動畫會消失。 

配置`butterly.yml`

```bash
# 加載動畫 Loading Animation
preloader: true
```

#### 字数统计

要为`Butterfly`配上字数統計特性, 你需要如下几個步骤:

1. 打開 hexo 工作目录

2. `npm install hexo-wordcount --save` or `yarn add hexo-wordcount`

3. 修改 `主題配置文件`:

   ```bash
   wordcount:
     enable: true
     post_wordcount: true
     min2read: true
     total_wordcount: true
   ```

### [其他](https://butterfly.js.org/posts/ceeb73f/)

## 三、其他配置

### 1、文章添加加密功能

**下载相关依赖**

```bash
npm install hexo-encrypt --save
```

配置hexo根目录的`_config.yml`文件。

```bash
#该密码为全局默认密码
#--------------为文章设置密码
encrypt:
	enable: true
  	password: ****	#默认密码
```

也可以为单独的文章设置：

post font-matter

```bash
encrypt: true/false
password: ***
message: 描述字段
```

### 2、图片懒加载

配置`主题文件`

```bash
lazyload:
	enable: true
	loadingImg: ***	#图片加载完之前显示的图片
```

### 3、代码压缩

下载相关依赖

```bash
npm install hexo-all-minifier --save
```

配置hexo根目录`_config.yml`文件。

```bash
#代码压缩
#-----------------
all_minifier: true

html_minifier:
  enable: true
  ignore_error: false
  exclude:

css_minifier:
  enable: true
  exclude:
    - '*.min.css'

js_minifier:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js'

image_minifier:
  enable: true # 图片不压缩
  interlaced: false
  multipass: false
  optimizationLevel: 2
  pngquant: false
  progressive: false
```

### 4、配置滚动条

在`主题目录的source/css`添加`normalize.css`文件。

代码：

```bash
/* 滚动条 */
::-webkit-scrollbar {
    width: 4px;
    height: 4px;
}

::-webkit-scrollbar-track {
    background-color: rgba(121,106,128, 0.2);
    border-radius: 2em;
    cursor: pointer;
}

::-webkit-scrollbar-thumb {
    background-color: rgba(178,110,131,1);
    /*background-image: -webkit-linear-gradient(*/
    /*        45deg,*/
    /*        rgba(255, 255, 255, 0.4) 25%,*/
    /*        transparent 25%,*/
    /*        transparent 50%,*/
    /*        rgba(255, 255, 255, 0.4) 50%,*/
    /*        rgba(255, 255, 255, 0.4) 75%,*/
    /*        transparent 75%,*/
    /*        transparent*/
    /*);*/
    border-radius: 2em;
}

::-webkit-scrollbar-corner {
    background-color: transparent;
}

::-moz-selection {
    color: #fff;
    background-color: #49b1f5;
}
```

配置主题的`_config.butterfly.yml`文件

```bash
inject:
  head:		#引入你的normalize.css
    - <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/sviptzk/HexoStaticFile@latest/Hexo/css/flink.min.css">
  bottom:
    - <script src="https://cdn.jsdelivr.net/gh/sviptzk/HexoStaticFile@latest/Hexo/js/hideMobileSidebar.js"></script>
```

