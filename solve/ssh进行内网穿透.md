## 最近在实习的过程中遇到了一个需要使用穿透的web服务。
背景大概是这样的：有两台公网服务器，其中一台是对外的服务器，另外一台服务器只开启了22端口用于ssh远程连接。因为对外服务的服务器配置非常低，所以要在另外一台服务器上进行部署web服务。因此使用ssh进行穿透使得外部能访问另外一台服务器的web服务。

执行ssh命令 `ssh -R <remote_port>:localhost:<local_port> -N <remote_user>@<remote_ip>` 

表示将远程服务器的端口接收到的所有消息通过 `ssh` 都转发到当前的本地服务器的端口中.

但是这条ssh连接会出现一个问题：由于tcp保活机制的原因，超过限制的时间没有消息传输会断开连接。

解决这个问题有两个方法解决：
（1）通过在后端编写一个定时触发器：在对应的时间内向对面的服务器发送一条保活信息。
（2）通过修改ssh_config保活配置进行延时；