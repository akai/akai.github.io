---
layout: post
title: "用 rbenv 来管理 Ruby 的安装"
date: 2011-08-22 10:43
comments: true
categories: Ruby
---

如果你 Ruby 开发者，应该知道用 [RVM](http://beginrescueend.com/) 来安装/管理 Ruby 版本，同时也能用它的 gemset 功能来管理各个工程的 gems。

最近，[37Signals](http://37signals.com) 的 [Sam Stephenson](http://sstephenson.us/) 也创建了一个类似的软件，叫做 [rbenv](https://github.com/sstephenson/rbenv)。不过它功能非常简单，简单到只是用来管理 Ruby 版本，连安装的功能也没提供。

*注意：rbenv 和 RVM 是不兼容的，所以安装 rbenv 之前要先把 RVM 卸载了。*

## 1 安装

### 1.1 安装 rbenv

rbenv 的源代码托管在 [GitHub](http://github.com) 下，安装只需要简单的 `clone` 下来就可以来。

把 rbenv clone 到 `~/.rbenv` 下：

``` bash
git clone https://github.com/sstephenson/rbenv ~/.rbenv
```

然后让 shell 加载 rbenv：

``` bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

*如果用 Zsh，就用 `~/.zshrc` 替换 `~/.bash_profile`。*

重启 shell， 或者运行 `exec $SHELL`，就可以开始用 rbenv 了。

### 1.2 安装 ruby-build

由于 rbenv 本身并不能用来安装 Ruby，为了方便我们还需要用到 [ruby-build](https://github.com/sstephenson/ruby-build)，它的安装也非常简单：

``` bash
git clone git://github.com/sstephenson/ruby-build.git
cd ruby-build
./install.sh
```

### 1.3 安装 Ruby

安装好 ruby-build 后，就可以用简单的一条命令来安装 Ruby：

``` bash
ruby-build 1.9.3-p0 ~/.rbenv/versions/1.9.3-p0
```

*注意：Ruby 需要安装在 `~/.rbenv/versions/` 下*

同时 ruby-build 还提供了一个 `rbenv install` 命令给 rbenv，所以上面的命令可以变成：

``` bash
rbenv install 1.9.3-p0
```

## 2 rbenv 的常用命令

rbenv 提供了很多命令，这里列几个常用的：

### 2.1 rbenv global

来用设置 Ruby 的全局版本。

上面安装好 Ruby 后，还需要运行一下这条命令：

``` bash
rbenv global 1.9.3-p0
```

这样默认就会用 1.9.3-p0 了。但如果当前目录下有 `.rbenv-version` 文件，就会用文件里显示的版本。

### 2.2 rbenv local

``` bash
rbenv local 1.9.2-p290
```

会在当前目录下生成 `.rbenv-version` 文件，此文件会覆盖 rbenv global 设定。

如果想取消的话，可以这样：

``` bash
rbenv local --unset
```

### 2.3  rbenv versions

显示所有版本，前面加 * 的为当前激活的版本。

``` bash
rbenv versions
```

### 2.4  rbenv rehash

每当安装新的 Ruby 版本，或 gem 都要运行一下，不然有可能会出现新安装的不起作用的现象：

``` bash
rbenv rehash
```

### 2.5 其它

当然还有其它命令，具体可以用 `rbenv help` 查看。

``` bash
rbenv help
```

## 3 最后

虽然 rbenv 提供的功能非常少，但对我来说者正是我需要的，less is more，其它的功能我根本不需要。

喜欢用 RVM gemset 的人，可以安装 [rbenv-gemset](https://github.com/jamis/rbenv-gemset) 插件来实现同样的功能。但还是用 [Bundler](http://gembundler.com/) 来管理应用依赖吧。

### 更新

如果你有安装 [Homebrew](http://mxcl.github.com/homebrew/) 的话，可以用以下命令来安装 rbenv 和 ruby-build：

``` bash
brew update
brew install rbenv
brew install ruby-build
```