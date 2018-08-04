---
layout: post
title:  "在Vultr的VPS服务器上搭建ss、ssr及v2ray"
---

# 一、在vultr上购买主机

# 二、使用putty连接主机

打开putty，在主机名称（IP地址）文本框中输入主机的ip地址，然后点击“打开”。软件中出现`login as:`。这时，输入用户名`root`。然后出现一行代码，要求你输入密码，这时，复制购买的主机的密码，在putty界面右击就自动输入了密码，这时输入的主机密码是不可见的，但是你只要右击了就行，然后回车，出现一行新的代码`[root@vultr ~]#`就表示登录主机成功。

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