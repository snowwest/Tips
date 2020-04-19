#### 一.AndroidStudio低配电脑配置

如果您要在低于建议规格（建议规格:最低配置4G RAM,建议8G ; 最小2G磁盘空间，建议4G）的计算机上运行 Android Studio，则可以按如下方式自定义 Android Studio 以提升它在计算机上的性能：

##### 1.减小 Android Studio 可用的最大堆大小

减小 Android Studio 可用的最大堆大小：将 Android Studio 的最大堆大小减小至 512Mb(默认1280MB)。
File > Settings > Appearance & Behavior > System Settings > Memory Settings

如果您处理的是大项目，或者您的系统有大量 RAM 可用，您可以通过增大 Android Studio 进程（例如核心 IDE、Gradle 守护进程和 Kotlin 守护进程）的最大堆大小来提升性能。
Android Studio 会自动检查可采取的堆大小优化措施，并在检测到性能可以提升时通知您。
如果您使用的是 64 位系统并配有不少于 5 GB 的 RAM，您还可以手动调整项目的堆大小：help--Change Memory Settings--512M

##### 2.更新 Gradle 和 Android Plugin for Gradle

更新至最新版本的 Gradle 和 Android Plugin for Gradle，以确保您能利用最新的性能改进。

##### 3.启用节能模式

启用节能模式会关闭一系列消耗大量内存和电池的后台操作，包括错误突出显示和动态检查、自动弹出式代码完成和自动增量式后台编译。要开启节能模式:File > Power Save Mode。

##### 4.停用不必要的 lint 检查

File--Settings--Editor--Inspections--点击相应的复选框以选中或取消选中适合项目的 lint 检查--Apply

##### 5.在实际设备上调试

##### 6.仅将必要的 Google Play 服务作为依赖项包含在内

将 Google Play 服务作为依赖项包含在项目中会增加所需的内存量。仅添加必要的依赖项以提高内存利用率和性能。如需了解详情，请参阅将 Google Play 服务添加到您的项目中。

	To make the Google Play services APIs available to your app:
	Open the build.gradle file inside your application module directory.
	Note: Android Studio projects contain a top-level build.gradle file and a build.gradle file for each module. Be sure to edit the file for your application module. See Building Your Project with Gradle for more information about Gradle.
	Add a new build rule under dependencies for the latest version of play-services, using one of the APIs listed below.
	Ensure that your top-level build.gradle contains a reference to the google() repo or to maven { url "https://maven.google.com" }.
	Save the changes, and click Sync Project with Gradle Files in the toolbar.
##### 7.减少可用于 Gradle 的最大堆大小

Gradle 的默认最大堆大小为 1536 MB。您可以通过替换 gradle.properties 文件中的 org.gradle.jvmargs 属性来减小此值，如下所示：org.gradle.jvmargs=-Xmx1536m

##### 8.不要启用并行编译

启用并行编译可以并行编译独立模块,不启用并行编译：File--Settings--Build-确认Compile independent modules in parallel没有被勾选

##### 9.开启Gradle的离线模式

如果您的带宽有限，请开启离线模式，以防 Gradle 在您编译期间尝试下载缺失的依赖项。防止Gradle在构建期间尝试下载丢失的依赖项。离线模式打开时，Gradle 会在丢失任何依赖项时发布构建故障，而不会尝试下载它们。
开启离线模式:File--Settings--Build, Execution, Deployment--Gradle,
Global Gradle中选中 Offline work--Apply/OK 以使更改生效。

##### 10.配置离线编译依赖项

如果您想在没有网络连接的情况下编译项目，请按照以下步骤配置 Android Studio，以使用 Android Gradle 插件和 Google Maven 依赖项的离线版本。
Preview 126 MB 	a3f278a8162aa65f103bf51f8e664cf5179de0047c93111e2f86251d44d9dbc3(SHA-256 checksum)
https://dl.google.com/android/studio/plugins/android-gradle/preview/offline-android-gradle-plugin-preview.zip
Stable 	2724MB 	f632eed0d7c2e540665242d7e44156efff1e10ddf878cffa4de312958f2c0e2f(SHA-256 checksum)
https://dl.google.com/android/studio/maven-google-com/stable/offline-gmaven-stable.zip

###### 10.1下载并解压缩离线组件

下载离线组件后，将其内容解压缩到以下目录中，如果该目录尚不存在，您可能需要创建该目录：
在 Windows 上：%USER_HOME%/.android/manual-offline-m2/
在 macOS 和 Linux 上：~/.android/manual-offline-m2/

###### 10.2要更新离线组件

​    删除 manual-offline-m2/ 目录中的内容。
​    重新下载离线组件。
​    将所下载的 ZIP 文件的内容解压缩到 manual-offline-m2/ 目录中。

###### 10.3在 Gradle 项目中添加离线组件

要告知 Android 编译系统使用您已下载并解压缩的离线组件，您需要创建一个脚本（如下所述）。请注意，即使在更新离线组件之后，您也只需创建并保存此脚本一次。
10.3.1使用以下路径和文件名创建一个空文本文件：
     在 Windows 上：%USER_HOME%/.gradle/init.d/offline.gradle
     在 macOS 和 Linux 上：~/.gradle/init.d/offline.gradle
10.3.2打开该文本文件并添加以下脚本：

```
def reposDir = new File(System.properties['user.home'], ".android/manual-offline-m2")
    def repos = new ArrayList()
    reposDir.eachDir {repos.add(it) }
    repos.sort()

allprojects {
  buildscript {
    repositories {
      for (repo in repos) {
        maven {
          name = "injected_offline_${repo.name}"
          url = repo.toURI().toURL()
        }
      }
    }
  }
  repositories {
    for (repo in repos) {
      maven {
        name = "injected_offline_${repo.name}"
        url = repo.toURI().toURL()
      }
    }
  }
}
```

10.3.3保存该文本文件。

10.3.4（可选）如果您想要验证离线组件是否运行正常，请从项目的 build.gradle 文件中移除在线代码库（如下所示）。在确认项目不使用这些代码库也能正确编译之后，您可以将它们放回到 build.gradle 文件中。   

```
buildscript {
        repositories {
            // Hide these repositories to test your build against
            // the offline components. You can include them again after
            // you've confirmed that your project builds ‘offline’.
            // google()
            // jcenter()
        }
        ...
    }
    allprojects {
        repositories {
            // google()
            // jcenter()
        }
        ...
    }
```

 注意：此脚本会影响您在工作站上打开的所有 Gradle 项目。   

 注意：此脚本会影响您在工作站上打开的所有 Gradle 项目。   

##### 11.减少可用于 Gradle 的最大堆大小

Gradle 的默认最大堆大小为 1536 MB。您可以通过替换 gradle.properties 文件中的 org.gradle.jvmargs 属性来减小此值，如下所示：
```
# Make sure to gradually decrease this value and note
# changes in performance. Allocating too lttle memory may
# also decrease performance.
org.gradle.jvmargs = -Xmx1536m
```

#### 二.AndroidStudio减少体积

##### 1.android studio如何减小安卓源码包的体积大小

起因：源码包压缩1G解压2G,
处理：build-->clean project，file-->invalivdate caches restart
结果：源码包缩小到200多M

##### 2.如何减少APK安装包体积

增大原因：apk体积增大源于：新需求不断的提出,需要支持高分辨率的屏幕而加入了高分图片,依赖了更多的第三方库
分析：将apk解压后发现，体积占大头的分别是lib、res文件夹和dex文件。所以我们的降低apk体积的策略也应当从如何缩减so文件、资源图片、控制代码质量上来入手
解决方案：

###### 2.1开启Progruard



#### 三.AndroidStudio Gradle下载速度慢解决方法

##### 方法1：替换本地gradle

1.https://gradle.org/releases -- 下载complete版本的gradle 
2.完全关闭AS，包括正在下载gradle的进程也需要关闭
3.cd C:\Users\david\.gradle\wrapper\dists\gradle-5.6.4-all\ankdp27end7byghfw1q2sw75f--删除该目录下所有文件
4.重新打开AS，会自动解压并生成文件，sync

##### 方法2：使用国内aliyun镜像替代

1.(工程文件下的) build.gradle
2..在 buildscript 和 allprojects 的 repositories 中分别注释掉 jcenter()
3.在 buildscript 和 allprojects 的 repositories 分别添加：maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
4.再在 buildscript 的 repositories 添加：maven{url "https://jitpack.io"}
文件示例如下：

```
buildscript {    
    repositories {
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        maven{url "https://jitpack.io"}
        google()
        //jcenter()       
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.6.2'  
    }
}
allprojects {
    repositories {
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        google()
        //jcenter()        
    }
}
task clean(type: Delete) {
    delete rootProject.buildDir
}
```

#### 四.AndroidStudio、SDK目录、Gradle说明

AndroidStudio：Android应用的开发工具，类似如Eclipse
Gradle：一个构建工具；这类的构建工具有Ant，android eclipse开发用到的就是；Maven；

SDK目录，根据官方文档的描述
tools 必须：安卓开发相关的工具,如android.bat、emulator.exe、uiautomatorviewer.bat、monitor.bat等
platform-tools 必须：pc端和移动端进行交互工具,如adb,sqlite3,fastboot等
build-tools 必须：aapt打包工具(aapt编译资源文件得到二进制xml和R.java)，aidl工具(aidl工具将aidl文件--java interface) 
Platform 必须至少安装一个版本，提供开发时候要使用的API版本
system-images 建议安装
Android Support 建议安装
docs\samples 建议安装
sources 源代码,主要是View这些类的java文件，模拟器的API Demo的源文件
docs 接口文档，解释方法

compileSdkVersion是告诉gradle用哪个SDK版本来编译，和运行时要求的版本号没有关系；
高版本的buildTools可以构建低版本编译的Android程序，即buildToolsVersion的版本不低于compileSdkVersion的版本；compileSdkVersion 18  ，buildToolsVersion "22.0.1"这样也是OK的；

![image-20200316222807387](C:\Users\david\AppData\Roaming\Typora\typora-user-images\image-20200316222807387.png)

这种情况就是说compileSdkVersion>buildToolsVersion，这是不允许的；
targetSdkVersion：是 Android 系统提供前向兼容的主要手段，系统首先会查一下调用的 APK 的 targetSdkVersion 信息，如果小于targetSdkVersion 版本号，则按照老的行为，否则执行新的行为
android support repository主要是方便在gradle中使用android support libraries，因为Google并没有把这些库发布到maven center或者jcenter去，而是使用了Google自己的maven仓库。
support library 支持库,如support v4,support v7
google repository主要是给gradle使用的，方便添加Google Play Service的引用等，这样gradle就可以使用google的maven仓库中的库，而不需要去maven centee或者jcenter

**build.gradle**说明：

```
//说明module的类型，com.android.application为程序，com.android.library为库
apply plugin: 'com.android.application'

//要求buildToolsVersion >= compileSdkVersion>= targetSdkVersion，如bu29.03,co26,ta26
//implementation 'com.android.support:appcompat-v7:26.1.0'   //与compileSdkVersion一致
//implementation 'com.android.support:design:26.1.0'         //与compileSdkVersion一致
android {
    compileSdkVersion 29  //Gradle指定的用来编译的sdk版本，即API Level，和运行时无关
    buildToolsVersion "29.0.3" //Android构建工具的版本，位于SDK/build-tools，最好使用最新版本
    defaultConfig {//默认配置
        applicationId "com.example.myapplication2"  //应用程序的包名
        minSdkVersion 21  //支持的最低版本，低于该版本，应用无法在手机上运行
        targetSdkVersion 29  //<=compileSdkVersion，目标设备的兼容版本，一般设置为和compileSdkVersion相同或者略低，根据应用的实际情况而定
        versionCode 1  //版本号
        versionName "1.0"  //版本名
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        ndk {
            abiFilters "armeabi", "armeabi-v7a", "x86", "x86_64", "arm64-v8a"
        }
    }

    sourceSets {//目录指向配置
        main {
            manifest.srcFile 'AndroidManifest.xml'//指定AndroidManifest文件
            java.srcDirs = ['src']//指定source目录
            resources.srcDirs = ['src']//指定source目录
            aidl.srcDirs = ['src']//指定source目录
            renderscript.srcDirs = ['src']//指定source目录
            res.srcDirs = ['res']//指定资源目录
            assets.srcDirs = ['assets']//指定assets目录
            jniLibs.srcDirs = ['libs']//指定lib库目录
        }
        debug.setRoot('build-types/debug')//指定debug模式的路径
        release.setRoot('build-types/release')//指定release模式的路径
    }

    signingConfigs {//签名配置
        release {//发布版签名配置
            storeFile file("fk.keystore")//密钥文件路径
            storePassword "123"//密钥文件密码
            keyAlias "fk"//key别名
            keyPassword "123"//key密码
        }
        debug {//debug版签名配置
            storeFile file("fk.keystore")
            storePassword "123"
            keyAlias "fk"
            keyPassword "123"
        }
    }
    /**
     * 调用本地aar文件
     */
    repositories {
        //mavenLocal()
        //mavenCentral()
        flatDir {
            dirs 'libs'
        }
    }
    /*** 渠道打包*/
    productFlavors {
        xxx {//渠道名
            applicationId "修改的包名"
            versionName "2.2.9"
            versionCode 229
        }
        xxx1 {//渠道名
            applicationId "修改的包名"
            versionName "2.2.9"
            versionCode 229
        }
    }
    /**
     * Android默认配置,建议不写，直接用系统默认的配置
     */
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
            /**
             * 如果.so文件跟Eclipse一样放在了libs文件夹下就需要加上这一行代码
             */
            jniLibs.srcDirs = ['libs']
        }
        //测试所在的路径，这里假设是tests文件夹，没有可以不写这一行
        instrumentTest.setRoot('tests')
    }

    buildTypes {//build类型
        release {//发布
            minifyEnabled true//混淆开启
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-project.txt'
//指定混淆规则文件
            signingConfig signingConfigs.release//设置签名信息
        }
        debug {//调试
            signingConfig signingConfigs.release
        }
    }
    //配置IP
    buildTypes {
        release { //发布
            minifyEnabled false //是否混淆
            manifestPlaceholders = [SERVER_IP         : "192.168.25.232",
                                    SERVER_PORT       : "8021",
                                    SERVER_IP_PORT    : "http://192.168.25.232:8021",
                                    SERVER_IP_PORT_URL: "http://192.168.25.232:8021/%s",
                                    SERVER_PUSH_IP    : "192.168.25.221"]
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
        debug { //调试
            minifyEnabled false  //是否混淆
            manifestPlaceholders = [SERVER_IP         : "192.168.25.232",
                                    SERVER_PORT       : "8021",
                                    SERVER_IP_PORT    : "http://192.168.25.232:8021",
                                    SERVER_IP_PORT_URL: "http://192.168.25.232:8021/%s",
                                    SERVER_PUSH_IP    : "192.168.25.221"]
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.debug
        }
        sourceSets {
            main {
                jniLibs.srcDirs = ['libs']
            }
        }
    }

    packagingOptions { //代码混淆
        exclude 'META-INF/ASL2.0'
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/NOTICE'
        exclude 'META-INF/MANIFEST.MF'
    }
    lintOptions {
        abortOnError false //lint时候终止错误上报,防止编译的时候莫名的失败
    }
    repositories { //引入aar文件
        flatDir {
            dirs 'libs'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', exclude: ['android-support*.jar'], include: ['*.jar'])
    //编译lib目录下的.jar文件
    compile project(':Easylink')//编译附加的项目
    compile project(':ImageLibrary')
    compile 'com.nostra13.universalimageloader:universal-image-loader:1.9.3'//编译来自Jcenter的第三方开源库
    //引入aar文件
    compile(name: 'arr包的名字', ext: 'aar')
}
```

在Application中获取接口IP

```
String ServerUrl = this.getPackageManager().getApplicationInfo(this.getPackageName(), PackageManager.GET_META_DATA).metaData.getString("SERVER_IP_PORT");// 测试环境
String HOST = this.getPackageManager().getApplicationInfo(this.getPackageName(), PackageManager.GET_META_DATA).metaData.getString("SERVER_IP");
String API_URL = this.getPackageManager().getApplicationInfo(this.getPackageName(), PackageManager.GET_META_DATA).metaData.getString("SERVER_IP_PORT_URL");
```

远程依赖：gradle 同时支持maven，ivy，以下用maven 作为例子：

```
repositories {  
      //从中央库里面获取依赖
      mavenCentral()    
      //或者使用指定的本地maven 库
      maven{
        url "file://F:/githubrepo/releases"
       } 
      //或者使用指定的远程maven库
      maven{
        url "远程库地址"
      }
  }
  dependencies {    
          //应用格式: packageName:artifactId:version
    compile 'com.google.android:support-v4:r13'
  }
  android {

  }
```

JNI加载 .so 文件：在java目录下创建相应的目录，在此目录下创建相应的类，在类中创建相应方法，例如若c文件中的方法名为 `Java_a_b_c_JniUtils_getStringFormC` ，那么需要创建的目录为a.b.c。

需创建的类及方法：

```
public class JniUtils {
    static {
        System.loadLibrary("mzs");   //defaultConfig.ndk.moduleName
   }
    public static native String getStringFormC();
}
```

![image-20200317001043015](C:\Users\david\AppData\Roaming\Typora\typora-user-images\image-20200317001043015.png)

![image-20200317001102769](C:\Users\david\AppData\Roaming\Typora\typora-user-images\image-20200317001102769.png)

#### 五.AndroidStudio Error

##### 1.AndroidStudio编译时报错“Password verification failed”

原因：签名错误，C:\Users\david\.android创建了debug.keystore文件，并且和默认密码android不一致
(创建密钥的命令debug.keystore文件的命令为：
keytool -genkey -v -keystore debug.keystore -alias androiddebugkey -keyalg RSA -validity 10000)
解决：删除debug.keystore文件，重新运行,AS会自动生成一个新的debug.keystore文件，程序运行成功，问题解决

#### 六.搭建本地maven环境

原文链接：https://blog.csdn.net/qq_41153943/java/article/details/103009184

1.https://mirror.bit.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip
2.新建一个maven文件夹，解压文件，并在同级目录下建立一个Repository文件夹
D:\DevTools\maven\apache-maven-3.6.3
D:\DevTools\maven\Repository
3.设置环境变量：
新建： Maven_HOME   --   D:\DevTools\maven\apache-maven-3.6.3
修改path，添加%Maven_HOME%\bin
4.修改D:\DevTools\maven\apache-maven-3.6.3\conf\setting.xml文件，找到localRepository标签，增加：  <localRepository>D:\DevTools\maven\Repository</localRepository>
5.由于一般的jar包的下载都需要连接国外的网站下载，下载速度很慢，所以可以配置国内的镜像仓库，如阿里云镜像仓库，配置代码为：

```
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
  </mirrors>
```

6.cmd命令窗口输入 mvn -v 查看maven版本，如果能正确的弹出maven的版本号，则maven安装配置成功：Apache Maven 3.6.3 (ceced...)...
7.mvn help:system，则自动maven工具会到配置的服务器下载缺省的或者更新的各种配置文件和jar包到本地仓库中。下载完成后，去刚刚配置的Repository文件夹下会出现很多jar包的文件夹。
8.Eclipse中使用：
8.1windows--Preference--Maven--Installation，填写下面2个参数后finish(installation home :D:\DevTools\maven\apache-maven-3.6.3 ， installation name :apache-maven-3.6.3)。
8.2maven--user settings : user settings 选择为D:\DevTools\maven\apache-maven-3.6.3\conf\setting.xml文件，下面的仓库地址会自动改变为本地的仓库。
8.3切换到Eclipse仓库视图下，即可看到本地仓库下的jar包，有些时候如果没更新则右键rebuid index即可。
8.4Idea的使用类似，菜单栏file—>other settings---->setting for new projects---->Build,Execution,Deploment—>Build Tool—>maven,然后将刚刚的user setting 选择为刚刚conf下的setting.xml文件，仓库选择本地仓库即可。

6.cmd命令窗口输入 mvn -v 查看maven版本，如果能正确的弹出maven的版本号，则maven安装配置成功：Apache Maven 3.6.3 (ceced...)...
7.mvn help:system，则自动maven工具会到配置的服务器下载缺省的或者更新的各种配置文件和jar包到本地仓库中。下载完成后，去刚刚配置的Repository文件夹下会出现很多jar包的文件夹。
8.Eclipse中使用：
8.1windows--Preference--Maven--Installation，填写下面2个参数后finish(installation home :D:\DevTools\maven\apache-maven-3.6.3 ， installation name :apache-maven-3.6.3)。
8.2maven--user settings : user settings 选择为D:\DevTools\maven\apache-maven-3.6.3\conf\setting.xml文件，下面的仓库地址会自动改变为本地的仓库。
8.3切换到Eclipse仓库视图下，即可看到本地仓库下的jar包，有些时候如果没更新则右键rebuid index即可。
8.4Idea的使用类似，菜单栏file—>other settings---->setting for new projects---->Build,Execution,Deploment—>Build Tool—>maven,然后将刚刚的user setting 选择为刚刚conf下的setting.xml文件，仓库选择本地仓库即可。

