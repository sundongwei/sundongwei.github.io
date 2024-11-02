---
title: 🎉 Ubuntu net config
summary: Ubuntu net config
date: 2024-11-02

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

# ubuntu 22.04如何配置静态IP、网关、DNS

1、当前系统

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519094733165-902369774.png)

 ubuntu 22.04.

2、进入/etc/netplan/目录，列出该目录下的内容

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519094954609-224110774.png)

3、利用vim编辑器打开xxx.yaml文件，进行编辑：

vim 01-network-manager-all.yaml

内容如下：（修改网卡名称、IP、网关后保存退出）

# Let NetworkManager manage all devices on this system
network:
    ethernets:
        ens32:                    ## network card name
            dhcp4: false
            addresses:
              - 192.168.3.88/24   ## setstatic IP
            routes:
              - to: default
                via: 192.168.3.1  ## gateway
            nameservers:
              addresses: [8.8.8.8,8.8.4.4,192.168.3.1]
    version: 2

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519100726878-1424437783.png)




格式的缩进很容易出错， 可以直接复制以上模板，只修改网卡名称、IP和网关即可。

4、开启 systemd-networkd服务（可选）

sudo systemctl start systemd-networkd

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519101953156-1178199085.png)

5、查看systemd-networkd服务状态（可选）

sudo systemctl status systemd-networkd

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519102119391-2052351580.png)



说明systemd-networkd服务已经启动。

6、重启网络服务

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519095815778-1085608550.png)

 没有报错说明格式正确。

7、查看设定的IP是否生效

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519095933112-934286772.png)

 设定的IP已生效。

8、测试网络连接是否正常

ping -c 3 www.baidu.com

![](https://img2022.cnblogs.com/blog/1447599/202205/1447599-20220519100056865-1649581180.png)

 说明网络连接正常。

