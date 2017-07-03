# mysql-cmd
mysql-open-cmd
myql - centos-cmd
========

CentOS下开启mysql远程连接，远程管理数据库

使用说明
-----------
登录mysql
  
```
mysql -u root -p mysql # 第1个mysql是执行命令，第2个mysql是系统数据名称

```

在mysql控制台执行:

```
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
# root是用户名，%代表任意主机，'123456'指定的登录密码（这个和本地的root密码可以设置不同的，互不影响）
flush privileges; # 重载系统权限
exit;
```

PS，上面iptables添加/删除规则都是临时的，如果需要重启后也生效，需要保存修改:

```
service iptables save # 或者: /etc/init.d/iptables save
```
centos6 iptable 和centos7 systemctl 是在 /etc/firewalld/zone/public.xml设定规则

```
vi /etc/sysconfig/iptables # 加上下面这行规则也是可以的
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```
centos7- 防火墙设定

```
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="dhcpv6-client"/>
  <service name="ssh"/>
  <port protocol="tcp" port="141"/>
  <port protocol="udp" port="141"/>
  <port protocol="tcp" port="3306"/>
  <port protocol="udp" port="3306"/> 这里是开通3306mysql的数据接口
</zone>

```
然后防火墙导入新的规则

```
firewall-cmd --reload
```
如果防火墙没开启是不能导入的这里先开启

```
systemctl start firewall  开启
systemctl stop firewall   关闭
systemctl status firewall 状态
```





