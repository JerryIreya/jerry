# Linux下命令别名

## 实例

ubuntu下列出当前目录下的所有文件的命令：ll

但是Linux下其实没有这个命令，那ll命令是哪里来的呢？

我们先用type命令看一下ll命令的详情：

    $ type ll
    ll is aliased to 'ls -alF'

从字面意思可以看出，ll是'ls -alF'的别名，在命令行中执行ll就相当于执行'ls -alF'。那么命令的别名是怎么定义的呢？怎么查看当前系统中有哪些命令别名呢？

## 查看当前系统中的所有命令别名

使用alias命令可以查看当前系统中的所有命令别名

    $ alias
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'

可以看到ll的别名就是 'ls -alF'

## 自己定义命令别名

linux使用alias来定义命令别名，格式为：

    $ alias 别名='命令'

例如，我系统中有一个默认的python，它的版本是2.7.6，它在/usr/bin目录下，因为/usr/bin是添加到PATH中的，所以我在命令行直接执行python命令，就会默认执行这个python。

我又装了一个Python3.6，而且我没有把它添加到PATH中，那么我如果想要使用Python3.6，就需要进到Python3.6所在的目录，然后执行./python3.6才可以，如果我想python默认指向Python3.6怎么做呢？

可以使用命令别名来实现：

    $ alias python='/home/jerry/python3.6/python'

查看是否设置成功

    $ alias
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias python='~/Python-3.6.1/python'

可以看到 python指向3.6的python了


> 注意：这样定义的命令别名是临时生效的，在终端关闭或者重启之后就失效了。下面会说到怎么使别名永久生效

### 测试python别名好不好使

    $ python
    Python 3.6.1 (default, Jun 17 2017, 15:05:57)
    [GCC 4.8.5] on linux
    Type "help", "copyright", "credits" or "license" for more information.

可以看到，直接执行python命令，使用的是Python 3.6.1，说明设置成功了


## 取消命令别名

既然有定义别名，就肯定有取消别名，取消别名使用unalias命令，就是在alias前面加上un否定前缀

    $ unalias python

再查看所有命令别名

    $ alias
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'

刚刚设置的python别名没有了


## 别名永久生效

别名永久生效的办法就是将命令别名添加到~/.bashrc中，然后重新载入下文件就可以了

    $ vim ~/.bashrc

可以看到~/.bashrc文件中有如下设置：

    # some more ls alias
    alias ll='ls -alF'
    alias la='ls -A'
    alias l='ls -CF'

这就是系统为我们定义好的别名，也就是使用alias命令看到的结果，我们如果要持久化自定义的命令别名，也可以在该文件中加入我们的别名,将下面的配置添加到~/.bashrc文件即可：

    $ alias python='~/Python-3.6.1/python'

添加完之后我们要让刚刚的配置生效，就需要重新载入~/.bashrc，重新载入的命令如下：

    $ source ~/.bashrc

这样我们刚刚的配置就生效了，别名也就永久生效了。

这种方式我们配置的是当前用户主目录下的.bashrc文件，所以该配置只对当前用户生效，如果要让该系统上的所有用户都生效，就需要配置系统的bashrc了，该文件在Ubuntu14.04上的位置是：/etc/bash.bashrc.将刚刚的配置配到该文件，然后重新载入下(source /etc/bash.bashrc)即可。
