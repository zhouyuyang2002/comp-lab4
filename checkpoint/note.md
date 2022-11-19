## 注释

测试环境为在 windows 10 主机上运行的 Ubuntu 18.04 虚拟机，虚拟机软件为 VMware

测试网络为 VMware 自带的虚拟网络，网络中主机 IP 地址为 192.168.230.1，用户名为 10679, 虚拟机地址为 192.168.230.231

windows 10 主机上 sftp 服务器端使用的是设置->应用->应用与功能->可选功能->添加功能->openssh 服务器。

## 使用

进行测试的时候 CP1,2,3,6 设置为 -DLOG_LEVEL=NOTICE、CP4,5 设置为 -DLOG_LEVEL=DEBUG

#### CP1-1

使用 sshd 命令询问 windows 的 sshd 服务器端版本, 软件版本 OpenSSH_for_Windows_7.7

#### CP1-2

在虚拟机上运行 mini-SFTP, 其输出版本序列为 SSH-2.0-OpenSSH_for_Windows_7.7， SSH 协议版本 2

#### CP2

在虚拟机上运行 mini-SFTP(和 CP1-2 来自同一运行记录)，输出每一个 method 的名字。
经检查，除去在协议中明确允许忽略的 language(第8,9个kex method)，其余均为服务器端唯一允许的加密方式。


#### CP3-2

在虚拟机上运行 mini-SFTP, 运行记录如 CP3-2 所示。将 wireshark 在此期间的监听记录保存在了 CP3.pcap 中

#### CP3-1

CP3.pcap 中第22帧的截图。不难发现数据均已经进行加密。

#### CP4-1

在虚拟机上运行 mini-SFTP, 运行记录如 CP4-1 所示。我们特地抓取了发送密码后服务器端返回的数据。此次输入密码正确。
经检查 0x34 = 52 代表 SSH_MSG_USERAUTH_SUCCESS, 因此我们身份验证成功

#### CP4-2

在虚拟机上运行 mini-SFTP, 运行记录如 CP4-2 所示。我们特地抓取了发送密码后服务器端返回的数据。此次输入密码错误。
经检查 0x34 = 51 代表 SSH_MSG_USERAUTH_FAILURE, 因此我们身份验证成功。

#### CP5

在虚拟机上运行 mini-SFTP, 运行记录如 CP5 所示。我们特地抓取了建立 channel 时候的包，并且将此时对应的信息输出。
server & client 端有着相同的 channel id = 1
client 端 window 有 64000, 但是 server 端的 window 只有 0. 其会等待后续操作增大 window 大小。

#### CP6-1
在虚拟机上运行 mini-SFTP, 运行记录如 CP6-1 所示。

#### CP6-2
执行完 put 后在 windows home 文件夹下新增的 client.c 的内容。

#### CP6-3
windows 文件管理器修改时间，其恰好是在 put 后进行的修改。

#### CP6-4
修改了 client.c，在其头部增加了注释。

#### CP6-5
linux 文件管理器修改时间，其恰好是在 get 后进行的修改。

#### CP6-6
执行完 put 后在 lab-sftp 文件夹下修改的 client.c 的内容

#### 备注

本次lab最终的源代码，以及CP3.pcap都使用mini-SFTP上传到windows端。







