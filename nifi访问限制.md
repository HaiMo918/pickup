### 环境准备

__安装iptables服务__

``` Shell
yum install iptables
yum install iptables-services
```

__停止firewalld服务__

``` Shell
# 停止firewalld服务
systemctl stop firewalld
systemctl stop firewalld.service

# 禁用firewalld服务
systemctl mask firewalld

# 禁止firewall开机启动
systemctl disable firewall.service

# 查看默认防火墙状态
firewall-cmd --state
```

__添加白名单__

```Shell
# 白名单 -I
iptables -I INPUT -p tcp -s 192.168.150.143 --dport 22 -j ACCEPT

# 黑名单 DROP
iptables -A INPUT -p tcp --dport 22 -j DROP
```

__启动iptables服务__

```
# 启动iptables服务
systemctl restart iptables.service

# 停止iptables服务
systemctl stop iptables.service

# 查看iptables服务状态
systemctl status iptables.service

# 设置iptables开机启动
systemctl enable iptables.service
```

__查看iptables以添加的规则__

```
# 查看iptables
iptables -L -n -v
```