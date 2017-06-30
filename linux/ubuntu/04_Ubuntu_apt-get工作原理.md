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
