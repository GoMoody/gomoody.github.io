---
title: "记录-解决Exsi无法删除、取消注册虚拟机"
date: 2023-09-01T15:23:24+08:00
lastmod: 2023-09-01T15:23:24+08:00
draft: false
author: "Song"
description: "记录一次解决Exsi虚拟机的操作问题"
images: []

tags: ["Exsi", DevOps"]
categories: ["documentation"]

lightgallery: true

toc:
  auto: false
---

`背景描述`：在2020年左右，安装完Exsi虚拟机之后， 创建了一台虚拟机，当时也做了删除，但是依然无法彻底删除

<!--more-->

## 1 常规步骤

如果一个虚拟机不需要使用了，

- [ ]  需要通过UI界面 https://xx.xx.xx.xx/ 使用管理员账号登录

- [ ] 点击左侧虚拟机/Virtual Machine

- [ ] 在需要删除的虚拟机上右击，点击删除即可

- [ ] > 在删除之前，需要先关机/关闭电源（一定要等任务跑完再操作）

    ![image-20230901153118588](https://yingsongxue.github.io/static/img/2023/upgit_20230901_1693553478.png)

## 2 特殊操作

当常规操作已经无法删除，或者删除的过程中，你尝试一些会导致任务中断的事情`关机`、`重新配置`等操作， 导致的异常情况。

- [ ] 虚拟机文件路径：/vmfs/volumes/613b3787-xxxxxxx-xxxx-a0369fa88738/`虚拟机名称` 。多个硬盘需要确认正确路径.
- [ ] 虚拟机的目录文件的路径/etc/vmware/hostd/vmInventory.xml

### 2.1 启用SSH终端访问

- [ ] 管理->服务->搜索 (==ssh==)，点击启动

    ![image-20230901153625107](https://yingsongxue.github.io/static/img/2023/upgit_20230901_1693553785.png)

- [ ] 主机->操作->服务->启用SecureShell(SSH)

- [ ] ![image-20230901153707408](https://yingsongxue.github.io/static/img/2023/upgit_20230901_1693553827.png)



### 2.2 删除虚拟机的文件

通过UI或者SSH都可以直接删除虚拟机的文件

- [ ] UI操作

    - [ ] UI 存储->数据存储->数据存储浏览器
    - [ ] 找到对应的硬盘，找到对应的虚拟机文件夹
    - [ ] 删除掉
    - [ ] ![image-20230901155328602](https://yingsongxue.github.io/static/img/2023/upgit_20230901_1693554808.png)

- [ ] SSH操作

    - [ ] ```shell
        cd /vmfs/volumes/xxxx-xxxx/
        ls
        `找到你的虚拟机`
        rm -rf 虚拟机路径
        ```

        

### 2.3 取消注册

获取所有的虚拟机编号

```shell
vim-cmd vmsvc/getallvms
```

![image-20230901160639995](https://yingsongxue.github.io/static/img/2023/upgit_20230901_1693555600.png)

如果看到有下面的提示，就是无效虚拟机的编号

```shell
Skipping invalid VM '2'
```

移除无效的虚拟机。（这个跟UI上调用unregister是一样的）

```shell
vim-cmd vmsvc/unregister 2
```

如果你还遇到了提示：

exsi 至少有一个虚拟机无法取消注册，或者像我这里遇到的`unregister Permission to perform this operation was denied`

### 2.3 手动取消注册

当调用的方法无法正确取消注册的虚拟机时，那就只有手动维护了，请在操作的时候谨慎一点，避免误删除错误的虚拟机，虚拟机列表的路径为：`/etc/vmware/hostd/vmInventory.xml`

```xml
<ConfigRoot>
  <ConfigEntry id="0002">
    <objID>11</objID>
    <secDomain>17</secDomain>
    <vmxCfgPath>/vmfs/volumes/613b3787-f424611a-xxxx-a0369fa88738/CentOS_NAS/CentOS_NAS.vmx</vmxCfgPath>
  </ConfigEntry>
  <ConfigEntry id="0003">
    <objID>12</objID>
    <secDomain>19</secDomain>
    <vmxCfgPath>/vmfs/volumes/613b3787-f424611a-xxxx-a0369fa88738/WebSite/WebSite.vmx</vmxCfgPath>
  </ConfigEntry>
  <ConfigEntry id="0009">
    <objID>21</objID>
    <secDomain/>
    <vmxCfgPath>/vmfs/volumes/613b3787-f424611a-xxxx-a0369fa88738/HomeAssistant_HomeKit/HomeAssistant_HomeKit.vmx</vmxCfgPath>
  </ConfigEntry>
</ConfigRoot>
```

每个虚拟机的实体如下

```xml
<ConfigEntry id="0009">
    <objID>21</objID>
    <secDomain/>
    <vmxCfgPath>/vmfs/volumes/613b3787-f424611a-xxxx-a0369fa88738/HomeAssistant_HomeKit/HomeAssistant_HomeKit.vmx</vmxCfgPath>
</ConfigEntry>
```

可以通过vi编辑器，

1. 上下移动行，然后按两次dd，可以删除一行
2. 按ESC键，输入:wq号可以保存， 输入:q!可以不保存退出

```shell
vi /etc/vmware/hostd/vmInventory.xml
/etc/init.d/hostd restart
/etc/init.d/vpxa restart
```



{{< admonition >}}
工作中总会遇到各种各样的问题。 通过关键字搜索，问题拆分，可以一步一步定位问题。 对于解决不了的问题， 可以尝试回本溯源，找到可以对问题产生根本影响的因素，尝试去解决。
{{< /admonition >}}

