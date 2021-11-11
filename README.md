# HP ProDesk 400 G2 迷你型台式电脑 OpenCore 0.7.5 EFI

![电脑图片](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/PC.png)

## 随便说点

在20年10月份那会机缘巧合之下获得了一台MacBook Pro，凑巧在B站首页上刷到了机汤哥的视频，是一个[面向萌新的从零开始的黑苹果攒机视频](https://www.bilibili.com/video/BV1ck4y117Cj)，说的就是这台惠普小机机。当时我就心动了，主要是便宜和傻瓜化，顺便还能统一工作环境，犹豫了几天就开始买配件跟着视频组装了。

我当时买的时候价格比起视频里所介绍的小涨了一波，但也是可以接受。当时入手主机+天线+电源是330元，i5 6600T是440元，两根杂牌8G内存是240元，500G机械硬盘69元，240G影驰垃圾固态150元（后来频繁掉盘，当时写入量还不到13T，不得已只能换了东芝RC500，285元），总共加起来才一千块出头，算是对想折腾黑苹果很友好很实在的搭配了。

后来Clover引导没法无痛升级Big Sur，正好机汤哥又出了一期视频说怎么给这台机器用上OpenCore引导，然后升级到新系统的，我就又跟着折腾了一波。后来又机缘巧合地看到了[July's](https://github.com/july929)大佬的[博客](https://yangwenqing.com/archives/1550/ )，和我几乎是同样的配置，但分享的引导文件好像更完美一些，于是我就又换了他分享的引导，但是他定制的USB在我的机子上不太行，所以我这时候开始第一次学习怎么折腾自己的配置。经过一番搜索与实践，最后定制了完美匹配我自己机子的USB驱动。（附[定制USB驱动教程](https://www.bilibili.com/video/BV1m3411b7JP)）

后来大佬把机子卖了，想升级OpenCore又不敢自己乱搞，有天在GitHub上看到有个外国老哥分享自己0.7.2的EFI，我就把他的引导和我手头的引导杂糅了一下，不确定的就翻OpenCore文档，开始了自己慢慢琢磨配置文件之路。

总之整个折腾黑苹果的过程是非常有意思的。

## 注意事项

* 测试EFI不易，如有帮助请动动小手点个Star
* 请自行注入三码！！请自行注入三码！！请自行注入三码！！
* 不再更新macOS Big Sur下的OpenCore EFI引导文件，最后更新版本为OpenCore 0.7.4
* Release中0.7.4版本的`EFI_Big Sur.zip`为支持macOS Big Sur（11.x.x）系统的引导，`EFI_Monterey.zip`为支持macOS Monterey（12.0.1）系统的引导，按需选择；若想使用macOS California（10.15.x），请使用`EFI_Big Sur.zip`并自行测试功能
* macOS从Big Sur升级到Monterey时，需要替换EFI文件，替换为OpenCore 0.7.4版本的`EFI_Monterey.zip`或更高版本，同时清除NVRAM
* 替换0.7.4版本的EFI文件后若蓝牙无法使用，关机切断电源，过段时间后重新开机即可（仅针对BCM943224PCIEBT2，其他网卡自行测试）；该问题已在0.7.5版本中解决

## 配置

|   配置   |                             参数                             |
| :------: | :----------------------------------------------------------: |
|   设备   | [HP ProDesk 400 G2 迷你型台式电脑](https://support.hp.com/cn-zh/document/c04864998)  <br />（[HP ProDesk 400 G2 Desktop Mini PC](https://support.hp.com/us-en/document/c04843458)） |
| 当前系统 |                    macOS Monterey 12.0.1                     |
|   CPU    |              Intel Core i5-6600T @ 2.7GHz 4C4T               |
|   显卡   |                 Intel HD Graphics 530 1536MB                 |
|   内存   |               杂牌DDR4 2133MHz 8GB × 2 双通道                |
|   硬盘   | SSD: TOSHIBA RC500 500GB  <br />HDD: HGST HTS545050A7E380 500GB |
|   网卡   | Wired: Realtek RTL8111HSH-CG GbE  <br />Wireless: Broadcom BCM943224PCIEBT2 |
|   声卡   |                        Realtek ALC221                        |
|  SMBIOS  |                       Mac mini (2018)                        |
|   BIOS   |                        N23 Ver 02.53                         |
|   引导   |                        OpenCore 0.7.5                        |

![系统](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/桌面.png)

## 食用方法

可参考[国光大佬的macOS安装教程](https://apple.sqlsec.com/5-实战演示/5-5.html)，也可按[OpenCore用户指南上的安装教程](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)操作。以在Windows 10 x64系统下，OpenCore 0.7.4安装macOS Big Sur为例，进行如下步骤操作：

1. 给用于启动的硬盘预留出EFI引导分区（至少200MB），同时预留一部分空闲分区用于macOS

2. 从[OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.7.4)中下载`OpenCore-0.7.4-RELEASE.zip`，解压到本地

3. 进入目录`OpenCore-0.7.4-RELEASE/Utilities/macrecovery/`，在该目录中运行cmd，输入`python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download`（需要python3环境），等待下载完成后在该目录下得到`BaseSystem.dmg`和`BaseSystem.chunklist`两个文件

4. 下载Releases中0.7.4版本的`EFI_Big Sur.zip`文件，并解压到本地。若核显不是HD530，则需要到[Intel核显platform ID整理](https://blog.daliansky.net/Intel-core-display-platformID-finishing.html)中找到你核显对应的`platform-id`，并替换`EFI/OC/config.plist`文件中的`Root/DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0)/device-id`项；并将`Root/DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0)/AAPL,ig-platform-id`项修改为`platform-id`的反转字节形式（如`platform-id`为`3EA50009`，则`AAPL,ig-platform-id`项修改为`0900A53E`）

5. 准备一个至少4GB的U盘，最好是USB3.0的，格式化成FAT32文件系统，将第4步中准备好的`EFI`文件夹拷贝到U盘根目录和硬盘的引导分区

6. 在U盘根目录下创建`com.apple.recovery.boot`文件夹，并拷贝`BaseSystem.dmg`和`BaseSystem.chunklist`到该文件夹中；在`com.apple.recovery.boot`文件夹中创建`.contentDetails`文件，文件内容为`macOS Recovery`

7. 此时U盘中目录结构应如下：

    ```
    .
    +-- EFI
    |    +-- BOOT
    |    +-- OC
    +-- com.apple.recovery.boot
    |    +-- BaseSystem.dmg
    |    +-- BaseSystem.chunklist
    |    +-- .contentDetails
    ```

8. 将U盘插入到主机上，选择U盘启动，在看到引导界面时按下空格，选择`macOS Recovery`启动项

9. 点击`磁盘工具`，将第1步中的空闲分区格式化成APFS格式，然后退出磁盘工具

10. 点击`重新安装macOS`，并将macOS安装到第9步创建的分区中，等待安装过程

11. 结束安装后，从硬盘启动即可进入到macOS，注意不要登录Apple ID，还需要注入新的三码，可参考[OpenCore生成三码](https://heipg.cn/tutorial/macserial-and-iservice-opencore.html)和[国光的PlatformInfo配置说明](https://apple.sqlsec.com/4-OC配置/4-7.html)。完成macOS初始化设置后下载Releases中0.7.4版本的`OCC`文件（或使用[Hackintool](https://github.com/headkaze/Hackintool/releases)、[OCAuxiliaryTools](https://github.com/ic005k/QtOpenCoreConfig/releases)等app，建议使用OCAuxiliaryTools，个人认为比较好用），解压到本地后双击打开；在菜单栏中选中单击应用图标，点击`挂载分区`，输入密码后再点击`打开分区`，进入引导分区；双击打开`EFI/OC/config.plist`文件，在PlatformInfo设置中生成新的三码，并按下`Command+S`或点击左上角`文件-保存`即可。以OpenCore Configurator为例，过程可见下图：  
    ![挂载引导分区](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/挂载分区.png)  
    ![打开引导分区](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/打开分区.png)  
    ![打开config.plist文件](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/config文件.png)  
    ![注入新的三码](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/注入三码.png)  

12. 重启电脑，在OpenCore引导界面中按下空格，选择`Reset NVRAM`；自动重启后再次进入到macOS系统，即可登录Apple ID

## 已实现

* CPU睿频变频正常
* 核显H265硬解正常
* USB接口定制，速率正常
* 扬声器正常
* 有线/无线网卡正常，蓝牙正常，隔空投送正常，屏幕镜像正常
* DP接口4K60Hz输出正常，音频输出正常
* 3.5mm接口声音输出正常
* DP转HDMI主动转换器工作正常

## 未实现

* 麦克风无法工作，据说换成`VoodooHDA.kext`驱动就能正常使用麦克风和扬声器，但在Monterey系统下测试无效
* 长时间睡眠后唤醒异常

## 未详细测试

* 使用DP转HDMI被动转换器在1080P分辨率下正常，在2K分辨率及以上分辨率下可能会出现闪屏问题（在我的一台显示器上会闪，另一台则不会），也有可能是我的转换器本身有问题
* 2.4G Wi-Fi和蓝牙同时使用可能存在干扰，据说换其他的免驱网卡可解决
* 有线随航和无线随航，没有iPad设备，故无法测试

## BIOS设置

* 关闭Secure Boot
* 关闭Fast Boot
* 关闭VT-d，或在`config.plist`中将`Root/Kernel/Quirks/DisableIoMapper`项设为`True`
* 显存分配至少为64MB

## 截图

![OpenCore界面](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/OpenCore.png)

![GeekBench5跑分](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/GeekBench5.png)

![H265硬解](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/H265硬解.png)

## 参考

* [july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI](https://github.com/july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI)
* [toshinori8/HP-ProDesk-400-G2-DM](https://github.com/toshinori8/HP-ProDesk-400-G2-DM)
* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [OpenCore 简体中文参考手册](https://oc.skk.moe)
* [OpenCore 黑苹果引导配置说明](https://shuiyunxc.gitee.io/2020/03/10/instru/index/)
* [哔哩哔哩 机汤TV](https://space.bilibili.com/485711932)
* [哔哩哔哩 国光_](https://space.bilibili.com/112842166)
* [哔哩哔哩 大头蔡Cass](https://space.bilibili.com/16323318)

