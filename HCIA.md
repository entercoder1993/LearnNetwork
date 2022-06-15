# 网络通信基础

## 常见术语

## OSI模型和TCP/IP模型



# VRP基础

VRP（通用路由平台）是Versatile Routing Platform的简称，它是华为数据通信产品的通用网络操作系统。

## VRP命令行

命令行视图

* 用户视图：了解设备的基础信息、查询设备状态，但不能进行配置。
* 系统视图：可以使用绝大部分的基础功能配置命令。
* 接口视图：可以完成对相应接口的配置操作。

进入命令行界面后，首先进入的就是用户视图。<>表示的是用户视图。

在用户视图下使用system-veiw就可以进入系统视图。[]表示的是系统视图。使用`interface g0/0/1`进入接口视图。

命令级别和用户权限级别

* 0级（参观级）：网络诊断命令
* 1级（监控级）：查看网络状态和设备基本信息
* 2级（配置级）：对设备进行业务配置
* 3级（管理级）：特殊功能，例如上传或下载配置文件。

用户权限0～15共16个级别，默认3级就可以操作所有的命令。用户权限与提升命令级别的功能一起使用，可以对权限进行细分。

VRP支持不完整关键字输入和在线帮助`?`命令。

## 登录设备

通过Console口登录。

通过Telnet登录设备，需要对设备配置启用Telnet服务。

```

```

通过SSH登录，需要对设备配置启用SSH服务。

```
一，配置管理地址和接口vlan
[SW1]vlan 10
[SW1]interface vlan 10
[SW1-Vlanif10]ip address 192.168.10.100 24
[SW1]interface GigabitEthernet 0/0/1
[SW1-GigabitEthernet0/0/1]port link-type access
[SW1-GigabitEthernet0/0/1]port default vlan 10
二，开户stelnet权限
[SW1]stelnet server enable
三，创建本地密钥对
[SW1]rsa local-key-pair create
四，创建ssh用户名为user
[SW1]ssh user user //创建用户
[SW1]ssh user user authentication-type password //创建用户认证为密码认证
[SW1]ssh user user service-type stelnet //服务类型为ssh
五，进入aaa模式下配置用户
[SW1]aaa //进入aaa模式
[SW1-aaa]local-user user password cipher user123 //配置用户密码为user123
[SW1-aaa]local-user user privilege level 15 //配置用户权限级别
[SW1-aaa]local-user user service-type ssh //允许用户ssh访问权限
六，配置vty界面支持的登录协议
[SW1]user-interface vty 0 4
[SW1-ui-vty0-4]authentication-mode aaa //认证为aaa
[SW1-ui-vty0-4]protocol inbound ssh //登录协议为ssh登入
```

配置设备系统时钟

配置时区

```
clock timezone BJ add 08:00
```

配置日期和时间

```
clock datetime 02:06:00 2022-06-08
```

配置设备IP地址

IP地址是针对接口的配置，通常一个接口配置一个IP地址。假设e1/0/0为管理接口。

```
interface e1/0/0
ip address 10.1.1.100 255.255.255.0
```

用户界面配置

不同的用户拥有个字不同的用户界面。使用Telnet登录设备的用户，其用户界面对应了设备的虚拟VTY（Virtual Type Terminal，虚拟类型终端）接口。

用户验证方式

* Password验证：只需输入密码。使用该方式时，没有配置密码则无法登录。
* AAA验证：需要输入用户名和密码，使用Telnet登录时一般采用aaa验证。
* None验证：不需要输入用户名和密码可以直接登录设备。

通过Telnet登录的用户，权限级别为0级。

用户权限级别

默认情况下，用户级别在3级以上时，便可以操作设备的所有命令。

实例1:配置VTY用户界面

（1）配置最大VTY用户界面数为15

```
user-interface maximum-vty 15
```

（2）进入VTY用户界面视图

```
user-interface vty 0 4
```

（3）配置VTY用户界面的用户级别为2级

```
user privilege level 2
```

（4）配置VTY用户界面的用户验证方式为AAA

```
authentication-mode aaa
```

（5）配置AAA验证方式的用户名和密码

```
aaa
local-user admin password cipher admin@123
local-user admin service-type telnet
```

实例2:配置Console用户界面

（1）进入Console用户界面

```
user-interface console 0
```

（2）配置用户界面

```
authentication-mode password
set authentiation password cipher admin@123
```

