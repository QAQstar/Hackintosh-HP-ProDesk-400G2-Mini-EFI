# HP ProDesk 400 G2 微型台式电脑 OpenCore 0.7.4 EFI

当前系统：macOS Big Sur 11.6

电脑配置

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

### 已实现

---

* CPU睿频变频正常
* 核显硬解正常
* USB接口定制
* 扬声器正常
* 有线/无线网卡正常
* 蓝牙正常

### 未实现

---

* 未注入三码
* 麦克风无法正常工作
* 睡眠问题

### BIOS设置

---

* 关闭Secure Boot
* 关闭Fast Boot
* 关闭VT-d
* 显存分配至少为64MB

### 参考

---

* [july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI](https://github.com/july929/Hackintosh-Hp-Prodesk-400G2-DM-EFI)
* [toshinori8/HP-ProDesk-400-G2-DM](https://github.com/toshinori8/HP-ProDesk-400-G2-DM)
* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [OpenCore 简体中文参考手册](https://oc.skk.moe)
* [哔哩哔哩 机汤TV](https://space.bilibili.com/485711932)
* [哔哩哔哩 国光_](https://space.bilibili.com/112842166)

