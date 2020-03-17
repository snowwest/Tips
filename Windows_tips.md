1.win10当前的壁纸位置：
C:\用户\用户名\AppData\Roaming\Microsoft\Windows\Themes\CachedFiles
win10锁屏壁纸位置：
C:\Users\用户名\AppData\Local\Packages\Microsoft.Windows.ContentDeliveryManager数字\LocalState\Assets

2.powershell输出乱码
(默认当前代码页936，ANSI/OEM-简体中文GBK)控制面板--区域--管理--更改系统区域设置--勾选"Beta版，使用...UTF-8..."，重启电脑

3.powershell显示目录下所有文件(包含隐藏文件) : PS >dir -force
4.powershell去除同一文件中的重复行
PS > Get-Content .\t.txt | sort -Unique //使用sort -unique数组去重，可省略
PS D:\> Get-Content .\filename | Select-Object -Unique //使用select-object -unique去重复行
或
$spath="c:\Users\softy\Desktop\EAO.TXT"
$dpath="c:\Users\softy\Desktop\EAO_NEW.TXT"
$ls=Get-Content -Path $spath -ReadCount 0|Select-Object -Unique
Set-Content -Path $dpath -Value ($lines -join "`r`n")

#github下载缓慢问题解决
140.82.113.4 github.com
199.232.69.194 github.global.ssl.fastly.net
52.217.39.76 amazonaws.com