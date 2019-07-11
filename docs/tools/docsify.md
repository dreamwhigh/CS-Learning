### docsify 教程

#### 前期准备

##### 安装 git

[git 国内镜像下载地址](<https://github.com/waylau/git-for-win/>)

##### 安装 npm

参照 [npm 安装教程](<https://www.cnblogs.com/goldlong/p/8027997.html>)，安装 [node](<https://nodejs.org/en/>) ，建议选择 Long Term Support (LTS) 长期支持版本。

全局安装 `docsify-cli` 工具，可以方便创建及本地预览文档网站。

```
npm i docsify-cli -g
```

#### 初始化

在 E:\GitHub 下打开 git bash here ,通过 `docsify init ./name` 初始化项目，自动生成 E:\GitHub\docsify 目录，且在该目录下会存在以下三个文件：

- .nojekyll
- index.html
- README.md

```
$ docsify init ./docsify

Initialization succeeded! Please run docsify serve ./docsify
```

`docsify serve docsify`  运行本地服务器，默认访问 http://localhost:3000 实时预览效果

```
$ docsify serve docsify

Serving E:\GitHub\docsify now.
Listening at http://localhost:3000
```

#### 多页文档

```
-| docsify/
  -| README.md
  -| guide.md
  -| zh-cn/
    -| README.md
    -| guide.md
```

对应的访问地址为

```

```



#### 侧边栏

##### 默认加载方式

通过 `_sidebar.md` 文件创建

在 index.html 中配置 `loadSidebar` 选项，默认加载 `_sidebar.md` 文件

```
  <script>
    window.$docsify = {
	  loadSidebar: true
    }
  </script>
```

​	接着创建 `_sidebar.md` 文件，保存在根目录 E:\GitHub\docsify 下

```
* [首页](guide)
* [指南](zh-cn/guide)
* [自定义加载的文件](summary)
```

 	效果

![1562813004256](E:\GitHub\docsify-pics\1562813004256.png)

##### 自定义加载文件名

配置 `loadSidebar` 选项

```
  <script>
    window.$docsify = {
	  loadSidebar: 'summary.md'
    }
  </script>
```

效果

![562813148977](E:\GitHub\docsify-pics\1562813148977.png)

#### 目录

配置 `subMaxLevel`，其后的数字表示能够显示副标题的级数

```
<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2//只能显示一、二级标题
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

`{docsify-ignore}` 忽略特定标题；`{docsify-ignore-all}` 忽略特定页面下的所有标题；

修改 E:\GitHub\docsify\guide 文档如下

```
1 guide

# 一级标题0

不会显示在侧边栏

# 一级标题1

显示

## 二级标题1

显示

### 三级标题1

设置的 subMaxLevel: 2 ，故三级标题不显示

# 一级标题2

显示

## 二级标题2{docsify-ignore}

{docsify-ignore} 省略当前标题

# 一级标题3{docsify-ignore-all}

{docsify-ignore-all} 省略其下所有标题

## 二级标题3

显示
```

显示结果如下：

![1562814881526](E:\GitHub\docsify-pics\1562814881526.png)

#### 导航栏

##### HTML 定义

注意链接要以 `#/` 开头。

```
<body>
  <nav>//定义导航栏
    <a href="#/路径">导航栏命名</a>
    <a href="#/">EN</a>
    <a href="#/zh-cn/">中文</a>
  </nav>
  <div id="app"></div>
</body>
```

##### Markdown 文件定义

先配置 `loadNavbar` ，默认加载的文件为 `_navbar.md`

```
<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

​	创建 `_navbar.md` ，保存在根目录 E:\GitHub\docsify 下

```markdown
* [En](/)
* [中文](/zh-cn/)
```

​	与侧边栏一样，也可以自定义加载文件名

```
window.$docsify = {
  // 加载 _navbar.md
  loadNavbar: true,

  // 加载 nav.md
  loadNavbar: 'nav.md'
};
```

**Markdown 文件配置的优先级高于直接在 HTML 里定义**

##### 嵌套

如果导航内容过多，可以写成嵌套的列表，会被渲染成下拉列表的形式。

修改  `_navbar.md` 文件

```
- 导航1
  - [home 1](/)
  - [guide 1](/guide)

- 导航2
  - [home 2](/zh-cn/)
  - [guide 2](/zh-cn/guide)
```

结果如下图：

![1562817235032](E:\GitHub\docsify-pics\1562817235032.png)

#### 封面

##### Markdown 文件配置

修改 `index.html` 文件

```
<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

创建 `_coverpage.md` 文件，将引用图片保存在 E:\GitHub\docsify\_medial 路径下

```
![logo](_media/logo1)
or
<img width="280px" src="_media/logo1.png">

# docsify

> A magical documentation site generator.

* Simple and lightweight (~12kb gzipped)
* Multiple themes
* Not build static html files

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)
```

一份文档只会在根目录下加载封面，其他页面或者二级目录下都不会加载。

##### 背景

默认的背景是随机生成的渐变色，我们自定义背景色或者背景图。在文档末尾用添加图片的 Markdown 语法设置背景。

`_coverpage.md`

```
<!-- 背景图片 -->

![](_media/bg.png)

<!-- 背景色 -->

![color](#f0f0f0)
```

常用颜色????



##### 多个封面页

```
window.$docsify = {
  coverpage: true,

  // 自定义文件名
  coverpage: 'cover.md',

  // mutiple covers
  coverpage: ['/', '/zh-cn/'],

  // mutiple covers and custom file name
  coverpage: {
    '/': 'cover.md',
    '/zh-cn/': 'cover.md'
  }
};
```

#### [其他配置项](<https://docsify.js.org/#/zh-cn/configuration>)

主题

<link rel="stylesheet" href="//unpkg.com/docsify/lib/themes/vue.css">