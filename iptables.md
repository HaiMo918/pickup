# iptables命令

__iptables命令__是Linux上常用的防火墙软件，是netfilter项目的一部分。可以直接配置，也可以通过许多前端和图形界面配置。

资料来源：[http://man.linuxde.net/iptables](http://man.linuxde.net/iptables)

> 语法

```
iptables(选项)(参数)
```

> 选项

```
-t<表>：指定要操纵的表；
-A：向规则链中添加条目；
-D：从规则链中删除条目；
-i：向规则链中插入条目；
-R：替换规则链中的条目；
-L：显示规则链中已有的条目；
-F：清楚规则链中已有的条目；
-Z：清空规则链中的数据包计算器和字节计数器；
-N：创建新的用户自定义规则链；
-P：定义规则链中的默认目标；
-h：显示帮助信息；
-p：指定要匹配的数据包协议类型；
-s：指定要匹配的数据包源ip地址；
-j<目标>：指定要跳转的目标；
-i<网络接口>：指定数据包进入本机的网络接口；
-o<网络接口>：指定数据包要离开本机所使用的网络接口
```

__iptables命令选项输入顺序：__

```
iptables -t 表名 <-A/I/D/R> 规则链名 [规则号] <-i/o 网卡名> -p 协议名 <-s 源IP/源子网> --sport 源端口 <-d 目标IP/目标子网> --dport 目标端口 -j 动作
```

__表名包括：__

* raw
* mangle
* net
* filter

__规则链名包括：__

* INPUT：处理输入数据包。
* OUTPUT：处理输出数据包
* PORWARD：处理转发数据包
* PREROUTING：用于目标地址转换(DNAT)。
* POSTOUTING：用于源地址转换(SNAT)。

__动作包括：__

* ACCEPT：接收数据包。
* DROP：丢弃数据包。
* DEDIRECT：重定向、映射、透明代理。
* SNAT：原地址转换。
* DNAT：目标地址转换。
* MASQUERADE：IP伪装(NAT),用于ADSL。
* LOG：日志记录。

> 实例

__清除已有iptables规则__

``` Shell
iptables -F
iptables -X
iptables -Z
```

__开放指定的端口__

``` Shell
iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT #允许本地回环接口(即运行本机访问本机)
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT #允许已建立的或相关连的通行
iptables -A OUTPUT -j ACCEPT #允许所有本机向外的访问
iptables -A INPUT -p tcp --dport 22 -j ACCEPT #允许访问22端口
iptables -A INPUT -p tcp --dport 80 -j ACCEPT #允许访问80端口
iptables -A INPUT -p tcp --dport 21 -j ACCEPT #允许ftp服务的21端口
iptables -A INPUT -p tcp --dport 20 -j ACCEPT #允许FTP服务的20端口
iptables -A INPUT -j reject #禁止其他未允许的规则访问
iptables -A FORWARD -j REJECT #禁止其他未允许的规则访问
```

__屏蔽IP__

``` Shell
iptables -L -n -v
Chain INPUT (policy ACCEPT 231K packets, 81M bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 208K packets, 156M bytes)
 pkts bytes target     prot opt in     out     source               destination   
```

__查看已添加的iptables规则__

``` Shell

```

__删除已添加的iptables规则__

将所有iptables以序号标记显示,执行：

``` Shell
iptables -L -n --line-numbers
```

比如要删除INPUT里序号为8的规则,执行：

``` Shell
iptables -D INPUT 2
```

```

iptables -A INPUT -p tcp --dport 22 -j DROP
```

> 参考资料

[http://blog.csdn.net/l1028386804/article/details/50779761](http://blog.csdn.net/l1028386804/article/details/50779761)