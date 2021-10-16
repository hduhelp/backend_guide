# Git 教程

## 导入

Git 是一个分布式版本控制软件 最初由林纳斯·托瓦兹创作 于2005年以GPL发布 最初目的是为更好地管理Linux内核开发而设计

git 会记录其文件变化 对代码进行跟踪和定位 其最大的优势是 **合并跟踪** 这对于大型项目合作开发非常有用

同时 git 的操作更加类似与 文件系统 尽管其具体原理有些晦涩难懂 但是基本的操作又都是~~非常友好~~的 指令

git 的内部实现方式类似与 拷贝文件 但却是进行特殊二进制格式的对象存储 以及压缩 其具体实施过程会由 git 自动负责, 作为程序员本身不需要过多的操心。所以往往其整个 git 项目的文件夹的大小大约等于两倍的源码文件的大小

## 基础指令

### git clone 
 
这个指令就可以用四个大字概括 **拿来吧你**

一般可以有 三种下载方案 

HTTPS / SSH / 类 Git App

> 注意 ssh 处可能需要配置对应的账户 SSH 公私钥
>
> 具体操作可以在 [官方文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) 找到
> 目前 ssh 是使用较多的操作

### git add 

当代码编辑完毕之后使用

增加指令 可以将代码增添到 **临时的存储区域** 并且对存储对象进行预备操作 

> 一般在 jetbrains 系列内置的 git 中 只是勾选文件就可以进行简单的 add 操作

cli 下指令如下

. 是指 当前文件夹 git 会递归当前文件夹下的源代码文件 并且对变化的源代码文件进行增加

-v 只是增加了 verbose 以观察具体的 文件增加过程 以防错误地将敏感文件增加进入 git 历史之中

> 例如 您的 password, 小程序的 key 等等

``` bash
git add [-v] .
```

也可以替换为 其他的具体文件名称 例如 git.md 

``` bash
git add git.md
```

帮助 以及 更多 add 的特殊操作

``` bash
git add -h
```

在 add 进行完毕后 我们可以通过指令 来输出具体的添加和临时存储的信息

``` bash
git status
```

### git commit

commit 便是进行提交和本地归档的操作 所以 commit 操作较为核心 

其同时也具有更多的 子选项和子指令

git 会对 add 操作之后的 **临时存储区域** 进行存储操作 将指定的变动文件进行

现在是基础款的 信息添加
``` bash
git commit -m "提交消息1"
```

帮助 以及 更多 commit 的特殊操作

``` bash
git commit -h
```

这里的操作更多更为复杂 暂时不宜进行展开 可以在遇到了具体问题之后 进行搜索

> 同时 由于本组织使用 jetbrains 系列作为基础的开发
> 
> jetbrains IDE 掩盖了大多数的指令操作 具有的 git 模块会较为具体的展现出各个选项 对新人非常友好

### git push

push 指令是作为 上传指令出现的

其原理是将本地的 .git 文件夹重命名后 上传至服务器中

一般是 项目名称.git

``` bash
git push 
```

一旦你开了一个 pr 之后 再进行编辑时非常容易 push 失败

这时我们需要使用 来进行强制上传提交
``` bash
git push --force
```

帮助 以及更多 push 操作

``` bash
git push -h
```

### git submodule

这是一个很奇怪的概念。操作是一个子模块 安插在你的项目里，并且起到一定的作用（例如 hugo 的主题模块等）

同时 submodule 本身也是作为一个 git repo 存在的

你可能会说
> 啊 这都一样啊 为什么不直接使用那个 仓库 的 git 把它直接塞进去呢？

这里有两个问题

1. 你不能在 git 下使用两个 带.git文件夹的资料 submodule 会将子项目（子模块）的.git变更为文件 指向主仓库的模块 并且创建 .gitmodule 用以维护
2. 子模块可以随时跟着 子模块的仓库的更新进行更新


## Commit & Branch 基础使用的小游戏

https://learngitbranching.js.org/?locale=zh_CN (这是中文版的)

上述教学网站可以提供输入 shell 指令的空间 通过简单的交互和小任务(游戏)的形式使得使用者快速入门 git 命令行操作

### 关于 branch fork 与 commit

- commit 基础款的提交操作 可以用来进行详细的说明与操作
- branch 更像一个对 commit 的引用操作
- fork 几乎可以是完全分离 有一些分家的意味 但也可以用来 提 pull Request 为项目提交改变

## Commit & Branch 等基础使用

https://learngitbranching.js.org/?locale=zh_CN (这是中文版的)

上述教学网站可以提供输入 shell 指令的空间,通过简单的交互和小任务(游戏)的形式使得使用者快速入门 git 命令行操作.

## 客户端选择

- 普普通通 cli git
- 可视化的 fork (https://git-fork.com/) on windows mac
- ... and so on

## Goland Git

https://www.jetbrains.com/help/go/using-git-integration.html

goland jetbrains 官方对于 内置 Git 功能的说明与使用


## Unimportant Misc

### git commit 规范问题

AngularJS 的 git 规范 是使用较为广泛的规范

具体的规范需要遵从具体的项目下说明文档

文章: https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.uyo6cb12dt6w 


## git 源代码安全

根据上文，有提及 git 的原理是代码文件的二进制对象存储和压缩。

所以根据这些这些可以完整的恢复出目录的结构与内容 导致整个项目的源代码的泄露

后端要注意在部署环境时需要注意泄露问题 并且将 /.git/ 加入 403 Forbidden 名单

例如：
> .git/HEAD 当前 git 头指向的一半是下一行的
> .git/refs/heads/ 分支头 会解释每个分支头的 hash
> .git/objects/ 存储各种 二进制对象

例如

```
└─[$] <git:(eson_dev)> cat .git/refs/heads/eson_dev 
58d6c9ae97405490619d15cdde4fe5d1d97976ed
└─[$] <git:(eson_dev)> cat .git/objects/58/d6c9ae97405490619d15cdde4fe5d1d97976ed 
x?? <binary objects>
```

存储二进制的格式又是公开的标准 所以相关的工具很多。

