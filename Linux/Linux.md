# Linux

## UTC时区设置

~~~ 
# 删除当前时区文件
rm -f /etc/localtime
# 创建新的时区文件
ln -s /usr/share/zoneinfo/UTC /etc/localtime
# 时区信息
timedatectl
# 当前时间
date
~~~

## 同步网络时间

~~~
# 安装ntpdate工具
yum -y install ntp ntpdate
# 设置系统时间与网络时间同步
ntpdate 0.asia.pool.ntp.org
# 将系统时间写入硬件时间
hwclock --systohc
~~~

## 修改虚拟机IP

~~~
vim /etc/sysconfig/network-scripts/ifcfg-ens33
修改IPADDR和GATEWAY
查看网关
编辑-》虚拟网络编辑器-》更改设置-》NAT模式-》NAT设置
~~~



