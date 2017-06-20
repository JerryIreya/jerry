# 生成SSH公钥

大多数Git服务器都会选择使用SSH公钥来进行授权。系统中的每个用户都必须提供一个公钥用于授权，没有的话就要生成一个。生成公钥的过程在所有操作系统上都差不多。首先先确认一下是否有一个公钥了。SSH公钥默认存储在账户的主目录下的`~/.ssh`目录。进去看看：


    $ cd ~/.ssh
    $ ls
    authorized_keys2  id_dsa       known_hosts
    config            id_dsa.pub
    
关键是看有没有用`something`和`something.pub`来命名的一对文件，这个`something`通常就是`id_dsa`或`id_rsa`。有`.pub`后缀的文件就是公钥，另一个文件则是密钥。加入没有这些文件，或者干脆连`.ssh`目录都没有，可以用`ssh-keygen`来创建。该程序在Linux上由SSH包提供：

    $ ssh-keygen    //命令
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/jerry/.ssh/id_rsa):  //在这里直接敲回车，就代表将生成的公钥和密钥存放到/home/jerry/.ssh/id_rsa 和 /home/jerry/.ssh/id_rsa.pub文件中了。
    Enter passphrase (empty for no passphrase):     //如果需要密码，就输入，不需要就直接回车。
    Enter same passphrase again:    //再次输入密码，没有密码就直接回车。
    Your identification has been saved in /home/jerry/.ssh/id_rsa.  //秘钥文件为id_rsa
    Your public key has been saved in /home/jerry/.ssh/id_rsa.pub.  //公钥文件为id_rsa.pub
    The key fingerprint is:
    43:c5:5b:5f:b1:f1:50:43:ad:20:a6:92:6a:1f:9a:3a schacon@agadorlaptop.local
    
    
这时在/home/jerry/.ssh/目录下就会生成id_rsa 和 id_rsa.pub文件，其中id_rsa.pub就是公钥，将里面的内容配置到服务器即可。
