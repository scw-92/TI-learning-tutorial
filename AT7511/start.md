CMI_AT751 BSP

##  产品启动说明
* 串口登录:
```sh
  1:请参考原理图制作debug port线，1角为 VCC，2角TX，3角RX，4角GND。
  2:注： 此信号为3.3V TTL 信号，如果需要与PC连接，通常PC需要一个USB转TTL的模块才能与此连接，切不可直接连接 RS232。
  3:Debug port 设置：波特率：115200，数据位：8，校验：none ， 停止位：1。
  4:登录账号：root, 密码：root。
```
* Vsftpd 服务使用介绍：
```sh
  1:Eth0 配置IP 192.168.1.1，Eth1 配置192.168.2.1
  2:请使用 ftp 客户端工具登录 CMI-AT151 ftp服务器。
  3:账号：root, 密码：root
```
* Ssh 服务登录：
```sh
  1:如果是连接 Eth0 在windows ssh 客户端输入 192.168.1.1
  2:登录账号：root, 密码：root。
```
