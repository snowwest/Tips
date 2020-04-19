##### 1.SourceTree入门

###### 1.1免登陆安装（V3.3.8）

1.1.1下载[SourceTree](https://www.sourcetreeapp.com/),双击安装到弹出bitbucket登陆界面退出

1.1.2cd C:\Users\david\AppData\Local\Atlassian\SourceTree目录下创建accounts.json文件（"david"需要修改为其他username），内容如下

```
[
  {
    "$id": "1",
    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",
    "Authenticate": true,
    "HostInstance": {
      "$id": "2",
      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",
      "Host": {
        "$id": "3",
        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",
        "Id": "atlassian account"
      },
      "BaseUrl": "https://id.atlassian.com/"
    },
    "Credentials": {
      "$id": "4",
      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",
      "Username": "username@email.com"
    },
    "IsDefault": false
  }
]
```

1.1.3打开C:\Users\david\AppData\Local\Atlassian\SourceTree.exe_Url_nzymulh1rfz04sjiglmsbh4hnbaxch41\3.3.8.3848\user.config文件，增加

```
<setting name="AgreedToEULA" serializeAs="String">
	<value>True</value>
</setting>
<setting name="AgreedToEULAVersion" serializeAs="String">
	<value>20160201</value>
</setting>
```

1.1.4重新双击SourceTree.exe安装即可，Mercurial选择安装不不安装随意，完美跳过注册安装成功。

###### 1.2集成文件对比插件

以BeyondCampare为例 : 工具--选项--比较--“外部对比工具”，“合并工具”都选择Beyond Compare，按照提示选择BComp.exe的路径。

###### 1.3配置

1.3.1生成SSH

1.3.2远程仓库如github添加生成的密钥

1.3.3工具--选项--(SSH客户端配置)修改ssh客户端为OpenSSH,密钥位置为C:\Users\david\.ssh\id_rsa

###### 1.4SourceTree基本使用

1.4.1本地克隆：克隆/新建--拖放本地git项目即可

1.4.2

