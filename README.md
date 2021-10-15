# HP ProDesk 400 G2 微型台式电脑 OpenCore 0.7.4 EFI

![电脑图片](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/PC.png)

## 随便说点

在20年10月份那会机缘巧合之下获得了一台MacBook Pro，凑巧在B站首页上刷到了机汤哥的视频，是一个[面向萌新的从零开始的黑苹果攒机视频](https://www.bilibili.com/video/BV1ck4y117Cj)，说的就是这台惠普小机机。当时我就心动了，主要是便宜和傻瓜化，顺便还能统一工作环境，犹豫了几天就开始买配件跟着视频组装了。

我当时买的时候价格比起视频里所介绍的小涨了一波，但也是可以接受。当时入手主机+天线+电源是330元，i5 6600T是440元，两根杂牌8G内存是240元，500G机械硬盘69元，240G影驰垃圾固态150元（后来频繁掉盘，当时写入量还不到13T，不得已只能换了东芝RC500，285元），总共加起来才一千块出头，算是对想折腾黑苹果很友好很实在的搭配了。

后来Clover引导没法无痛升级Big Sur，正好机汤哥又出了一期视频说怎么给这台机器用上OpenCore引导，然后升级到新系统的，我就又跟着折腾了一波。后来又机缘巧合地看到了[July's](https://github.com/july929)大佬的博客（现在好像上不去了），和我几乎是同样的配置，但分享的引导文件好像更完美一些，于是我就又换了他分享的引导，但是他定制的USB在我的机子上不太行，所以我这时候开始第一次学习怎么折腾自己的配置。经过一番搜索与实践，最后定制了完美匹配我自己机子的USB驱动。

后来大佬把机子卖了，想升级OpenCore又不敢自己乱搞，有天在GitHub上看到有个外国老哥分享自己0.7.2的EFI，我就把他的引导和我手头的引导杂糅了一下，不确定的就翻OpenCore文档，最终就形成了现在的EFI。

总之整个折腾黑苹果的过程是非常有意思的。

## 配置

|  配置  |               参数                |
| :----: | :-------------------------------: |
|  CPU   | Intel Core i5-6600T @ 2.7GHz 4C4T |
|  显卡  |   Intel HD Graphics 530 1563MB    |
|  内存  |   杂牌DDR4 2133MHz 16GB 双通道    |
|  硬盘  | 东芝 RC500 500GB + 日立HDD 500GB  |
|  网卡  |     RTL8111H/BCM943224PCIEBT2     |
|  声卡  |          Realtek ALC221           |
| SMBIOS |          Mac mini (2018)          |
|  BIOS  |             N23 Ver.A             |
|  引导  |          OpenCore 0.7.4           |

系统：macOS Big Sur 11.6

![系统](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/桌面.png)

## 食用方法

可参考[国光大佬的macOS安装教程](https://apple.sqlsec.com/5-实战演示/5-5.html)，也可按[OpenCore用户指南上的安装教程](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)操作。以在Windows 10 x64系统下为例，进行如下步骤操作：

1. 给用于启动的硬盘预留出EFI引导分区（至少200MB），同时预留一部分空闲分区用于macOS
2. 从[OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.7.4)中下载`OpenCore-0.7.4-RELEASE.zip`，解压到本地
3. 进入目录`OpenCore-0.7.4-RELEASE/Utilities/macrecovery/`，在该目录中运行cmd
4. 输入`python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download`（需要python3环境），等待下载完成后在该目录下得到`BaseSystem.dmg`和`BaseSystem.chunklist`两个文件
5. 下载我的`EFI`文件，若核显不是HD530，则需要到[Intel核显platform ID整理](https://blog.daliansky.net/Intel-core-display-platformID-finishing.html)中找到你核显对应的`platform-id`，并替换`EFI/OC/config.plist`文件中的`Root/DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0)/device-id`项；并将`Root/DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0)/AAPL,ig-platform-id`项修改为`platform-id`的反转字节形式（如`platform-id`为`3EA50009`，则`AAPL,ig-platform-id`项修改为`0900A53E`）
6. 准备一个至少4GB的U盘，最好是USB3.0的，格式化成FAT32文件系统，将第4步中准备好的`EFI`文件夹拷贝到U盘根目录和硬盘的引导分区
7. 在U盘根目录下创建`com.apple.recovery.boot`文件夹，并拷贝`BaseSystem.dmg`和`BaseSystem.chunklist`到该文件夹中；在`BaseSystem.chunklist`文件夹中创建`.contentDetails`文件，文件内容为`macOS Recovery`
8. 将U盘插入到主机上，选择U盘启动，在看到引导界面时按下空格，选择`macOS Recovery`启动项
9. 点击`磁盘工具`，将第1步中的空闲分区格式化成APFS格式，然后退出磁盘工具
10. 点击`重新安装macOS`，并将macOS安装到第9步创建的分区中，等待安装过程
11. 结束安装后，从硬盘启动即可进入到macOS，注意不要登录Apple ID，使用工具为你的黑苹果注入三码后再登录

## 已实现

* CPU睿频变频正常
* 核显H265硬解正常
* USB接口定制，速率正常
* 扬声器正常
* 有线/无线网卡正常，蓝牙正常，隔空投送正常，屏幕镜像正常
* DP接口4K60Hz输出正常，音频输出正常
* 3.5mm接口声音输出正常
* 主动DP转HDMI转换器工作正常

## 未实现

* 麦克风无法工作

## 未详细测试

* 睡眠问题
* 使用被动DP转HDMI转换器在1080P分辨率下正常，在2K分辨率及以上分辨率下可能会出现闪屏问题（在我的一台显示器上会闪，另一台则不会），也有可能是我的转换器本身有问题
* 2.4G Wi-Fi和蓝牙同时使用存在干扰，据说换其他的免驱网卡可解决

## BIOS设置

* 关闭Secure Boot
* 关闭Fast Boot
* 关闭VT-d
* 显存分配至少为64MB

## 截图

![OpenCore界面](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/OpenCore.png)

![H265硬解](https://github.com/978025302/Hackintosh-HP-ProDesk-400G2-Mini-EFI/raw/master/img/H265硬解.png)

## 参考

* [july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI](https://github.com/july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI)
* [toshinori8/HP-ProDesk-400-G2-DM](https://github.com/toshinori8/HP-ProDesk-400-G2-DM)
* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [OpenCore 简体中文参考手册](https://oc.skk.moe)
* [哔哩哔哩 机汤TV](https://space.bilibili.com/485711932)
* [哔哩哔哩 国光_](https://space.bilibili.com/112842166)
* [大头蔡Cass](https://space.bilibili.com/16323318)

