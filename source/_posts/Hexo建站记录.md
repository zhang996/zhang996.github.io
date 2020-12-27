---
title: Hexo建站记录
date: 2020-12-27 13:21:00
author: Zhang
img: https://ae05.alicdn.com/kf/H395622ab73aa42a5aa32ff37101e9804d.png
summary: 搬运完了以前的文章，是时候总结一下这个站点如何建立了，技术小白—只做记录
tags:
  - Hexo
---

## 什么是 Hexo？

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 使用环境

由[Hexo](https://hexo.io/zh-cn/docs/)的官网可以了解到，本地需要部署[Node.js](http://nodejs.cn/)以及[Git](https://www.runoob.com/git/git-install-setup.html)

我选择的版本如下：

```
[root@VM-8-8-centos ~]# node -v
v10.23.0
[root@VM-8-8-centos ~]# hexo -v
hexo-cli: 4.2.0
```

这里给出官方要求的对应版本

| Hexo 版本   | 最低兼容 Node.js 版本 |
| :---------- | :-------------------- |
| 5.0+        | 10.13.0               |
| 4.1 - 4.2   | 8.10                  |
| 4.0         | 8.6                   |
| 3.3 - 3.9   | 6.9                   |
| 3.2 - 3.3   | 0.12                  |
| 3.0 - 3.1   | 0.10 or iojs          |
| 0.0.1 - 2.8 | 0.10                  |

前述准备工作做好以后，安装Hexo

```
npm install -g hexo-cli
```

## 建站

### （1）创建文件夹

创建一个新文件夹hexo，并打开hexo文件夹作为工作目录

```
mkdir hexo
cd /hexo
```

### （2）Hexo初始化

执行初始化命令，并安装必要模块

```
hexo init 
npm install
```

新建完成后，指定文件夹的目录如下

```
.
├── _config.yml			————网站的配置信息
├── package.json		————应用程序的信息。EJS, Stylus 和 Markdown renderer已默认安装
├── scaffolds			————模版文件夹
├── source				————存放用户资源的地方
|   ├── _drafts
|   └── _posts
└── themes				————主题文件夹
```

对_config.yml文件进行配置可参考[官方手册](https://hexo.io/zh-cn/docs/configuration)

值得一提的事config文件会有好多份，这里需要注意一下优先级问题：Hexo 在合并主题配置时，Hexo 配置文件中的 theme_config 的优先级最高，其次是 _config.[theme].yml 文件，最后是位于主题目录下的 _config.yml 文件。

### （3）常用命令

```
hexo generate
```

生成静态文件。

| 选项                  | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| `-d`, `--deploy`      | 文件生成后立即部署网站                                       |
| `-w`, `--watch`       | 监视文件变动                                                 |
| `-b`, `--bail`        | 生成过程中如果发生任何未处理的异常则抛出异常                 |
| `-f`, `--force`       | 强制重新生成文件 Hexo 引入了差分机制，如果 `public` 目录存在，那么 `hexo g` 只会重新生成改动的文件。 使用该参数的效果接近 `hexo clean && hexo generate` |
| `-c`, `--concurrency` | 最大同时生成文件的数量，默认无限制                           |

------

```
hexo server
```

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

| 选项             | 描述                           |
| :--------------- | :----------------------------- |
| `-p`, `--port`   | 重设端口                       |
| `-s`, `--static` | 只使用静态文件                 |
| `-l`, `--log`    | 启动日记记录，使用覆盖记录格式 |

------

```
hexo deploy
```

部署网站。

| 参数               | 描述                     |
| :----------------- | :----------------------- |
| `-g`, `--generate` | 部署之前预先生成静态文件 |

------

```
hexo clean
```

清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

### （4）备份与恢复

这部分内容参考：[IMZM低吟浅唱](https://pzh0226.top/2020/07/13/Hexo%E5%8D%9A%E5%AE%A2%E5%A4%87%E4%BB%BD%E4%B8%8E%E6%81%A2%E5%A4%8D/)的文章

#### 思路

在仓库里创建新的分支hexo，存放Hexo生成的原始文件，原master分支存放生成的静态网页。备份时上传需备份的文件到hexo分支，恢复时拉取hexo分支到本地。

#### 博客内文件或文件夹的内容

- `.deploy_git`: 执行`hexo g`自动生成，不需拷贝
- `node_modules`: 安装包的目录，执行`npm install`自动生成，不需拷贝
- `public`: 执行`hexo g`自动生成，不需拷贝
- `scaffolds`: 文章的模板，**需拷贝**
- `source`: 包含生成网页需要的源文件和其他资源文件，**需拷贝**
- `themes`: 主题，**需拷贝**
- `.gitignore`: 在push时忽略的文件，**需拷贝**
- `_config.yml`: 站点的配置文件，**需拷贝**
- `db.json`: 配置文件，不需拷贝
- `package.json`: 依赖的模块列表，**需拷贝**
- `package-lock.json`: 模块安装记录，自动生成，不需拷贝

#### 具体操作

1. 在`.gitignore`里添加不需拷贝的内容，可以添加不需拷贝的主题（例如自带主题）。
2. 删除主题文件夹下的`.git/`，否则无法push（或者新增个备份文件夹`themesbk/themename`存放主题拷贝，然后push）
3. 在blog根目录执行命令`git init`初始化本地仓库
4. 继续执行命令`git checkout -b hexo`创建hexo分支，分支名称可改
5. 执行`git remote add origin https://github.com/username/username.github.io.git`添加远程仓库
6. 依次执行命令`git add .` , `git commit -m "something"` , `git push origin hexo`
7. 需备份时只需执行第6步的命令

#### 恢复

1. 安装git, nodejs
2. 执行`git clone -b hexo https://github.com/username/username.github.io.git blog`克隆至blog文件夹（名称可改），clone速度太慢可使用镜像`github.com.cnpmjs.org`或者给git设置代理
3. 在blog文件夹内执行`npm install`安装必要模块（网上部分教程依次执行`npm install hexo-cli`, `npm install`, `npm install hexo-deployer-git`，笔者不清楚是否有必要）
4. 正常地更新博客然后使用`hexo g`, `hexo s`, `hexo d`就行啦（若deploy提示失败根据提示登录即可）