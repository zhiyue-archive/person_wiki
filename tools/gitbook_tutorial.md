# gitbook
最后修改时间 ：{{ file.mtime }}

> GitBook 是一个基于 Node.js 的命令行工具，可使用 Github/Git 和 Markdown 来制作精美的电子书。

- Gitbook项目地址

 - [GitBook项目官网：](http://www.gitbook.com)
 - [GitBook Github地址：](https://github.com/GitbookIO/gitbook)
 - [GitBook 参考文档](http://help.gitbook.com/)



## 安装
### 安装Node.js
参考：https://nodejs.org

### 安装 gitbook
官网指导：https://github.com/GitbookIO/gitbook
- 使用NPM安装
```
$ npm install gitbook-cli -g
```

## 使用
### 基本文件
gitbook 项目必须包含两个文件：
- README.md
> 相当于一本书的简介

- SUMMARY.md
> 描述一本书的目录结构，使用Markdown语法

`README.md` 示例文件：

```
wiki
=======

这里主要记录了我的个人的wiki，主要为了备忘，主要大部分可能摘录自网上
```


`SUMMARY.md` 实例文件：
```
# 目录

* [我的工具](tools/README.md)
 - [git](tools/git.md)
 - [tmux](tools/tmux.md)
 - [zsh](tools/zsh.md)
```

配置好这两个文件后就可以使用以下的命令初始化；
```
$ gitbook init
```
就可以生成对应的目录结构和文件了，每个目录中，都有一个README.md文件，相当于一章的说明。
```
E:\REPOS\GITBOOK\ZHIYUE\TEST
│- README.md
│- SUMMARY.md
│
└─tools
   - git.md
   - README.md
   - tmux.md
   - zsh.md

```
### 本地预览
```
$ gitbook serve
```

### 生成静态文件
```
$ gitbook build
```

### 输出格式
Gitbook支持如下输出：

 - 静态站点：GitBook默认输出该种格式，生成的静态站点可直接托管搭载Github Pages服务上；
 - eBook：需要安装[ebook-convert](http://manual.calibre-ebook.com/cli/ebook-convert.html)
   - PDF：`gitbook pdf ./myrepo ./mybook.pdf`
   - ePub: `gitbook epub ./myrepo ./mybook.epub`
   - mobi: `gitbook mobi ./myrepo ./mybook.mobi`
 - JSON：一般用于电子书的调试或元数据提取。
 ```
 gitbook build ./myrepo --format=json
 ```
### 图书格式
#### 多语言支持
GitBook 支持构建不同语言版本的书。每种语言使用一个子目录和通常的gitbook格式一样，但图书根目录需要放置一个`LANGS.md`:
```
* [English](en/)
* [简体中文](zh/)
```

#### 词汇表
可以定义一些词汇的含义，gitbook会在页面上自动索引和高亮这些词汇。
`GLOSSARY.md` 格式：
```
# term
Definition for this term

# Another term
With it's definition, this can contain bold text and all other kinds of inline markup ...
```

#### 变量和模板
可以在`book.json`定义一些变量：
```
{
    "variables": {
        "host": "mybook.com"
    }
}
```
这些变量可以在markdown文件里使用：
```
The host is {{ book.host }}
```
还可以对变量进行条件判定：
```
{% if book.host == "mybook.com" %}

{% else %}

{% endif %}
```
可以使用`file`和`gitbook`变量获取一些信息：
```
My file is {{ file.path }}
Modified at {{ file.mtime }}
Book built with GitBook {{ gitbook.version }}
```
#### 内容引用

- 引用同一本书的内容：

```
{% include "./test.md" %}
```

- 引用repository的内容:

```
{% include "git+https://github.com/GitbookIO/documentation.git/README.md#1.0.1" %}
```

- 引用变量：

```
{% include book.ref_doc_readme %}
```

#### .gitignore 文件

`.gitignore` ` .bookignore` `.ignore` 都是起到相同的作用.

`.bookignore`示例:
```
# Node rules:
## Grunt intermediate storage (http://gruntjs.com/creating-plugins#storing-task-files)
.grunt

## Dependency directory
## Commenting this out is preferred by some people, see
## https://docs.npmjs.com/misc/faq#should-i-check-my-node_modules-folder-into-git
node_modules

# Book build output
_book

# eBook build output
*.epub
*.mobi
*.pdf
```
[来源](https://github.com/github/gitignore/blob/master/GitBook.gitignore)

#### 封面
在根目录下创建：`/cover.jpg` 和 `/cover_small.jpg`
`/cober.jpg`的最佳分辨率是 `1800x2360`

- 可以使用[autocover](https://github.com/GitbookIO/plugin-autocover)插件生成封面

#### AsciiDoc
从2.0版本之后 就开始支持`AsciiDoc`格式,可以使用`.adoc`后缀取代`.md` 。并且目录使用`SUMMARY.adoc`





### 插件
- [GitBook 插件](https://plugins.gitbook.com/)


#### 插件设置
`book.json` 的文件作为插件配置:

```
{
    // 输出目录
    // 可以被命令行覆盖
    // 不建议在 book.json 中使用该参数
    "output": null,

    // Generator to use for building
    // Caution: it overrides the value from the command line
    // 不建议在 book.json 中使用该参数
    "generator": "site",

    // 元数据（某些可以从 README 中提取）
    "title": null,
    "description": null,
    "isbn": null,

    // For ebook format, the extension to use for generation (default is detected from output extension)
    // "epub", "pdf", "mobi"
    // 可以被命令行覆盖
    // 不建议在 book.json 中使用该参数
    "extension": null,

    // 插件列表，可以使用 "-name" 删除默认插件
    "plugins": [],

    // 插件的全局设置
    "pluginsConfig": {
        "fontSettings": {
            "theme": "sepia", "night" or "white",
            "family": "serif" or "sans",
            "size": 1 to 4
        }
    },

    // 模板渲染参数
    "variables": {},

```
#### 插件安装

```
gitbook install
```


#### 官方插件
|名称|功能描述|
|---|---|
|[exercises](https://github.com/GitbookIO/plugin-exercises)|添加可交互的习题|
|[quizzes](https://github.com/GitbookIO/plugin-quizzes)|添加可交互的选择题|
|[mathjax](https://github.com/GitbookIO/plugin-mathjax)|添加数学表达式支持|
#### 第三方插件
| 名称 | 功能描述 |
| ----- | ---- |
| [Google Analytics](https://github.com/GitbookIO/plugin-ga) | Google Analytics 追踪服务 |
| [Disqus](https://github.com/GitbookIO/plugin-disqus) | Disqus 评论插件 |
| [Autocover](https://github.com/GitbookIO/plugin-autocover) | 自动生成书本封面 |
| [Transform annoted quotes to notes](https://github.com/erixtekila/gitbook-plugin-richquotes) | 对 Markdown 引用进行二次渲染 |
| [Send code to console](https://github.com/erixtekila/gitbook-plugin-toconsole) | 在浏览器控制台中执行 JavaScript |
| [Bootstrap JavaScript 插件](https://github.com/mrpotes/gitbook-plugin-bootstrapjs) | 在 GitBook 中使用 [Bootstrap JavaScript 插件](http://getbootstrap.com/javascript) |
| [Piwik Open Analytics](https://github.com/emmanuel-keller/gitbook-plugin-piwik) | Piwik Open Analytics 数据追踪服务 |
| [Heading Anchors](https://github.com/rlmv/gitbook-plugin-anchors) | 标题上添加 GitHub 风格的锚点链接 |
| [JSBin](https://github.com/jcouyang/gitbook-plugin-jsbin) | [在 GitBook 中嵌入 JS 终端](http://jcouyang.gitbooks.io/functional-javascript/content/en/functor_&_monad/functor.html) |
| [GrVis](https://github.com/romanlytkin/gitbook-grvis) | 将 Markdown 文本转为 svg 格式的 Graphviz 图 |
| [PlantUml](https://github.com/romanlytkin/gitbook-plantuml) | 根据 Markdown 内容生成 svg 格式的 UML 图片 |
| [Mermaid](https://github.com/JozoVilcek/gitbook-plugin-mermaid) | 通过 [mermaid](https://github.com/knsv/mermaid) 渲染流程图、示意图 |
|ComScore|ComScore 可以为各级标题添加不同的颜色，更容易区分各级标题 |
|duoshuo|多说评论插件|
#### 更多插件
你可以在[官方插件中心](http://plugins.gitbook.com/) 或者 [npm](https://www.npmjs.com/search?q=gitbook-plugin) 寻找更多插件。

#### 支持mathjax

在`book.json`加入如下插件：
```
{
    "plugins": ["mathjax"]
}
```

使用：
```
Here is some inline math: $$a \ne 0$$

Here is a block of math:

$$
a \ne 0
$$
```

效果：

Here is some inline math: $$a \ne 0$$

Here is a block of math:

$$
a \ne 0
$$
####  序列图插件
[JS Sequence Diagram 插件](https://github.com/gmassanek/gitbook-plugin-js-sequence-diagram)
是一个 GitBook 生成序列图的插件，这里以它为例讲解如何安装插件。

首先在配置文件中加入插件名 ``js-sequence-diagram``，不包括 ``gitbook-`` 那部分：

```json
{
    "plugins": ["js-sequence-diagram"]
}
```

然后使用 gitbook 控制台工具安装：``gitbook install``。

JS Sequence Diagram 的基本使用如下：

    ```sequence
    Title: 标题

    A->B: 直线
    B-->C: 虚线
    C->>D: 箭头
    D-->>A: 虚线箭头
    ```

效果：

```sequence
Title: 标题

A->B: 直线
B-->C: 虚线
C->>D: 开箭头
D-->>A: 虚线开箭头
```
JS Sequence Diagram 的更多使用请移步[其文档](http://bramp.github.io/js-sequence-diagrams/)。







## 发布

### 发布到Gitbook
要使用 GitBook.com 来托管你的书籍，首先需要注册一个账号，免费注册后，用户也可以选择升级为付费用户，享受更多的服务.

登陆 GitBook.com 后，在用户页面，可以管理现有书籍以及创建新的书籍，如下图：
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/QQ%E6%88%AA%E5%9B%BE20151022221516.png)
点击 "+ Create a new book" 后，跳转到新建书籍页面，如下图：
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/QQ%E6%88%AA%E5%9B%BE20151022221559.png)
创建完成后，就会进入书籍属性页面，如下图所示：
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/QQ%E6%88%AA%E5%9B%BE20151022221658.png)

这里可以进行对书籍的各个属性进行配置，例如：
- 编辑书籍（Edit Book）
- 书籍主题（Theme）
- 绑定 GitHub（GitHub）
- 绑定域名（Domain Names）

#### 编辑书籍
##### 在线编辑
进入到书籍的属性页面后，点击 "Edit Book" 按钮即可打开在线编辑器。
##### [gitbook editor](https://www.gitbook.com/editortor)编辑器
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022223755.png)
##### 下载到本地编辑
托管在gitbook.com上的书籍是使用git管理的，git的地址格式一般为：
```
https://git.gitbook.com/zhiyue/test.git
```
使用`git clone`下载到本地后即可以编辑
#### 自定义域名
- gitbook 设置
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/Animation%204.gif)
- dnspod 设置

![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022224815.png)
#### GitHub 集成
![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022225422.png)

### 使用GiHub Pages 服务
上一节 已经把gitbook 与github的repos 连接起来。目前测试是双向同步的即无论在gitbook上修改或者在github上提交修改都会进行双向同步。
既然代码可以托管在github上，那我们何不利用github上提供的github pages 服务呢？
#### 创建 gh-pages 分支
```
$ git checkout --orphan gh-pages
$ git rm --cached -r .
$ git clean -df
$ rm -rf *~
```
现在，目录下应该只剩下 _book 目录了，首先，忽略一些文件：
```
$ echo "*~" > .gitignore
$ echo "_book" >> .gitignore
$ git add .gitignore
$ git commit -m "Ignore some files"
```
然后，加入 _book 下的内容到分支中：
```
$ cp -r _book/* .
$ git add .
$ git commit -m "Publish book"
```
#### 自定义域名
- 在 根目录下增加`CNAME`文件：
> everforget.com

- 设置dnspod

![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022225653.png)
这是使用双线路。国外的解析到gitbook上生成的静态页面，国内的解析到github上，虽然github上也很慢。

### 持续化集成(使用travis自动化构建)
增加`.travis.yml`文件在根目录：
```
# Deploy hexo site by travis-ci
# https://github.com/jkeylu/deploy-hexo-site-by-travis-ci
# LICENSE: MIT
#
# 1. Copy this file to the root of your repository, then rename it to '.travis.yml'
# 2. Replace 'YOUR NAME' and 'YOUR EMAIL' at line 29
# 3. Add an Environment Variable 'DEPLOY_REPO'
#     1. Generate github access token on https://github.com/settings/applications#personal-access-tokens
#     2. Add an Environment Variable on https://travis-ci.org/{github username}/{repository name}/settings/env_vars
#         Variable Name: DEPLOY_REPO
#         Variable Value: https://{githb access token}@github.com/{github username}/{repository name}.git
#         Example: DEPLOY_REPO=https://6b75cfe9836f56e6d21187622730889874476c23@github.com/jkeylu/test-hexo-on-travis-ci.git

language: node_js

node_js:
  - "0.12"

branches:
  only:
    - master

before_install:
- npm install -g gitbook-cli

install:
#- npm install hexo-renderer-ejs@0.1.0 --save
#- npm install hexo-renderer-marked@0.1.0 --save
#- npm install hexo-renderer-stylus@0.1.0 --save
#- npm install hexo-generator-feed@0.2.1 --save
#- npm install hexo-generator-sitemap@0.2.0 --save
#- npm install hexo-tag-bootstrap@0.0.6 --save
- gitbook install



script:
  - "git submodule init"
  - "git submodule update"
  - "git submodule foreach git pull origin master"
  - gitbook build

after_success:
  - "git clone $DEPLOY_REPO git_deploy"
  - "cd git_deploy"
  - "pwd"
  - "git checkout gh-pages"
  - "cd .."
  - "pwd"
  - "ls ."
  - "rm -r git_deploy/* "
  - "cp -r _book/* git_deploy"
  - "cd git_deploy"
  - "git config --global push.default simple"
  - "git config --global user.name 'zhiyue'"
  - "git config --global user.email cszhiyue@gmail.com"
  - "git add -A"
  - "git commit -m 'Site updated'"
  - "git push -q"
```
#### 去 https://travis-ci.org/ 设置：

![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022231048.png)

- 设置`DEPLOY_REPO` 变量：

![enter image description here](http://7fvgjy.com1.z0.glb.clouddn.com/gitbook_tutorial/20151022230954.png)
## 缺点

- 不支持中文搜索


## 参考：
- [GitBook](https://github.com/GitbookIO/gitbook#variables-and-templating)
- [GitBook使用](http://wiki.11ten.net/Node/gitbook%E4%BD%BF%E7%94%A8.html)
- [使用GitBook- 不破不立](http://blog.windrunner.info/app/gitbook-tutorial.html)
- [gitbook- chengwei's blog](http://www.chengweiyang.cn/gitbook/gitbook.com/newbook.html)

## 修订日志

- 2015-10-22 第一次撰写
