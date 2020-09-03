# Awesome Mirrors [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)


由于众所周知的原因，中国的开发者在下载各种依赖的时候速度都比较慢。因此部分有实力的高校、公司在国内搭建了方便开发者的各种镜像仓库，并且几乎都是和国外的源定时同步的。本仓库主要收集方便中国开发者提速的源，以及配置的方式。

下面的操作方式在不同的系统可能会有细微的差别，欢迎大家补充和纠正。

## 目录

+ [Docker](#docker)
+ [Nodejs](#nodejs)
+ [Python](#python)
+ [Java](#java)
+ [Goproxy](#goproxy)
+ [Ruby](#ruby)
+ [Alpine apk](#alpine-apk)
+ [Centos yum](#centos-yum)
+ [Debian apt](#debian-apt)
+ [Ubuntu apt](#ubuntu-apt)
+ [Homebrew](#homebrew)
+ [iOS](#ios)


## Docker

首先从网上搜索国内的docker registry源，然后修改docker的配置并**重启**docker。在这里我比较推荐使用免费的阿里云镜像加速器（至少截至2020年09月03日还是免费的），目前使用一直比较平稳。

- 获取镜像加速url

注册一个阿里云账号并登录，在`产品与服务`中搜索`容器镜像服务`，跟随引导完成必要的一些步骤，然后来到这个[页面](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)，就可以看到自己专有的加速器地址了。

- 给docker客户端配置镜像加速器

这里以MacOS为例，其他系统类似。如果没有`/etc/docker/daemon.json`文件，则可以直接通过下面的命令完成配置；如果该文件已经存在了，则选择自己熟悉的文本编辑器编辑该文件，添加加速器地址的配置。如果你使用的带界面的客户端，也可以在`Preferences... -> Docker Engine`显示的编辑器中添加相应的配置。

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
EOF
```

**注意**：将`https://xxx.mirror.aliyuncs.com`替换成你自己的镜像加速器地址。再次提醒，配置完成后，**重启**docker之后才会生效。

除了阿里云的源，还有一些其他的源可以参考：
1. https://registry.docker-cn.com
2. http://hub-mirror.c.163.com
3. http://docker.mirrors.ustc.edu.cn
4. http://mirror.azure.cn/help/docker-registry-proxy-cache.html

## Nodejs

下面是NPM的配置方式，以淘宝的源为例。

* 临时使用

```shell
# 为单用户安装
npm install --registry https://registry.npm.taobao.org <package-name>

#为所有用户安装
npm install --global --registry https://registry.npm.taobao.org <package-name>

# yarn
yarn save <package-name> --registry https://registry.npm.taobao.org
```

* 默认使用

```sh
# npm
npm set registry https://registry.npm.taobao.org

# yarn
yarn config set registry https://registry.npm.taobao.org
```

除了淘宝的源，还有一些其他的源可以参考：

1. http://crproxy.trafficmanager.net:4873

另外，可以通过[`nrm`](https://github.com/Pana/nrm)管理多个NPM的源，相应的`yarn`也有[`yrm`](https://github.com/i5ting/yrm)。

## Python

PyPI (Python Package Index) 是 Python 编程语言的软件存储库。开发者可以通过 PyPI 查找和安装由 Python 社区开发和共享的软件，也可以将自己开发的库上传至 PyPI 。这里以阿里云的源为例。

* 临时使用

```shell
pip install -i https://mirrors.aliyun.com/pypi/simple <package-name>
```

注意，`simple` 不能少, 是 `https` 而不是 `http`

* 默认使用

升级 pip 到最新的版本 (>=10.0.0) 后进行配置：

```shell
pip install pip -U
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

除了阿里云的源，还有一些其他的源可以参考：

1. https://pypi.tuna.tsinghua.edu.cn/simple
2. https://pypi.mirrors.ustc.edu.cn/simple
3. https://pypi.douban.com/simple
4. http://mirror.azure.cn/pypi/simple

`Anaconda`的配置方式可以参考清华大学的[Anaconda镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda)

## Java

* Maven 配置

打开 Maven 的配置文件(windows机器一般在maven安装目录的conf/settings.xml)，在`<mirrors></mirrors>`标签中添加 mirror 子节点:

```xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

如果想使用其它代理仓库,可在`<repositories></repositories>`节点中加入对应的仓库使用地址。以使用spring代理仓为例：

```xml
<repository>
    <id>spring</id>
    <url>https://maven.aliyun.com/repository/spring</url>
    <releases>
        <enabled>true</enabled>
    </releases>
    <snapshots>
        <enabled>true</enabled>
    </snapshots>
</repository>
```

* gradle 配置

在 build.gradle 文件中加入以下代码:

```groovy
allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public/' }
        mavenLocal()
        mavenCentral()
    }
}
```

如果想使用 maven.aliyun.com 提供的其它代理仓，以使用 spring 仓为例，代码如下:

```groovy
allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public/' }
        maven { url 'https://maven.aliyun.com/repository/spring/'}
        mavenLocal()
        mavenCentral()
    }
}
```

除了Spring的仓库之外，阿里云还提供一些其他的源：

1. central https://maven.aliyun.com/repository/central
2. jcenter https://maven.aliyun.com/repository/public
3. public https://maven.aliyun.com/repository/public
4. google https://maven.aliyun.com/repository/google
5. gradle-plugin https://maven.aliyun.com/repository/gradle-plugin
6. spring https://maven.aliyun.com/repository/spring
7. spring-plugin https://maven.aliyun.com/repository/spring-plugin
8. grails-core https://maven.aliyun.com/repository/grails-core
9. apache snapshots https://maven.aliyun.com/repository/apache-snapshots

## Goproxy

使用go1.11以上版本并开启go module机制，下面以阿里云的源为例：

```shell
export GO111MODULE=on
export GOPROXY=https://mirrors.aliyun.com/goproxy
```

除了阿里云的源，还有一些其他的源可以参考：

1. https://goproxy.cn
2. https://goproxy.io

## Ruby

这里以配置`Ruby China`的源为例：

```shell
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
```

`Bundler`用户参考如下配置：

```shell
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```

这样你**不用**改你的`Gemfile`的`source`。

```
source 'https://rubygems.org/'
gem 'rails', '4.2.5'
...
```

除了[Ruby China](https://gems.ruby-china.com)的源，还有一些其他的源可以参考：

1. https://mirrors.aliyun.com/rubygems
2. https://mirrors.tuna.tsinghua.edu.cn/rubygems
3. https://mirrors.ustc.edu.cn/rubygems
4. http://mirror.azure.cn/rubygems

## Alpine apk

Alpine Linux 是一个面向安全，轻量级的基于musl libc与busybox项目的Linux发行版。

下面以配置阿里云的源作为例子：

```shell
sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
```

除了阿里云的源，还有一些其他的源可以参考：

1. dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn

## Centos yum

CentOS，是基于 Red Hat Linux 提供的可自由使用源代码的企业级 Linux 发行版本；是一个稳定，可预测，可管理和可复制的免费企业级计算平台。

更换源之前，建议先备份一下，下面以阿里云的源为例：

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

下载阿里云的源：

```shell
# Centos 6
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-6.repo
# Centos 7
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
# Centos 8
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo
```

生成缓存：

```shell
yum makecache
```

**注意：** 非阿里云ECS用户会出现 `Couldn't resolve host 'mirrors.cloud.aliyuncs.com' `信息，不影响使用。用户也可自行修改相关配置：

```shell
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
```

除了阿里云的源，还有一些其他的源可以参考：

1. https://mirrors.tuna.tsinghua.edu.cn/help/centos
2. http://mirrors.ustc.edu.cn/help/centos.html
3. http://mirror.azure.cn/help/centos.html

## Debian apt

Debian GNU/Linux ，是一个操作系统及自由软件的发行版，由一群自愿付出时间和精力的用户来维护并更新。它附带了超过 59000 个软件包，这些预先编译好的软件被打包成一种良好的格式以便于用户安装和使用。

下面以阿里云的源为例：

**debian 7.x (wheezy)**

编辑/etc/apt/sources.list文件(需要使用sudo), 在文件最前面添加以下条目(操作前请做好相应备份)

```
deb http://mirrors.aliyun.com/debian/ wheezy main non-free contrib
deb http://mirrors.aliyun.com/debian/ wheezy-proposed-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ wheezy main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ wheezy-proposed-updates main non-free contrib
```

**debian 8.x (jessie)**

编辑/etc/apt/sources.list文件(需要使用sudo), 在文件最前面添加以下条目(操作前请做好相应备份)

```
deb http://mirrors.aliyun.com/debian/ jessie main non-free contrib
deb http://mirrors.aliyun.com/debian/ jessie-proposed-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ jessie main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ jessie-proposed-updates main non-free contrib
```

**debian 9.x (stretch)**

编辑/etc/apt/sources.list文件(需要使用sudo), 在文件最前面添加以下条目(操作前请做好相应备份)

```
deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb http://mirrors.aliyun.com/debian-security stretch/updates main
deb-src http://mirrors.aliyun.com/debian-security stretch/updates main
deb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
deb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
```

除了阿里云的源，还有一些其他的源可以参考：

1. https://mirrors.tuna.tsinghua.edu.cn/help/debian
2. http://mirrors.ustc.edu.cn/help/debian.html

## Ubuntu apt

Ubuntu，是一款基于 Debian Linux 的以桌面应用为主的操作系统，内容涵盖文字处理、电子邮件、软件开发工具和 Web 服务等，可供用户免费下载、使用和分享。

下面以阿里云的源为例：

用你熟悉的编辑器打开`/etc/apt/sources.list`，替换默认的`http://archive.ubuntu.com/`为`mirrors.aliyun.com`。

### ubuntu 16.04 配置如下

```
deb http://mirrors.aliyun.com/ubuntu/ xenial main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

### ubuntu 18.04(bionic) 配置如下

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

### ubuntu 20.04(focal) 配置如下

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

除了阿里云的源，还有一些其他的源可以参考：

1. https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu
2. http://mirrors.ustc.edu.cn/help/ubuntu.html
3. http://mirror.azure.cn/help/ubuntu.html

## Homebrew

Homebrew 是一款自由及开放源代码的软件包管理系统，用以简化 macOS 系统上的软件安装过程。它拥有安装、卸载、更新、查看、搜索等很多实用的功能，通过简单的一条指令，就可以实现包管理，十分方便快捷。

下面以阿里云的源为例：

| 名称             | 说明                                  |
| :--------------- | :------------------------------------ |
| brew             | Homebrew 源代码仓库                   |
| homebrew-core    | Homebrew 核心源                       |
| homebrew-cask    | 提供 macOS 应用和大型二进制文件的安装 |
| homebrew-bottles | 预编译二进制软件包                    |

* Bash 终端配置

```shell
# 替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
# 替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
# 应用生效
brew update
# 替换homebrew-bottles:
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```

* 恢复默认配置

出于某些场景, 可能需要回退到默认配置, 你可以通过下述方式回退到默认配置。

首先执行下述命令:

```shell
# 重置brew.git:
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
# 重置homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

然后删掉 `HOMEBREW_BOTTLE_DOMAIN` 环境变量,将你终端文件`~/.bash_profile`中`HOMEBREW_BOTTLE_DOMAIN`行删掉, 并执行`source ~/.bash_profile`。

除了阿里云的源，还有一些其他的源可以参考：

1. https://mirrors.tuna.tsinghua.edu.cn/help/homebrew
2. http://mirrors.ustc.edu.cn/help/brew.git.html

## iOS

`CocoaPods` 是一个 Cocoa 和 Cocoa Touch 框架的依赖管理器，具体原理和 [`Homebrew` ](#homebrew)有点类似，都是从 GitHub 下载索引，然后根据索引下载依赖的源代码。

下面以清华大学的源为例：

对于旧版的 CocoaPods 可以使用如下方法使用 tuna 的镜像：

```
$ pod repo remove master
$ pod repo add master https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git
$ pod repo update
```

新版的 CocoaPods 不允许用`pod repo add`直接添加master库了，但是依然可以：

```
$ cd ~/.cocoapods/repos 
$ pod repo remove master
$ git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git master
```

最后进入自己的工程，在自己工程的`podFile`第一行加上：

```
source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
```

**注意：** 从[`1.7.2`](http://blog.cocoapods.org/CocoaPods-1.7.2/)开始，已经完全切到`CDN`上了。[`1.8`](http://blog.cocoapods.org/CocoaPods-1.8.0-beta/)以上甚至把`CDN`作为默认源使用，在`Podfile`最上面添加即可。

```
source 'https://cdn.cocoapods.org/'
```

除了清华大学的源，还有一些其他的源可以参考：

1. https://gitclub.cn/CocoaPods/Specs.git
2. http://git.oschina.net/akuandev/Specs.git
