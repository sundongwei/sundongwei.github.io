---
title: 🎉 谷歌拼音安装
summary: Intall google pinyin
date: 2024-11-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'

authors:
  - Sun Dongway

tags:
  - Academic
  - Technology
  - AI
---

1.首先命令行安装汉语语言包

sudo apt-get install language-pack-zh-hans

执行该命令后，系统就会自动安装所需要的汉语语言包


![08c8414d047c4a2eacf174e6e0d06af7.png](https://s2.loli.net/2024/11/01/W2LNHZxudEQAskB.png)

2.然后命令行安装谷歌拼音输入法

sudo apt-get install fcitx-googlepinyin

执行该命令后，系统就会自动安装 fcitx 和 goolgepinyin 程序，也同时会安装一些配置 fcitx 的工具

![cab3b91cec4d49009169a100d8e60baf.png](https://s2.loli.net/2024/11/01/dHWCox9MwGEK6ip.png)
3.安装Fcitx输入法软件，输入如下命令。

sudo apt-get install fcitx


![63cb4c81e9ad4559a4d2f0b3c0effbc3.png](https://s2.loli.net/2024/11/01/DPf4Q1hZqXH8l9y.png)
4.配置fcitx。

im-config


![428d99d723814c8abc1a667766d90de1.png](https://s2.loli.net/2024/11/01/6WMUpidEmN7Qxf2.png)


![6da6e3309dc24575b5c237ee78e055e1.png](https://s2.loli.net/2024/11/01/qaz3xYfL8tc6Tuo.png)


![b08457ca45ef4f588fc281a58e7047b8.png](https://s2.loli.net/2024/11/01/jf2gYxHsUlbkOKZ.png)



![1ee45263ee434da2b905d91f3c83ecad.png](https://s2.loli.net/2024/11/01/kV7lwaUAP3yFn8I.png)

5.重启虚拟机,建议重启ubuntu系统

6.打开终端，配置googlepinyin输入法，在如图界面进行输入法配置。

fcitx-config-gtk3

![414d5d165514472dab9a2c9442ca165e.png](https://s2.loli.net/2024/11/01/aAkZ8MzjIb9BqcF.png)


![c20e83470c034f2e9d7136266c54e93f.png](https://s2.loli.net/2024/11/01/tQrZ6oP81f2ydXT.png)

7.通过快捷键，进行切换

