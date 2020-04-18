##### 1.win10当前的壁纸位置：
C:\Users\david\AppData\Roaming\Microsoft\Windows\Themes\CachedFiles
win10锁屏壁纸位置：
C:\Users\david\AppData\Local\Packages\Microsoft.Windows.ContentDeliveryManager_cw5n1h2txyewy\LocalState\Assets

##### 2.用户免密码登录

win+r--netplwiz--选中账户，不勾选“要使用本计算机，用户必须输入用户名和密码”--确定

##### 3.PowerShell

###### 3.1PowerShell(管理员权限)一键卸载所有Win10预装App

`> Get-AppxPackage -User $env:USERNAME | Remove-AppxPackage`

###### 3.2逐个删除win10系统自带应用

```
> Get-AppxPackage *people* | Remove-AppxPackage  #人脉
> Get-AppxPackage *3dbuilder* | Remove-AppxPackage  #3D Builder
> Get-AppxPackage *windowsalarms* | Remove-AppxPackage  #闹钟与时钟
> Get-AppxPackage *windowscalculator* | Remove-AppxPackage  #行事历
> Get-AppxPackage *windowscommunicationsapps* | Remove-AppxPackage  #行事历与邮件
> Get-AppxPackage *windowscamera* | Remove-AppxPackage  #相机
> Get-AppxPackage *officehub* | Remove-AppxPackage  #获取 Office
> Get-AppxPackage *skypeapp* | Remove-AppxPackage  #获取 Skype
> Get-AppxPackage *getstarted* | Remove-AppxPackage  #获取开始
> Get-AppxPackage *zunemusic* | Remove-AppxPackage  #Groove 音乐
> Get-AppxPackage *windowsmaps* | Remove-AppxPackage  #地图
> Get-AppxPackage *solitairecollection* | Remove-AppxPackage  #Microsoft Solitaire Collection
> Get-AppxPackage *bingfinance* | Remove-AppxPackage  #财经
> Get-AppxPackage *zunevideo* | Remove-AppxPackage  #Movies & TV
> Get-AppxPackage *bingnews* | Remove-AppxPackage  #新闻
> Get-AppxPackage *onenote* | Remove-AppxPackage  #OneNote
> Get-AppxPackage *people* | Remove-AppxPackage  #联系人
> Get-AppxPackage *windowsphone* | Remove-AppxPackage  #手机小帮手
> Get-AppxPackage *photos* | Remove-AppxPackage  #相片
> Get-AppxPackage *windowsstore* | Remove-AppxPackage  #市场
> Get-AppxPackage *bingsports* | Remove-AppxPackage  #运动
> Get-AppxPackage *soundrecorder* | Remove-AppxPackage  #语音录音机
> Get-AppxPackage *bingweather* | Remove-AppxPackage  #天气
> Get-AppxPackage *xboxapp* | Remove-AppxPackage  #Xbox
```

###### 3.3.powershell输出乱码

(默认当前代码页936，ANSI/OEM-简体中文GBK)控制面板--区域--管理--更改系统区域设置--勾选"Beta版，使用...UTF-8..."，重启电脑

###### 3.4.powershell显示目录下所有文件 

 `> dir -force  #包含隐藏文件`

###### 3.5.powershell去除同一文件中的重复行

```
> Get-Content .\t.txt | sort -Unique //使用sort -unique数组去重，可省略
> Get-Content .\filename | Select-Object -Unique //使用select-object -unique去重复行
或
$spath="c:\Users\softy\Desktop\EAO.TXT"
$dpath="c:\Users\softy\Desktop\EAO_NEW.TXT"
$ls=Get-Content -Path $spath -ReadCount 0|Select-Object -Unique
Set-Content -Path $dpath -Value ($lines -join "`r`n")
```

##### 4.#github下载缓慢问题解决

140.82.113.4 github.com
199.232.69.194 github.global.ssl.fastly.net
52.217.39.76 amazonaws.com