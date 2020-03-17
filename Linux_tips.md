### 一.Linux常用命令
1.查找文件命令：
find / -name name.txt 根据名称查找/目录下的name.txt文件
find . -name “*.xml” 递归查找所有的xml文件
find ./ -size 0 | xargs rm -f & 删除文件大小为零的文件
ls -l | grep '.jar' 查找当前目录中的所有JAR文件
grep 'test' d* 显示所有以d开头的文件中包含测试的行
grep 'test' aa bb cc 显示在AA，BB，CC文件中匹配测试的行
grep '[a-z]{5}' aa 显示所有包含每个字符串至少有5个连续小写字符的字符串的行。 

2.复制文件命令：
cp source dest 复制文件
cp -r sourceFolder targetFolder 递归复制整个文件夹

3.打包和压缩文件命令：
bunzip2 file.bz 解压一个叫做 'file.bz'的文件
bzip2 file 压缩一个叫做 'file' 的文件
gzip -9 file1 最大程度压缩
rar a file1.rar test_file 创建一个叫做 'file1.rar' 的包
rar a file1.rar file1 file2 dir1 同时压缩 'file1', 'file2' 以及目录 'dir1'
rar x file1.rar 解压rar包
unrar x file1.rar 解压rar包
tar -cvf archive.tar file1 创建一个非压缩的 tarball
tar -cvf archive.tar file1 file2 dir1 创建一个包含了 'file1', 'file2' 以及 'dir1'的档案文件
tar -tf archive.tar 显示一个包中的内容
tar -xvf archive.tar 释放一个包
tar -xvf archive.tar -C /tmp 将压缩包释放到 /tmp目录下
zip file1.zip file1 创建一个zip格式的压缩包
zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包
unzip file1.zip 解压一个zip格式压缩包 。

4.创建目录命令：mkdir newfolder
5.查看文件，包含隐藏文件：ls -al
6.查看当前工作目录：pwd
7.删除目录：
rmdir deleteEmptyFolder 删除空目录 
rm -rf deleteFile 递归删除目录中所有内容
8.移动文件：mv /temp/movefile /targetFolder
9.重命名命令：mv oldNameFile newNameFile
10.切换用户：su -username
11.修改文件权限：chmod xike file.java //file.java的权限-rwxrwxrwx，r表示读、w表示写、x表示可执行
12.启动tomcat
进入tomcat的bin目录，nohup ./startup.sh & tail -f ../logs/catalina.out ， tail -f ../logs/catalina.out 同时查看tomcat启动日志

二.1./etc/profile与/etc /enviroment的比较 
先将export LANG=zh_CN加入/etc/profile ,退出系统重新登录，登录提示显示英文。将/etc/profile 中的export LANG=zh_CN删除，将LNAG=zh_CN加入/etc/environment，退出系统重新登录，登录提示显示中文。
用户环 境建立的过程中总是先执行/etc/profile然后在读取/etc/environment。为什么会有如上所叙的不同呢？
应该是先 执行/etc/environment，后执行/etc/profile。
/etc/environment是设置整个系统的环境，而 /etc/profile是设置所有用户的环境，前者与登录用户无关，后者与登录用户有关。
系统应用程序的执行与用户环境可以是无关的， 但与系统环境是相关的，所以当你登录时，你看到的提示信息，象日期、时间信息的显示格式与系统环境的LANG是相关的，缺省LANG=en_US，如果系 统环境LANG=zh_CN，则提示信息是中文的，否则是英文的。
对于用户的SHELL初始化而言是先执行/etc/profile,再 读取文件/etc/environment.对整个系统而言是先执行/etc/environment。这样理解正确吗？
/etc/enviroment --> /etc/profile --> $HOME/.profile -->$HOME/.env (如果存在)
/etc/profile 是所有用户的环境变量
/etc/enviroment是系统的环境变量
登陆系统时shell读取的顺序应该是
/etc/profile ->/etc/enviroment -->$HOME/.profile -->$HOME/.env
原因应该是 jtw所说的用户环境和系统环境的区别了
如果同一个变量在用户环境(/etc/profile)和系统环境(/etc /environment)有不同的值那应该是以用户环境为准了。
（1）/etc/profile： 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行. 并从/etc/profile.d目录的配置文件中搜集shell的设置。
（2）/etc /bashrc: 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。
（3）~/.bash_profile: 每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户 的.bashrc文件。
（4）~/.bashrc: 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。
（5） ~/.bash_logout:当每次退出系统(退出bash shell)时,执行该文件. 另外,/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承 /etc/profile中的变量,他们是"父子"关系。
（6）~/.bash_profile 是交互式、login 方式进入 bash 运行的~/.bashrc 是交互式 non-login 方式进入 bash 运行的通常二者设置大致相同，所以通常前者会调用后者。
通 过比较最后发现，在ubuntu下配置java运行环境时，所做的设置代表的意义了。
$ sudo vim /etc/profile
在 文件最后添加
#set java environment
JAVA_HOME=/opt/jdk1.6.0_07
export JRE_HOME=/opt/jdk1.6.0_07/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
最后wq!
备注：在shell中执行 程序时，shell会提供一组环境变量。export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅及于该此登陆操作。