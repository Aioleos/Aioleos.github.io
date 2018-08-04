---
layout: post
title:  "在Vultr的VPS服务器上搭建ss、ssr及v2ray"
---
# 一、在vultr上购买主机
# 二、使用putty连接主机
## （一）、打开putty，在主机名称（IP地址）文本框中输入主机的ip地址，然后点击“打开”
## （二）、软件中出现
`login as:`
这时，输入用户名
`root`

## （三）、然后出现一行代码，要求你输入密码，这时，复制购买的主机的密码，在putty界面右击就自动输入了密码，这时输入的主机密码是不可见的，但是你只要右击了就行，然后回车，出现一行新的代码
`[root@vultr ~]#`
就表示登录主机成功
# 三、安装googlel的bbr，开源提速工具
依次输入下面三行代码，等前一行代码在主机中运行完毕后输入后一行代码。

`wget –no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh`

`chmod +x bbr.sh`

`./bbr.sh`
安装完bbr之后，会出现这样一行代码

`Info: The system needs to be restart. Do you want to reboot? [y/n]`
这时输入`y`，回车。
等待主机重启，这时putty与主机的连接会中断。弹出一个对话框，直接点击“确定”就行了。然后关闭putty，重新连接主机，进行下一步，安装ss

如果出现问题，可以使用下面的代码检查

`uname -r`

或者输入这个代码

`sysctl net.ipv4.tcp_available_congestion_control`
然后看是否有返回

`net.ipv4.tcp_available_congestion_control = bbr cubic reno`

或者输入

`lsmod | grep bbr`
看是否有bbr字样

# 四、安装ss
输入第一行代码

`wget --no-check-certificate  https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh`

输入第二行代码

`chmod +x shadowsocks.sh`

输入第三行代码

`./shadowsocks.sh 2>&1 | tee shadowsocks.log`

然后会要求你输入ss的密码，这时可以自己设置ss的密码，这里的密码输入是可见的。
然后设置远程端口，可以使用默认设置。
然后设置加密方式，可以使用的加密方式很多，这里可以使用第7中加密方式aes-256-cfb
至此，就安心等待安装ss的完成吧。
完成之后，会统一显示主机ip，ss密码，加密方式和远程端口的信息，将这些信息复制里面，就可以使用这些信息来使用ss了。

# 五、安装ssr
以下三个脚本可以任意选择一个来使用。

`wget --no-check-certificate https://freed.ga/github/shadowsocksR.sh; bash shadowsocksR.sh`

`wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh`

`wget http://vpn.ximcx.cn/SSR/SSR && bash SSR`
每个脚本都有些许不同，但是ssr服务都是一样的，主要的区别也只是在于一些配置上的自动化与否。

# 六、安装v2ray
由于ss及ssr的封禁越来越厉害，今天又学习了一下搭建v2ray。

## （一）安装
`bash <(curl -L -s https://install.direct/go.sh)`

## （二）配置
`vim /etc/v2ray/config.json`

## （三）启动
`systemctl start v2ray`

## （四）控制
`systemctlstart|restart|stop|status v2ray`

## （五）其他
也可以安装233blog脚本，这个脚本比较方便，可以不用手动去配置，即不用通过第二步中的vim命令去配置，这个脚本是直接在安装的时候提供配置的选择及配置的填写，比vim命令修改配置文件要方便很多。
`bash <(curl -s -L https://233blog.com/v2ray.sh)`
一路根据脚本提示来安装及配置就可以了。













# 迅雷下载宝刷固件之旅

## 一、由来
之前买了一个迅雷下载宝，挂机下载，又不费电，还是挺好用的，但是最近经常出现下载器不在线的问题，就想刷刷别人定制的固件，找了两个，一个是[【小白教程】迅雷下载宝刷H大的Padavan教程](http://www.right.com.cn/forum/thread-200183-1-1.html)，另一个是[【满血复活】迅雷下载宝定制固件（Luci+NFS+PT+Aria2+FTP+Ngrok+远程挂载NAS或硬盘 ](http://www.right.com.cn/forum/forum.php?mod=viewthread&tid=186873&page=1&authorid=307731)。前一个是Padavan固件，后一个是利用sd卡外挂optware运行环境，所以后一个还是可以保留原生的迅雷下载宝功能，而前一个会覆盖掉原生功能。考虑到有时迅雷下载宝的下载速度还是很可观的，就使用了后一个固件。这两个固件我只尝试了后一个，我估计最大的区别还是后一个用sd卡实现外挂增加了很多功能，而前一个就是直接将固件刷入下载包的rom中，当然二者的界面也是很不相同，功能上也有一些差异，但是对于下载宝来说，主要的下载功能两者都具备了。
下面，就开始记录一下我刷后一个固件的操作，当然这些操作都是看的这个教程[【满血复活】迅雷下载宝定制固件（Luci+NFS+PT+Aria2+FTP+Ngrok+远程挂载NAS或硬盘 ](http://www.right.com.cn/forum/forum.php?mod=viewthread&tid=186873&page=1&authorid=307731)。

## 二、刷Bootloader
如果之前刷过别的Bootloader，比如刷了breed的，必须将Bootloader刷回官方的uboot，如果没有刷过其他的Bootloader，就不用操作这一步。具体的恢复uboot的方法，我没有具体操作，需要的朋友就直接参考上面的教程吧！

## 三、升级迅雷下载宝固件
这个固件是迅雷官方的固件，不是第三方的固件，这里的升级也不会带来第三方功能的增加，如果不需要第三方功能，也可以只升级迅雷下载宝本身的固件。

[链接](https://pan.baidu.com/s/1pNokoKb)，密码：i4js

在百度网盘上下载好需要用的固件，将整个文件夹都下载到本地。

升级方法：

在浏览器登录以下地址（确保浏览器所在的电脑与下载宝在同一路由环境下），`http://下载宝IP:81/upgrade.do`，这里的“下载宝IP”是在局域网内路由器自动分配给下载宝的ip地址，可以在迅雷下载宝的手机App中查到。登录之后，操作很简单，可以看到一个“选择文件”的按键和一个“Apply”的按键，首先点击“选择文件”选择上面下载的固件fw-7621-xiazaibao-338.171012.bin这个文件，然后点击“Apply”。等待升级完成。

## 四、刷入外挂功能
将上面下载的文件中的rom.tar直接拷贝到sd卡中。不需要解压，直接拷贝就行。将sd卡插入下载宝，等待刷入固件。

## 五、登录luci界面
刷入以上第四步的第三方固件后，就可以登录Luci界面使用第三方功能，登录以下地址就可以登录到luci界面，`http://下载宝IP`，这里的“下载宝IP”与第三步中的是一样的。登录需要账号密码，账号是root，密码是admin

目前我只使用了aria2功能，其他的功能还没有尝试过。aria2功能也就是下载功能，可以实现http。bt，磁力，ftp等下载功能。aria2的服务端程序已经集成在了该第三方固件中，只需要在aria2服务中进行一下配置就可以使用。

具体的配置就是设置token用于加密，就相当于使用账号密码登录服务一样。然后更新一下tracker列表，可以使用[tracker链接](https://github.com/ngosang/trackerslist)中的tracker中的列表，最后勾选“启动”，然后保存并应用就可以了。

## 六、登录aria2管理界面
登录aria2的管理界面用来管理下载操作，就是添加下载链接、文件进行下载管理的界面，这里使用[Aria2 Web 控制台](http://aria2c.com/)进行管理。在设置（一个长得像扳手的图标）中，
JSON-RPC Path设置为这样的格式`http://token:xxx@下载宝IP:6800/jsonrpc`，其中token后面的xxx表示你在luci界面中设置的token值。然后就可以愉快的下载了。
