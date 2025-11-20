
> [!NOTE]
> 没法 root 就没办法用 sudo 命令, 只能先下一个 *tsu* 暂时用一下.
# Shell 
选择zsh
# IDE
选择LazyVim
# SSH 连接
## 手机上的操作
- 注意要在同一个局域网中操作

1. 首先安装 openssh

```shell
pkg install openssh
```

2. 设置密码

```shell
passwd
```

3. 查看 IP 地址

```shell
ifconfig
```
## 电脑上的操作
```shell
ssh -p <端口> <IP 地址>
```

接下来会问你密码,输入正确之后就能连接了