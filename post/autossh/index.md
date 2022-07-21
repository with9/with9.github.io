
## 起因

所里为了确保安全，是禁止服务器开放对外部访问的，为了可以在宿舍也能登陆服务器进行办公，尝试了各种奇怪方法，之前一直是利用tmate这个灵车方案的，很不稳定，无意看到autossh可以端口转发，就打算试一下，非常简单，把公钥传送给远程服务器，然后运行autossh就可以啦。

```shell
#A 是所内服务器地址
#B 是我租的腾讯云学生服务器地址

auautossh -M 4010 -fNR 4000:A:22 usename@B #就可以把A服务器的22端口转发到B服务器啦
ssh usename@B -p 4000 #这里用户名要填A服务器的用户名。
```



## 二

一切都满顺利的,但是有个问题,这个服务器不是我一个人在用,大家也不接受只可以密钥登陆的方式,暴露在公网还允许密码登陆就非常危险了,于是我就打算在这里面跑一台虚拟机,把虚拟机的ssh端口转发到B服务器上.

### 建立虚拟机

```shell
qemu-img create -f qcow2 arch.qcow2 20G #建立虚拟磁盘
qemu-system-x86_64 archlinux.iso -drive file=/home/with/qemu/arch.qcow2  -m 4G  -nic user,hostfwd=tcp::10022-:22 -vnc :0 -smp cores=2,threads=1,sockets=1 -monitor unix:arch-socket,server,nowait 
# 一些参数说明
# -monitor: 把控制台转发到一个文件,之后可以利用socat来发送命令
# -nic    : 网络,并把虚拟机的22端口转发到10022上

# 然后vnc登陆进去照着wiki安装一下就行,安装sshd服务,并启用,配置禁止密码登陆
```

### 端口转发

```shell
#回到宿主机
auautossh -M 4010 -fNR 4000:A:10022 usename@B #把虚拟机的22端口转发到B服务器的4000上
ssh usename@B -p 4000 #这里用户名要填虚拟机的用户名。
```

### 补充

#### 文件传送
为了传送文件,打算搞一个[virtfs](https://wiki.qemu.org/Documentation/9psetup),也很简单

```shell
#加上-virtfs 参数就行
qemu-system-x86_64 ...... -virtfs local,path=/data2/qemu_temp,mount_tag=temp,security_model=none 

# 进入虚拟机
sudo mount -t 9p -o trans=virtio,access=any temp /temp_qemu #挂载上,之后就可以rsync或者scp把文件传上去了
```

#### tmux管理

首先连接不是太稳定,然后要访问的服务器也有点点多,所以打算用tmux来管理.

```shell
> cat .tmux.conf
unbind C-b 
set -g prefix C-a #修改prefix,避免冲突

> cat .bashrc
...
...
tmux attach # 一登陆就打开tmux,避免重链

autossh -M 5100 usename@B -p 4000 # 通过autossh来链接,会自动重连,比较方便
```

最后附上截图

![image-20220409124541099](/img/autossh/Screenshot_20220721_164330.png)
