#  🌈🌈linux内核安装

## 目录🌳
1. [介绍](#介绍)✈️
2. [前提条件](#前提条件)✈️
3. [下载内核源码](#下载内核源码)✈️
4. [配置内核](#配置内核)✈️
5. [编译内核](#编译内核)✈️
6. [安装内核](#安装内核)✈️
7. [验证安装](#验证安装)✈️

## 介绍🌳
***OpenWrt x86_64 虚拟机镜像（EFI 版本）***

***内核版本：Linux 5.15.134***

***OpenWrt 版本：23.05.0***

***构建时间：2025年6月***

***作者：Canhui Bao***



## 文件说明🌳

本压缩包包含一个可直接用于 VMware 的 OpenWrt x86_64 EFI 虚拟机镜像，适合进行开发调试与部署。

压缩包名称：
    openwrt_vm_efi_package.tar.gz

解压后内容：
    - openwrt-x86-64-generic-ext4-combined-efi.vmdk  → VMware 虚拟磁盘镜像
        - README.txt                                     → 使用说明
    - .config                                              → OpenWrt 编译配置文件（供复现使用）



## 前提条件🌳
在开始之前，请确保你具备以下条件：
- 一台已经安装了 Linux 操作系统的虚拟机（本教程以 Ubuntu24版本 为例）。
- 具备 sudo 权限的用户帐户。
- 足够的硬盘空间用于存放编译后的内核文件。
- 需要切换国内的软件源🏃[参考该博主](https://blog.csdn.net/Zzp750/article/details/145771731?ops_request_misc=&request_id=&biz_id=102&utm_term=Ubuntu24%E6%8D%A2%E5%8E%9F&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-145771731.142^v102^control&spm=1018.2226.3001.4187)
- 安装了一些必要的依赖工具（如 `gcc`、`make`、`libncurses-dev` 等）。
- 例如：在 Ubuntu 上，可以使用以下命令安装所需的开发工具和库：
  ```bash
  sudo apt update
  sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc

## 下载内核源码🌳
- 下载内核源码前可以先在虚拟机上创建一个文件夹用来保存下载的安装包 方便以后查询。
    ```bash
    makedir /你的文件名
- 可以直接去🏃[linux源码官网](https://www.kernel.org/)下载对应linux版本（这里采用linux5.15.134）
- 或者直接执行如下命令
    ```bash
    sudo apt update
    sudo apt install linux-image-5.15.134

## 配置内核🌳
-  使用`make menuconfig`对内核进行图形化配置
    ```bash
      make menuconfig
## 编译内核🌳
-  使用`make `对内核进行编译,其中x可以根据自己的cpu设置
      ```bash
     make  -jx
-  亦可直接复制如下指令
     ```bash
     make -j$(nproc) 
-  `-j$(nproc)` 会让编译使用系统上所有的 CPU 核心，这样会加快编译速度。如果你只想使用一个核心，移除 `-j$(nproc)` 即可。
## 安装内核🌳 
-  复制如下指令
     ```bash
     sudo make modules_install
     sudo make install
-  可以检查自己安装的模块是否在`lib`下面
     ```bash
     cd /lib/modules
     ls

## 验证安装🌳
-  在执行`sudo make install` 后会自动更新`grub`可以不做手动更新
-  重启系统默认会切换到最新的内核 ,也可以在启动界面手动进入高级选项选择内核，进入系统使用如下指令查看当前系统版本
    ```bash
     uname -a
-  使用如下指令查看电脑已经安装的内核
    ```bash
     dpkg --get-selections|grep linux

## 使用步骤（VMware 用户）🌳

1. 打开 VMware Workstation / Player，选择 “新建虚拟机”。
2. 虚拟机类型选择 “使用现有虚拟磁盘”。
3. 浏览并加载 `openwrt-x86-64-generic-ext4-combined-efi.vmdk`。
4. 网络建议设置为 “桥接模式（Bridged）”，确保虚拟机与宿主机同网段。
5. 启动虚拟机。

##  默认登录信息

- **默认 IP 地址**：192.168.1.1（首次启动）
- **用户名**：root
- **密码**：无密码（直接回车）

可使用 Xshell、MobaXterm、SSH 工具进行远程连接：

```bash
ssh root@192.168.1.1
```

由于已经配置DHCP自动获取主机ip地址，故可由opwrt虚拟机 命令查看

```bash
ip a
```

ip地址：192.168.31.18，xshell通过ssh命令即可连接与虚拟机连接

至此，所有工作均已完成，即可加载内核模块代码进行测试
常见问题：openwrt IP地址正常，网卡配置正常，如果xshell无法连接，则检查openwrt与主机是否处于同一网段，192.168.31.xx，要求二者处于同一网段方可连接成功。
