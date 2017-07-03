# apt-get 工作原理

先介绍几个和apt-get相关的目录：

* /var/lib/dpkg/available

> 文件的内容是软件包的描述信息，该软件包括当前系统所使用的Debian安装源中的所有软件包，其中包括当前系统中已安装和未安装的软件包。

* /var/cache/apt/archives/

>目录是在用apt-get install安装软件时，软件包的临时存放路径

* /etc/apt/sources.list

> 文件中存放的是软件源站点，当你执行sudo apt-get install xxx时，ubuntu就去这些站点下载软件包到本地并执行安装

* /var/lib/apt/lists/

> 使用apt-get update命令会从/etc/apt/soureces.list中下载软件列表，并保存到该目录

## APT工作原理：

Ubuntu采用集中式的软件仓库机制，将各式各样的软件包分门别类地存放在软件仓库中，进行有效地组织和管理。然后，将软件仓库置于许许多多的镜像服务器中，并保持基本一致。这样，所有的Ubuntu用户随时都能获得最新版本的安装软件包。因此，对于用户，这些镜像服务器就是他们的软件源(Reposity)。

然而，由于每位用户所处的网络环境不同，不可能随意地访问各镜像站点。为了能够有选择地访问，在Ubuntu系统中，使用软件源配置文件/etc/apt/sources.list列出最合适访问的镜像站点地址。

## apt-get 的更新过程

* 执行apt-get update
* 程序分析 /etc/apt/sources.list
* 自动连网寻找list中对应的Packages/Sources/Release列表文件，如果有更新则下载之，存入/var/lib/apt/lists/目录
* 然后 apt-get install 相应的包，下载并安装。

即使这样，软件源配置文件只是告知Ubuntu系统可以访问的镜像站点地址，但那些镜像站点具体都拥有什么软件资源并不清除。若每安装一个软件包，就在服务器上寻找一遍，效率是很低的。因而，就有必要为这些软件资源列个清单(建立索引文件)，以便本地主机查询。

apt-get install 下载的软件存放到/var/cache/apt/archives/下。

同时，apt能够检查Ubuntu Linux系统中的软件包依赖关系，大大简化了Ubuntu用户安装和卸载软件包的过程。

## apt-get install 原理
