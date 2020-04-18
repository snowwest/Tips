##### 1.Virtualbox手动安装系统增强VBoxGuestAdditions

```
$ sudo mkdir --p /media/cdrom
$ sudo mount -t auto /dev/cdrom /media/cdrom/
$ cd /media/cdrom/
$ sudo sh VBoxLinuxAdditions.run
```

重启虚拟机就可以了

##### 2.xfce4

安装xfce4 : $sudo apt-get install xfce4
卸载xfce桌面:

```
$ sudo apt-get remove xfce4  #卸载xfce4  
$ sudo apt-get remove xfce4* #卸载相关软件 
$ sudo apt-get autoremove #自动卸载不必要的软件 
$ sudo apt-get clean #系统清理  
如果安装的是xubuntu-desktop还需要卸载xubuntu : $sudo apt-get remove xubuntu*
```

##### 3.更新默认源为阿里云镜像源

```
lsb_release -a  #列出系统全部信息
lsb_release -c  #查看版本信息,Ubuntu 18.04 LTS 的代号是bionic 
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak #复制源文件备份
sudo vim /etc/apt/sources.list  #编辑源文件，设置aliyun镜像源取代默认的更新源
```
```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```
```
sudo apt-get update  #更新软件列表
sudo apt-get upgrade #对比新的软件列表，更新软件
```