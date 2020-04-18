Ubuntu18安装后需要作的事情

#### 一.通用

##### 1.更新软件源：软件和更新--更新为阿里或者清华的源

2.更新系统

```
	$ sudo apt update
	$ sudo apt upgrade
	$ sudo apt dist-upgrade
```

##### 3.win10与ubuntu18时间不一致

```
$ sudo timedatectl set-local-rtc 1  #将硬件时间UTC改为CST，双系统时间保持一致，重启
$ sudo apt-get install ntpdate
$ sudo ntpdate time.windows.com  #先在ubuntu下更新一下时间，确保时间无误
$ sudo hwclock --localtime --systohc  #将时间更新到硬件上，重新进入windows系统，时间恢复正常
```

###### 3.1ubuntu server 修改时区和时间
首先查看时区 : $ date -R
修改时区 : $ sudo tzselect #选择asia,china,beijing,yes
复制文件到/etc目录下 : $ sudo cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
更新时间 : $ sudo ntpdate time.windows.com
在修改时间后修改硬件CMOS的时间 : $ sudo hwclock --systohc //非常重要，如果没有这一步的话，后面时间还是不准
###### 3.2Virtual box的虚拟机ubuntu无权限访问共享文件夹
在虚拟机下查看共享文件夹的属性，发现该目录的所有者是root，所属组是vboxsf。
解决方法是将自己登录的用户，添加到vboxsf组 : $ sudo usermod -aG vboxsf $(whoami)
重启虚拟机
###### 3.3关机和重启命令
重启命令 : 
$ sudo reboot 
$ sudo shutdown -r now 立刻重启
$ sudo shutdown -r 10 过10分钟自动重启
$ sudo shutdown -r 20:35 在时间为20:35时候重启
如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启 
关机命令 : 
$ sudo halt   立刻关机（一般加-p 关闭电源）
$ sudo poweroff 立刻关机 
$ sudo shutdown -h now 立刻关机
$ sudo shutdown -h 10 10分钟后自动关机 
如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消关机
###### 3.4U盘支持exfat
$ sudo apt-get install exfat-fuse exfat-utils


##### 4.为Dock启用“最小化点击”

`$ gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'  #更改立即生效`

##### 5.设置Dock(相当于windows的状态栏)：

​	设置-Dock,可设置Dock自动隐藏，图标大小，居中/左/右

##### 6.启用“夜灯”以获得更好的睡眠:

​	设置>设备>显示，选中“夜灯”

##### 7.安装gnome-tweak-tool

功能:更改GTK主题,将窗口按钮移动到左侧,调整鼠标/触控板行为,在顶部栏中启用“电池百分比”,更改系统字体,管理GNOME扩展。
`$ sudo apt install gnome-tweak-tool`

###### 7.1gnome-shell手动安装和卸载：

安装:https://extensions.gnome.org/ -- 下载相应的插件并解压到 ~/.local/share/gnome-shell/extensions 目录
卸载：删除~/.local/.../extensions下的对应目录 -- alt+f2 , r (重新启动gnome shell即可)

###### 7.2gnome-shell通过浏览器或软件中心安装和卸载

安装1:Dash to Panel(和win10一样的任务栏):软件中心搜索dash to panel安装,tweak(优化)--扩展(Extensions)--Dash to Panel；
安装2:安装firefox/chrome插件gnome shell integration -- 打开插件或直接访问https://extensions.gnome.org/ -- 对应的插件off 改为 on 即可

卸载:`$ sudo apt install chrome-gnome-shell  #chrome与浏览器无关，然后通过插件即可安装、配置和卸载`

###### 7.3Gnome-shell扩展推荐：

​    Dash to Panel – 将顶部吧和启动器结合到一个面板中
​    像素节省 – 减小最大化窗口标题的大小
​    弧形菜单 – 将传统的应用程序菜单添加到桌面
​    Gsconnect – 无线连接Android到Ubuntu桌面
​    截图工具 – 采取屏幕片段并上传到云端

##### 8.终端复用 : $ sudo apt-get install tmux
使用它最直观的好处就是，通过一个终端登录远程主机并运行tmux后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机
##### 9.安装截图软件flameshot , shutter

`$ sudo apt install flameshot -y  #启动后默认双击截图,ESC退出截图 ,设置快捷键参考:设置-设备-键盘`
$ sudo apt-get install shutter  #安装shutter

##### 10.GIF录屏Peek

```
$ sudo add-apt-repository ppa:peek-developers/stable  #添加一下源：(回车)
$ sudo apt update  #更新源
$ sudo apt install peek -y  #安装软件 ，设置快捷键：`Ctrl+Alt+G`
```

##### 11.安装Gimp,深度图片浏览器

```
$ sudo apt install deepin-image-viewer -y  #安装深度图片浏览器 
$ sudo add-apt-repository ppa:otto-kesselgulasch/gimp  #添加Gimp PPA ,按Enter键继续
$ sudo apt-get update
$ sudo apt-get install gimp -y
PS1:安装相关插件：sudo apt-get install gimp-plugin-registry gimp-data-extras
  插件描述：
  gimp-data-extras 刷子/调色板/渐变色的GIMP插件集
  gimp-gmic 用于《GREYC魔术图像转换软件》的GIMP插件
  gimp-gutenprint GIMP的打印插件
  gimp-plugin-registry GIMP的可选扩展库
  gvfs-backends 用户空间虚拟文件系统-后端
  xcftools 命令行工具，用于XCF文件的额外数据
  gimp-gap gif动态图制作插件
  mathmap 制作德罗斯特效应插件
```

##### 12.安装网易云音乐

```
$ wget http://d1.music.126.net/dmusic/netease-cloud-music_1.2.1_amd64_ubuntu.deb
$ sudo dpkg -i netease-cloud-music_1.2.1_amd64_ubuntu_20190428.deb 
```

##### 13.安装视频播放器smplyaer,VLC,mpv

VLC:`$ sudo snap install vlc`
mpv:`$ sudo apt install mpv -y` 

smplayer:

```
$ sudo add-apt-repository ppa:rvm/smplayer 
$ sudo apt-get update 
$ sudo apt-get install smplayer smtube smplayer-themes smplayer-skins -y
```
安装解码器
$ sudo apt-get install ubuntu-restricted-extras 
安装FFmpeg：
```
$ sudo add-apt-repository ppa:djcj/hybrid
$ sudo apt-get update
$ sudo apt-get install ffmpeg 
```

##### 14.安装Htop监控 

htop 是一个 Linux 下的交互式的进程浏览器，可以用来替换Linux下的top命令:`$ sudo apt install htop -y`

##### 15.安装搜狗输入法

```
$ wget http://cdn2.ime.sogou.com/dl/index/1571302197/sogoupinyin_2.3.1.0112_amd64.deb?st=ijd4ysUeqaTIij3P5N41Fg&e=1584768275&fn=sogoupinyin_2.3.1.0112_amd64.deb
$ sudo dpkg -i sogoupinyin_2.3.1.0112_amd64.deb
```

##### 16.安裝vim,net-tools,curl,filezilla,Proxy SwitchyOmega

```
$ sudo apt install net-tools -y  #网络管理工具,ifconfig等
$ sudo apt install curl -y #curl,wget单线程,wget 18.04系统默认安装
$ sudo apt install -y vim
$ sudo apt install filezilla -y  #FTP软件
Proxy SwitchyOmega:
  Chrome浏览器:https://github.com/FelisCatus/SwitchyOmega/releases,下载 SwitchyOmega_Chromium.crx
  火狐浏览器:下载地址:https://addons.mozilla.org/en-US/firefox/addon/switchyomega/
  使用方法:第一部分有1步,注意要首先在本地1080有代理:(proxy servers)socks5,127.0.0.1,1080
  第二部分:auto switch页面：profile(proxy,Direct)--Rule List Format(AutoProxy)--
  Rule List URL(https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt)
```

##### 17.安装远程连接和桌面openssh-server,rdesktop,teamview

```
$ sudo apt install openssh-server -y
$ sudo apt install rdesktop -y
uget : https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
$ sudo dpkg -i teamviewer_15.3.2682_amd64.deb
ps1: $ /etc/init.d/ssh restart  #启动ssh-server
ps2: $ netstat -tlp  #确认ssh-server是否正常工作,出现tcp6 0 0 :ssh :* LISTEN -表示ssh-server已经在运行
ps3: $ ssh -l name 113.112.23.124  #ssh登录示例,假设服务器的IP地址是113.112.23.124，登录的用户名是name
```

##### 18.安装google chrome  

```
$ sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
$ sudo apt update
$ sudo apt install google-chrome-stable
```

##### 19.安装aria2,uGet,百度网盘

aria2多线程下载工具,uGet+aria2替代迅雷的神器

`$ sudo apt install -y aria2` 
配置aria2:

```
$ sudo mkdir /etc/aria2    #新建文件夹 
$ sudo touch /etc/aria2/aria2.session    #新建session文件
$ sudo chmod 777 /etc/aria2/aria2.session    #设置aria2.session可写 
$ sudo vi /etc/aria2/aria2.conf    #创建配置文件
```

aria2.conf文件配置：

```
dir=/home/david/下载
disable-ipv6=true
#打开rpc的目的是为了给web管理端用
enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true
#rpc-listen-port=6800
#断点续传
continue=true
input-file=/etc/aria2/aria2.session
save-session=/etc/aria2/aria2.session
#最大同时下载任务数
max-concurrent-downloads=20
save-session-interval=120
#Http/FTP 相关
connect-timeout=120
#lowest-speed-limit=10K
#同服务器连接数
max-connection-per-server=10
#max-file-not-found=2
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
check-certificate=false
#http-no-cache=true
```

```
启动aria2 : $ sudo aria2c --conf-path=/etc/aria2/aria2.conf  #ctrl+c可随时停止命令
启动aria2 : $ sudo aria2c --conf-path=/etc/aria2/aria2.conf -D  #后台运行,ctrl+c可随时停止命令

开始下载: $ aria2c http://www.mirror.tw/pub/ubuntu/releases/jaunty/ubuntu-9.04-desktop-i386.iso
分段下载(-s n 可加速文件下载速度): $ aria2c -s 2 http://.../ubuntu-9.04-desktop-i386.iso
断点续传(-c): $ aria2c -c http://.../ubuntu-9.04-desktop-i386.iso
bt下载: $ aria2c -o gutsy.torrent http://.../gutsy-desktop-i386.iso.torrent
Download from 2 sources: $ aria2c http://a/f.iso ftp://b/f.iso
Download using 2 connections per host: $ aria2c -x2 http://a/f.iso
BitTorrent: $ aria2c http://example.org/mylinux.torrent
BitTorrent Magnet URI: $ aria2c 'magnet:?xt=urn:btih:248D0A1CD08284299DE78D5C1ED359BB46717D8C'
Metalink: $ aria2c http://example.org/mylinux.metalink
Download URIs found in text file: $ aria2c -i uris.txt
```

安装配置uget:

```
$ sudo add-apt-repository ppa:plushuang-tw/uget-stable
$ sudo apt update
$ sudo apt install uget aria2  #Recommended Method (aria2 Plugin Included)
配置uget:uget--编辑--插件--aria2+curl
```

```
安装百度网盘: $ sudo aria2c --conf-path=/etc/aria2/aria2.conf -D #启动
$ aria2c -c -s 3 http://wppkg.baidupcs.com/issue/netdisk/LinuxGuanjia/3.0.1/baidunetdisk_linux_3.0.1.2.deb
```

##### 20.安裝wps和缺失字体

```
$ wget https://wdl1.cache.wps.cn/wps/download/ep/Linux2019/9126/wps-office_11.1.0.9126_amd64.deb
$ sudo dpkg -i wps*.deb
$ sudo dpkg -i wps-office-fonts*.deb
$ sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office  #安装缺失字体
$ sudo mkfontscale  #生成字体索引 
$ sudo mkfontdir  #生成字体索引 
$ sudo fc-cache  #更新字体缓存
```

##### 21.安装markdown写作工具typora

```
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
$ sudo add-apt-repository 'deb https://typora.io/linux ./'
$ sudo apt-get update
$ sudo apt-get install typora
```

##### 22.安装pdf阅读工具okular

`$ sudo apt-get install okular #可阅读,可注释pdf文件`

23.安装电脑操作手机软件scrcpy

$ `snap` install scrcpy



#### 二.开发

##### 24.Git版本控制 
24.1安装git
`$ sudo apt install git -y` 
24.2 配置本机git的user.name和user.email
$ git config --global user.name "david"
$ git config --global user.email "shareinfo2015@163.com"
$ git config –list  #查看是否设置成功
24.3 本地git账号关联Github设置
$ ssh-keygen -t rsa -C "shareinfo2015@163.com" #一路下一步，完成后home目录下多出.ssh目录，id_rsa和id_rsa.pub,id_rsa是私钥，id_rsa.pub是公钥	
$ ssh-add ~/.ssh/id_rsa
进入github , Settings->SSH and GPG keys->New SSH key,拷贝id_rsa.pub文件的内容到key,点击 Add SSH key按钮添加,这样git push的时候就不需要每次输入用户名和密码了
##### 25.安装postman

软件中心安装或`uget: https://dl.pstmn.io/download/latest/linux64` # 解压到自己的目录，双击postman就可以使用了

##### 26.安装mysql8

```
uget: https://dev.mysql.com/get/mysql-apt-config_0.8.15-1_all.deb
$ sudo dpkg -i mysql-apt-config_0.8.15-1_all.deb  #弹出的界面直接选择OK
$ sudo apt-get update
$ sudo apt-get install mysql-server -y  #安装mysql服务
查看状态：sudo service mysql status 
启动服务：sudo service mysql start 
停止服务：sudo service mysql stop
$ sudo apt install mysql-client  #安装客户端
$ sudo apt install libmysqlclient-dev  #安装依赖
$ sudo netstat -tap | grep mysql  #检查状态
$ mysql -V  #查看mysql版本
$ sudo cat /etc/mysql/debian.cnf  #查看MySQL默认账号和密码
查看用户已经修改重置root密码:
mysql> select user, plugin from mysql.user; 
mysql> update mysql.user set authentication_string=PASSWORD(‘123456’), plugin=’mysql_native_password’ where user=’root’; 
mysql> flush privileges;
mysql> exit
配置远程访问mysql:
修改配置文件注释掉bind-address = 127.0.0.1 
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
$ mysql -uroot -p
mysql> grant all on . to root@’%’ identified by ‘123456’ with grant option;
	Query OK, 0 rows affected, 1 warning (0.00 sec)
mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)
mysql> exit 
    sudo /etc/init.d/mysql restart
1.彻底删除MySQL:此教程是你想安装MySQL 8.0或者重装MySQL 5.7的前提条件，如果你没有这两个需求，此教程可以忽略！
删除 mysql:
$ sudo apt-get autoremove --purge mysql-server-*
$ sudo apt-get remove mysql-server
$ sudo apt-get autoremove mysql-server
$ sudo apt-get remove mysql-common
2.清理残留数据:dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
安装mysql可视化，Linux上比较好的一款mysql-workbench：
$ sudo apt-get install mysql-workbench
出现依赖包无法下载错误时
$ sudo apt-get update --fix-missing
$ sudo apt-get install mysql-workbench
```

27.安装pycharm和webstorm
  http://www.jetbrains.com/
  注意激活 hosts文件的修改操作 0.0.0.0 account.jetbrains.com
  Windows系统hosts文件路径为：c:\windows\system32\drivers\etc
  Mac和Ubantu(Linux)系统hosts文件路径为：/etc
  激活码地址：http://idea.lanyus.com/

28.安装g++ gcc 开发必备编译库
$ sudo apt-get install build-essential  #Ubuntu默认不安装g++

29.安装sublime3
https://www.sublimetext.com/3

30.安装vscode
https://code.visualstudio.com/Download
sudo dpkg -i 对应的安装包名
如果出现依赖问题，执行：sudo apt-get install -f 
再次执行：sudo dpkg -i 对应的安装包名

31.安装nginx：
$ sudo apt-get install nginx
安装好的位置：
  /usr/sbin/nginx：主程序
  /etc/nginx：存放配置文件
  /usr/share/nginx：存放静态文件
  /var/log/nginx：存放日志
  service nginx start  #启动nginx
  service nginx reload  #重新加载nginx
  在浏览器输入你的ip地址，如果出现Wellcome to nginx 代表配置成功

32.shadowsocks：这里只用一种简单的ss客户端(注意如果要出 sslocal 的命令不能使用pip安装)
  sudo apt install shadowsocks
  然后写一个配置文件:gedit ~/Desktop/ss_config
  这里就直接在桌面上建一个文件,在打开的文本窗口中复制:
	{  
	    "server":"代理服务器ip",  
	    "server_port":端口,
	    "local_port":本地代理端口一般为1080,  
	    "password":"密码",  
	    "timeout":超时设置,  
	    "method":"加密方式"  

	}
  比如:
	{  
	    "server":"123.12.23.123",  
	    "server_port":8888,
	    "local_port":1080,  
	    "password":"1233211234567",  
	    "timeout":500,  
	    "method":"aes-256-cfb"  
	}
  然后保存后退出.运行下面的代码就可以了:sslocal -c ~/Desktop/Ashell/ss_config &
  如果需要全局代理,polipo!
33.polipo(全局代理)：前提:本地的1080端口已有代理.如ss...
  安装polipo: sudo apt-get install polipo ；sudo vi /etc/polipo/config
  在文件最后添加这几行:
    socksParentProxy = "localhost:1080"
    socksProxyType = socks5
    logLevel=4
  然后保存后退出,重启polipo (理论上应该用restart,但是好像重启的脚本有点问题,并不会刷新config,所以咱们stop再start)
    sudo service polipo stop
    sudo service polipo start
  重启后会在本地的 8123 端口开启全局代理,下面着行代码进行测试:
    sudo apt install curl
    http_proxy=http://localhost:8123 curl ip.gs
  如果得到的ip是代理服务器的,证明代理成功,下面配置全局代理:
    1>如果只想在当前终端中代理,可以使用
    export http_proxy=http://localhost:8123
    export https_proxy=https://localhost:8123
    2>真-全局代理：sudo vi ~/.bashrc
    在文件最后添加:
    export http_proxy=http://localhost:8123
    export https_proxy=https://localhost:8123
    然后使它生效:source ~/.bashrc

34.ubuntu与windows宿主机之间互相传送文件(使用ssh,而非虚拟机共享)
  >适用于小型文件:sudo apt install lrzsz
  然后在宿主机使用类似xshell的软件通过ssh连接到ubuntu虚拟机后,在宿主机中操作:
  sz 文件   #这个可以把虚拟机中的文件传输到本地
  rz 文件   #这个可以上传文件到虚拟机中

35.

36.

37.

38.

39.

40.

