### 登录

`ssh user@hostname`

登录到指定端口

`ssh user@hostname -p 22`

第一次登录提示：

```shell
The authenticity of host 'xxx.xxx.xxx.xxx (xxx.xxx.xxx.xxx)' can't be established.
ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入 `yes` 后回车，然后输入 `user` 的密码。（输密码时屏幕不显示）

该服务器的信息记录在本地主机的 `~/.ssh/known_hosts` 文件中。

`known_hosts`文件中内容：

```shell
xxx.xxx.xxx.xxx ssh-ed25519 AAAAC3NzaC1lZDIxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxRLzDIl2WgnlLfS3E9TW
xxx.xxx.xxx.xxx ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxNmvMPZ66vQouwRA7jy5Xn3BnWPvwY3YXXNIxxxxxxxxxxxxxxxxxxxxxxxEtHm8072Eb254ni3WVCiqc=
```

如果提示以下内容：（演示的本地主机为windows，linux同理）

```shell
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:pncLJlof4xD9F04tRvylSpArOtOAIul9FHEzQdoDjz0.
Please contact your system administrator.
Add correct host key in C:\\Users\\Qwer-laptop/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\Qwer-laptop/.ssh/known_hosts:1
Host key for 43.142.52.251 has changed and you have requested strict checking.
Host key verification failed.
```

把 `~/.ssh/known_hosts` 中的该服务器域名的信息删去，或者删去 `known_hosts` 文件。

### 配置免密登录

#### 配置别名

编辑 `config`

`vim ~/.ssh/config`

按 i 进入编辑模式，按需求输入以下内容：

```shell
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```

按 ESC 退出编辑模式，输入 `:wq` 保存并退出。

之后再使用服务器时，可以直接使用别名 `myserver1`、`myserver2`。

登录即 `ssh myserver1`

#### 创建密钥

`ssh-keygen`，然后一直回车（如果已存在密钥，则输入 y 覆盖）

```shell
PS C:\Users\Qwer-laptop\.ssh> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\Qwer-laptop/.ssh/id_rsa):
C:\Users\Qwer-laptop/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\Qwer-laptop/.ssh/id_rsa
Your public key has been saved in C:\Users\Qwer-laptop/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxx马赛克xxxxxxxxxxxxxxxxx qwer-laptop@Legion
The key's randomart image is:
+---[RSA 3072]----+
|xxxxxxxxx        |
|o+o B +o..       |
|=.o= E .O        |
|*xxxx马赛克xx     |
|+o    . S        |
|xx    马赛克      |
|xxx    xxxx      |
|xxxx             |
|xxxxx            |
+----[SHA256]-----+
```

#### 添加密钥

Linux 本地主机可以通过 `ssh-copy-id myserver` 一键添加公钥。

Windows 本地主机如下操作

将本地主机的`~/.ssh/id_rsa.pub`中的内容复制到 `myserver` 中的`~/.ssh/authorized_keys`文件里。

### ubuntu root 登陆

先用正常用户登陆, 进入超级用户模式

`sudo -i`

设置 root 密码

`passwd`

修改 sshd 配置文件

```bash
#PermitRootLogin prohibit-password
```

修改为

```bash
PermitRootLogin yes
```