微软官方帮助文件 [Windows Subsystem for Linux Installation Guide for Windows 10 http](s://docs.microsoft.com/en-us/windows/wsl/install-win10)

##### 一.安装WSL
###### 1.开启WSL
方法1：打开PowerShell(管理员)--`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`--重启
方法2：控制面板-卸载程序--启用或关闭Windows功能--勾选“虚拟机平台”&&“适用于Windows的Linux子系统”--
2.Windows 下查看 WSL 文件位置，文件位置在：E:\Ubuntu\rootfs 下
###### 2.安装Linux系统
方法1:(默认安装到系统盘，可以通过如下方法更改位置：设置--系统--存储--更改新内容的保存位置--新的应用将保存到)--Windowns商店--搜索Ubuntu--选择相应的版本安装
方法2：
2.1通过PowerShell下载：`Invoke-WebRequest -Uri ttps://aka.ms/wsl-ubuntu-1804 -OutFile Ubuntu.appx -UseBasicParsing`
(Tip：If the download is taking a long time, turn off the progress bar by setting `$ProgressPreference = 'SilentlyContinue'`)
or Curl下载：`curl.exe -L -o ubuntu-1804.appx https://aka.ms/wsl-ubuntu-1804`
2.2安装Linux套件：(安装的位置为存储中默认的位置)
`cd app_name.appx包所在目录`--`Add-AppxPackage .\app_name.appx`
2.3安装Linux套件(可指定安装位置)：创建目录--在目录中解压套件--执行套件中的.exe文件(如ubuntu.exe)
```
Rename-Item ./Ubuntu.appx ./Ubuntu.zip
Expand-Archive ./Ubuntu.zip ./Ubuntu
```
PS1：LxRunOffline i -n <安装名称> -d <安装路径> -f <安装文件>
<安装名称>可以自定义，<安装路径>为自定义安装路径，<安装文件>为解压的app_name.appx文件中的install.tar.gz的路径，回车后等待安装完成。
若安装不止一个WSL,使用 LxRunOffline sd -n <安装名称> 设置默认启动系统，然后在cmd中输入 wsl 启动系统。
若忘记安装名称，可通过LxRunOffline list命令查看。
PS2:wsl命令帮助
wsl -l 列出当前系统上已经安装的 Linux 子系统
wsl --list --verbose查看使用的wsl版本
wsl --set-default-version 2 设置默认为wsl2
wsl --set-version Debian 2设置某个发行版为wsl2
启动wsl: (PowerShell)wsl
关闭当前运行的WSL实例：wsl --shutdown
PS3:若手滑直接删除了ubuntu的目录：查看发行版--注销--重新解压安装：
```
wslconfig /l  #查看发行版 
wslconfig /u Ubuntu  #从列表中选择要卸载的发行版（例如Ubuntu）并键入命令
```
PS4:使用wslconfig命令进行管理
1.查看已安装的linux系统 : wslconfig /list
2.设置默认运行的linux系统 : wslconfig /setdefault <DistributionName>
3.卸载(注销)linux系统 : wslconfig /unregister <DistributionName>
PS5:`ubuntu config --default-user root #设置默认登陆用户，如root登陆,“ubuntu /?”可查看其更多使用方法`

3.更改源文件：进入Ubuntu系统--查看版本信息--备份原文件&&编辑源文件(删除所有内容，拷贝以下内容，以阿里云的源为例)--保存退出--更新升级
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
PS:apt-get帮助
apt-cache search package 搜索包
apt-cache show package 获取包的相关信息，如说明、大小、版本等
sudo apt-get install package 安装包
sudo apt-get install package - - reinstall 重新安装包
sudo apt-get -f install 修复安装"-f = ——fix-missing"
sudo apt-get remove package 删除包
sudo apt-get remove package - - purge 删除包，包括删除配置文件等
sudo apt-get update 更新源
sudo apt-get upgrade 更新已安装的包
sudo apt-get dist-upgrade 升级系统
sudo apt-get dselect-upgrade 使用 dselect 升级
apt-cache depends package 了解使用依赖
apt-cache rdepends package 是查看该包被哪些包依赖
sudo apt-get build-dep package 安装相关的编译环境
apt-get source package 下载该包的源代码
sudo apt-get clean && sudo apt-get autoclean 清理无用的包
sudo apt-get check 检查是否有损坏的依赖
4.开启root账户 
```
sudo passwd root
```
PS:
cat /etc/passwd #查看账户密码信息
id username #查看账户username的信息:uid,gid,groups
5.改变WSL显示语言(中文乱码时可用)
$ sudo update-locale LANG=en_US.UTF8
6.How do I access a port from WSL in Windows?
WSL shares the IP address of Windows, as it is running on Windows. As such you can access any ports on localhost e.g. if you had web content on port 1234 you could https://localhost:1234 into your Windows browser.
7.How can I back up my WSL distros?
The best way to backup your distros is available in Windows Version 1809 and later. You can export your entire distribution to a tarball using the wsl --export command. You can then import this distro back into WSL using the wsl --import command, allowing you to backup and save states of your WSL distributions.
Please note that traditional backup services that backup files in your Appdata folders (like Windows Backup) will not corrupt your Linux files.
