# Docsify教程 

### Docsify是什么？
`docsify`可以即时生成您的文档网站。与`GitBook`不同，它不会生成静态`html`文件。相反，它可以智能地加载和解析您的`Markdown`文件并将其显示为网站。要开始使用它，您所需要做的就是创建一个`index.html`并将其部署在`GitHub Pages`上。

> GITHUB：[https://github.com/docsifyjs/docsify/](https://github.com/docsifyjs/docsify/)
>
> 官网文档：[https://docsify.js.org/#/](https://docsify.js.org/#/)



### 安装

首先要先安装`node.js`,[安装教程](tutorial/nodejs.md),然后打开命令行窗口执行如下命令
```bash
npm i docsify-cli -g
```


### 初始化

选择一个目录编写文档，例如在当前目录下的`docs`目录下编写则可以使用`init`命令。
```bash
docsify init ./docs
```
**在之后init完成后，你可以看到在文件列表./docs子目录。**

- `index.html` 作为入口文件
- `README.md` 作为主页
- `.nojekyll` 防止GitHub Pages忽略以下划线开头的文件



### 本地预览

在当前目录下执行一下命令
```bash
docsify serve docs
```
进入[http://localhost:3000](http://localhost:3000)就能看到效果！

### 手动初始化

可以参考官网教程，[点击查看](https://docsify.js.org/#/quickstart?id=manual-initialization)

### 载入对话框

如果需要，您可以在`docsify`开始呈现文档之前显示一个加载对话框，在`inde.html`中修改即可：

```html
<div id="app">Please wait...</div>
```



### 如何写文章

如果需要更多页面，只需在docsify目录中创建更多markdown文件即可。如果创建一个名为的文件`guide.md`，则可以通过访问该文件`/#/guide`。

例如，目录结构如下：

```text
.
└── docs
    ├── README.md
    ├── guide.md
    └── zh-cn
        ├── README.md
        └── guide.md
```

匹配路线

```text
docs/README.md        => http://domain.com
docs/guide.md         => http://domain.com/#/guide
docs/zh-cn/README.md  => http://domain.com/#/zh-cn/
docs/zh-cn/guide.md   => http://domain.com/#/zh-cn/guide
```

### 创建侧边栏

可以创建自己的侧边导航栏：

首先，您需要在`index.html`中设置`loadSidebar`为**true**。

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```

其次，在`docs`目录下创建`_sidebar.md`：

```markdown
<!-- docs/_sidebar.md -->

* [Home](/)
* [Guide](guide.md)
```

您需要在`docs`目录下创建一个`.nojekyll`来防止GitHub Pages忽略以下划线开头的文件。

### 从侧边栏选择设置页面标题

页面的`title`标签是根据*所选的*侧边栏项目名称生成的。您可以通过在文件名后指定字符串来自定义标题。

```markdown
<!-- docs/_sidebar.md -->
* [Home](/)
* [Guide](guide.md "The greatest guide in the world")
```

### 自定义导航栏

1. 如果需要自定义导航，则可以创建基于HTML的导航栏。请注意，文档链接以开头`#/`。

```html
<!-- index.html -->

<body>
  <nav>
    <a href="#/">EN</a>
    <a href="#/zh-cn/">中文</a>
  </nav>
  <div id="app"></div>
</body>
```

2. 您也可以通过设置`loadNavbar`为**true**并创建`_navbar.md`自定义导航文件。

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<!-- _navbar.md -->

* [En](/)
* [chinese](/zh-cn/)
```

3. 您可以通过缩进特定父项下的项目来创建子列表，下拉菜单的形式呈现。

```markdown
<!-- _navbar.md -->

* Getting started

  * [Quick start](quickstart.md)
  * [Writing more pages](more-pages.md)
  * [Custom navbar](custom-navbar.md)
  * [Cover page](cover.md)

* Configuration
  * [Configuration](configuration.md)
  * [Themes](themes.md)
  * [Using plugins](plugins.md)
  * [Markdown configuration](markdown.md)
  * [Language highlight](language-highlight.md)
```

## 将自定义导航栏与emoji插件结合

如果您使用表情符号插件，引入`js`文件。

```html
<!-- index.html -->
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
```

例如，您可以在自定义导航栏Markdown文件中使用标志表情符号：

```markdown
<!-- _navbar.md -->

* [:us:, :uk:](/)
* [:cn:](/zh-cn/)
```

### 自定义封面

#### 基本用法

设置`coverpage`为**true**，并创建一个`_coverpage.md`：

```html
<!-- index.html -->

<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<!-- _coverpage.md -->

![logo](_media/icon.svg)

# docsify <small>3.5</small>

> A magical documentation site generator.

- Simple and lightweight (~21kB gzipped)
- No statically built html files
- Multiple themes

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)
```

> 一个文档站点只能有一个封面！

### 自定义背景

默认情况下，背景颜色是随机生成的。您可以自定义背景色或背景图片：

```markdown
<!-- _coverpage.md -->

# docsify <small>3.5</small>

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)

<!-- background image -->

![](_media/bg.png)

<!-- background color -->

![color](#f0f0f0)
```

### 封面作为首页

通常，封面和首页会同时显示。当然，您也可以通过`onlyCover选项`分隔封面。

### 多个封面

如果您的文档网站使用多种语言，则设置多个封面可能很有用。

例如，您的docs结构是这样的

```text
.
└── docs
    ├── README.md
    ├── guide.md
    ├── _coverpage.md
    └── zh-cn
        ├── README.md
        └── guide.md
        └── _coverpage.md
```

现在，您可以设置

```js
window.$docsify = {
  coverpage: ['/', '/zh-cn/']
};
```

或特殊文件名

```js
window.$docsify = {
  coverpage: {
    '/': 'cover.md',
    '/zh-cn/': 'cover.md'
  }
};
```

### 个性化配置

#### 配置 `window.$docsify`

```html
<script>
  window.$docsify = {
    repo: 'docsifyjs/docsify',
    maxLevel: 3,
    coverpage: true,
  };
</script>
```

详细配置请查看官网[https://docsify.js.org/#/configuration](https://docsify.js.org/#/configuration)





<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<div class="social-share"></div>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>

