### nifi访问限制


```
#编辑sshd_config文件
vi /etc/ssh/sshd_config

#禁用密码验证
PasswordAuthentication no

#启用密钥验证
RSAAuthentication yes
PubkeyAuthentication yes

#指定公钥数据库文件
AuthorsizedKeysFile.ssh/authorized_keys

#重启sshd服务
service sshd restart
```