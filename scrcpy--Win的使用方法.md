## scrpy--Win的使用方法

scrcpy简介

> 注意：拼写是scrcpy，非Python爬虫框架Scrapy。

简单地来说，scrcpy就是通过adb调试的方式来将手机屏幕投到电脑上，并可以通过电脑控制您的Android设备。它可以通过USB连接，也可以通过Wifi连接（类似于隔空投屏），而且不需要任何root权限，不需要在手机里安装任何程序。scrcpy同时适用于GNU / Linux，Windows和macOS。

它的一些特性：
- 亮度（原生，仅显示设备屏幕）

- 性能（30~60fps）

- 质量（1920×1080或以上）

- 低延迟（35~70ms）

- 启动时间短（显示第一张图像约1秒）

- 非侵入性（设备上没有安装任何东西）

**使用scrcpy的要求**

1. Android设备至少需要API 21（Android 5.0以上版本）;
2. 确保在您的设备上启用了adb调试;
3. 在某些设备上，您还需要启用其他选项以使用键盘和鼠标控制它。

adb调试的开启一般是多次点击手机系统版本，如我用的是MIUI10，开启方法是 “设置”->“我的设备”->“全部参数”->点击7下MIUI版本，开启“开发者选项”。然后在 “设置”->“更多设置”->“开发者选项” 中同时开启 USB调试 和 USB调试(安全设置)。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMuemh5b25nLmNuL2ltYWdlcy8yMDE5LzA4LzE4LzIwMTkwODE4MTQyMDA2LnBuZw)

注意：USB调试(安全设置)必须开启，否则不可以使用电脑控制手机，即上述要求的第三条。

使用电脑连接手机
在Android手机中打开了USB调试后，我们即可在电脑中使用adb进行调试。

我使用的是Windows10系统，以下以Windows为例，MacOS或Linux请点击这里。

程序使用了Java语言，我们需要在电脑中搭建Java运行环境，参考：Windows10 配置 Java 开发环境

首先下载scrcpy，可去releases下载最新版本，目前最新版本为v1.10。

下载地址：https://github.com/Genymobile/scrcpy/releases

解压后的目录：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMuemh5b25nLmNuL2ltYWdlcy8yMDE5LzA4LzE4LzIwMTkwODE4MTQ1NTU3LnBuZw)

打开cmd定位到此目录（在地址栏中输入cmd回车），或者将该目录如D:\Github_Run\scrcpy-win64-v1.10加入到系统环境变量中，程序的使用都在cmd命令行中操作。

**使用USB进行连接**
此方式推荐使用，相对更加流畅。

1. 手机通过USB连接到PC上，首次连接会弹出是否信任该电脑，点击始终信任即可。

2. 运行adb usb查看是否连接成功

   > D:\Github_Run\scrcpy-win64-v1.10>adb usb   
   > restarting in USB mode

3. 运行scrcpy即可。

**使用无线连接**
  此连接方式更加方便快捷，若宽带速率高，使用效果更佳，使用方法也非常简单。

1. 确保PC和手机在同一Wifi中

2. 手机先通过USB与PC相连

3. 在PC上运行 adb tcpip 服务端口，如端口为5555

   > D:\Github_Run\scrcpy-win64-v1.10>adb tcpip 5555  
   > restarting in TCP mode port: 5555

4. 拔下你的设备，断开USB连接

5. PC上运行 adb connect 手机IP:服务端口（手机IP可通过手机的状态信息查看，或者登录路由器查看，一般以192.168开头）
   > D:\Github_Run\scrcpy-win64-v1.10>adb connect 192.168.0.4:5555                      
   > connected to 192.168.0.4:5555

6. 运行scrcpy，在cmd中输入scrcpy.exe

   > 这样弹出手机的屏幕，手机投屏成功！正如预期的那样，性能与USB不同，默认的scrcpy比特率是8Mbps，这对于Wi-Fi连接来说可能太多了。根据使用情况，降低比特率和分辨率可能是一个很好的折中方案。

> scrcpy --bit-rate 2M --max-size 800
>
> 简写：如下
>
> scrcpy -b2M -m800

若要切换回USB模式：adb usb

常用快捷键（重要）
描述	快捷键
切换全屏模式	Ctrl+f
点击手机电源	Ctrl+p
返回	Ctrl+b
返回到HOME	Ctrl+h
多任务	Ctrl+s
更多操作	长按鼠标左键
显示最佳窗口	Ctrl+g
调节音量	Ctrl+上下键
关闭设备屏幕（保持镜像）	Ctrl+o
将设备剪贴板复制到计算机	Ctrl+c
将计算机剪贴板粘贴到设备	Ctrl+v
Tips：查看已连接设备命令adb devices，显示device则表示已连接，显示offline则离线：

> D:\Github_Run\scrcpy-win64-v1.10>adb devices  
> List of devices attached   
> 192.168.0.4:5555        device

使用命令行选项在启动时镜像时可以关闭设备屏幕，这一点也挺实用：

> scrcpy --turn-screen-off  
> scrcpy -S