首先在服务器防火墙打开9993,9994,3443端口Tcp和udp。

首先，安装Docker CE
更新系统源

```
sudo yum update
```

安装yum-utils工具

```
sudo yum -y install yum-utils
```

将软件包添加至本地缓存

```
sudo yum makecache fast
```

安装 Docker CE

```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

更新系统源

```
sudo yum update
```

```
sudo yum -y install docker-ce
```
启动自动服务

```
sudo systemctl enable docker
```

启动服务

```
sudo systemctl start docker
```
安装Git

```
yum install git
```

安装Planet
拉取安装脚本

```
git clone https://kgithub.com/vnge/zerotier.git
```

进入项目目录
```
cd /root/zerotier
```

运行 deploy.sh 脚本

```
./deploy.sh
```
端口输入9994，其他按照提示来。
等到出现下面的提示就表示成功安装了。

```
firewall-cmd --zone=public --add-port=3443/tcp --permanent
firewall-cmd --zone=public --add-port=3443/udp --permanent
firewall-cmd --zone=public --add-port=9993/tcp --permanent
firewall-cmd --zone=public --add-port=9993/udp --permanent
firewall-cmd --zone=public --add-port=9994/tcp --permanent
firewall-cmd --zone=public --add-port=9994/udp --permanent
firewall-cmd --reload
firewall-cmd --list-all
```


访问 http://ip:3443 进入controller页面
使用默认账号为:admin
默认密码为:password


客户端安装准备
安装好Planet后，在 /tmp/ 下有个 planet 文件，下载文件到本地

Windows版本设置
安装好ZeroTier客户端后。
把plant文件放入C:\ProgramData\ZeroTier\One
目录是隐藏的
重启ZeroTier服务

OpenWrt或者其他Linux
进入目录 /var/lib/zerotier-one
用下载的planet文件替换目录下的 planet 文件
重启服务 service zerotier-one restart
加入网络 zerotier-cli join 网络 id


zerotier-cli peers
如果看到只有一个Planet，那就成功了Leaf是客户端链接。



openwrt的Zerotier实现

防火墙配置
然后进行防火墙配置。
在软路由里找到网络防火墙，并切换到自定义规则


在后面添加如下：


```
iptables -I FORWARD -i zt6ovq7i62 -j ACCEPT
iptables -I FORWARD -o zt6ovq7i62 -j ACCEPT
iptables - t nat -I POSTROUTING -o zt6ovq7i62 -j MASQUERADE
```
注意这里的zt6ovq7i62要换上你的。



