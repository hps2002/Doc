# 背景
国庆在宿舍非常无聊，于是看着平板陷入了沉思，这么好的平板要是可以用来做开发的话，那就美滋滋了，于是萌发起了一个在平板上连接远程服务器的想法。

# 安装Termux
我在这里使用的是 `Termux` ，因为我的是华为MatePad 11，曾经遥遥领先。在应用商店可能下载不了，因为这是一个开源的软件，我们需要在github上进行下载。  
[下载入口](https://github.com/termux/termux-app/releases)，在里面选择选择合适你的CPU版本进行下载，安装完成就开始了最tmd难受的装环境之路。

进入界面之后显示的就是 `Termux` 的主界面，注意：`Termux` 和你的操作系统是分开的，此时的终端是无法访问你的内部存储。且由于权限关系，我们像Ubuntu无法那样轻易的使用 `root` 权限。

安装 `SSH` ，首先安装 `openssl` 用于证书的解析和加密，再安装 `openssh` 用于主机之间的 `SSH` 连接。

```sh
pkg install openssl
pkg install openssh
ssh -V # 查看是否安装成功并且添加路径
```

我们切换到 `.ssh的根目录` 

```sh
cd ~/.ssh/
```

在 `vim` 编辑器修改 `config` 文件，如果需要安装vim的话也是使用 `pkg install <package_name>` 进行安装。

`config` 文件的格式如下

```config
Host YourHostName
    Hostname server's ip
    user server's user
    IdentityFile "公钥文件" #可以没有，如果不添加这个属性的话就要输入密码
```

此时还不能进行连接，因为主机端没有服务端的ssh公钥，所以我们需要拿到服务端的ssh公钥，将你主机端的ssh公钥拷贝到平板中，并且执行 `pkg-setup-storage` 获取访问存储权限。然后将其复制到平板下的 `.ssh/` 下即可。

然后再服务器的防火墙中打开 `8022/tcp` 端口，因为Termux默认使用8022端口进行连接。  

最后输入密码即可连接成功。

如果不想每次输入都使用密码的话，将客户端的公钥传输到主机上，以后即可免密登录

# SSH连接的原理
参考（https://zhuanlan.zhihu.com/p/267701810?utm_id=0）
