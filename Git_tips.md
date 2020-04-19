##### 1.Git安装

下载 [git](https://git-scm.com/downloads) --右键安装包"以管理员身份运行"--安装位置自定义外其他默认即可

##### 2.使用Github

###### 2.1创建本地ssh key

$ ssh-keygen -t rsa -C "shareinfo2015@163.com" #邮箱为github注册邮箱，一路回车

###### 2.2 本地key关联github

复制 C:\Users\david\.ssh\id_rsa.pub 的  [key](ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDXuHLQEAxdgqquqH0zD/Mb2AACHVVWOI67DHZ9+BJtMmfZqBWGEbHJk0CFfDcnRMPZbIFMn/4ghQf3D1EOETF7c2M3jcpzlZZw+lqROs8ZQssnypEhJpDwF/osR4B7ot5VH0IgATS0dPWXauc5dtpIlbJvrTinHD3R1ipkpvGb2wWL+VXS5sBxcVsUtbuTGvu+ttYhTOj/3eKPY4RQ0SbW1c6NH8MITUYl2Qmj1enpn2GsnX/HXVX923tnbpfqizzqUJ+36MrYqVgvhf6E8upx9zVvuKbdtAzi9FEnoyuRsmyJvBak7ly8O75H7HZbdJFlJUVzFWOqf82wwt8xLjCXDN2auizHOU+ZheId4vUuCSw/9zWbMhQHhZhHvW0SC1kQ0b06njOXJW4TOeAhSA+BleVOqfg8/+C2PdLLI34jgzspsZIETfMcUW0JHvOmbLvcII71fqTS5f8fIQXRRcwz/EHzgbhMy/tgSuCkv8wF4CjmWECnFIru8XMcfd7D6s0= shareinfo2015@163.com) -- (GitHub)Account Settings--SSH and GPG keys--SSH keys--New SSH key--Title随意命名,Key为第2步生成

###### 2.3测试是否成功

```
$ ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Hi snowwest! You've successfully authenticated, but GitHub does not provide shell access.
```

###### 2.4设置你的用户名和电子邮件,github每次commit都会记录他们

```
$ git config --global  user.name "snowwest"
$ git config --global  user.email "shareinfo2015@163.com"
```

git config命令的--global参数，表示这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。(https://github.com/snowwest/Tips)

git config命令的--global参数，表示这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。(https://github.com/snowwest/Tips)

##### 3.克隆github项目到本地

```
方法1 : $ git clone git@github.com: snowwest/Tips.git  # 用户名/项目名.git
方法2：$ git clone https://github.com/snowwest/Tips.git
```

##### 4.本地新增，推送到github 

 $ git push -u origin master

##### 5.一次操作同时提交到gitee码云和github仓库

前提：gitee,github上拥有同一个项目，仓库地址地址示例：
https://github.com/snowwest/Tips.git
https://gitee.com/whsnow/Tips.git
方法：修改项目的.git/config文件,[remote "origin"],[branch "master"] 2个参数内添加地址。
未修改前config文件

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = git@github.com:snowwest/Tips.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

修改后config文件

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = git@github.com:snowwest/Tips.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = https://gitee.com/whsnow/Tips.git
[branch "master"]
	url = https://gitee.com/whsnow/Tips.git
	remote = origin
	merge = refs/heads/master
```

##### 6.git error

###### 6.1.本地push到git错误

```
$ git push -u origin master
fatal: Could not read from remote repository.alid format
```

方法：修复id_rsa.pub文件： `cd c:\Users\david\.ssh` , 'ssh-keygen -f id_rsa -y > id_rsa.pub'

