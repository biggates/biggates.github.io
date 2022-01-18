---
title: 在 Windows 11 家庭版中清理 docker desktop 的文件
layout: post
---

# 在 Windows 11 家庭版中清理 docker desktop 的文件

## 问题描述

Docker desktop 使用一段时间之后，在 wsl 中占用了大量硬盘空间。

使用文件查找功能，确定目标文件是 `C:\Users\xxx\AppData\Local\Docker\wsl\data\ext4.vhdx` 。

## docker 自带的功能

```bash
docker system prune -a --volumes
```

另外还找到一个：

```bash
docker run --privileged --pid=host docker/desktop-reclaim-space
```

不过没什么效果。


## 比较常见的解决方案

在安装了 Hyper-V 之后，使用

```powershell
optimize-vhd -Path xxx\xxx\ext4.vhdx -Mode full
```

来实现。然而 Hyper-V 在 Windows 家庭版中是没有的。

## 在 Windows 家庭版中的解决方案

1. 首先关掉 Docker Desktop

2. 关闭所有仍在运行的 wsl 虚拟机

    ```cmd
    c:\> wsl --shutdown
    ```

3. 然后打开 diskpart 工具（需要 UAC 验证）

    ```
    c:\> diskpart
    ```

4. 在 diskpart 工具中，做如下操作：

    ```cmd
    Microsoft DiskPart 版本 10.0.22000.1

    Copyright (C) Microsoft Corporation.
    在计算机上: xxxxx

    DISKPART> select vdisk file="C:\Users\xxx\AppData\Local\Docker\wsl\data\ext4.vhdx"

    DiskPart 已成功选择虚拟磁盘文件。

    DISKPART> attach vdisk readonly
    100 百分比已完成

    DiskPart 已成功连接虚拟磁盘文件。

    DISKPART> compact vdisk
    100 百分比已完成
    DiskPart 已成功压缩虚拟磁盘文件。

    DISKPART> detach vdisk
    DiskPart 已成功分离虚拟磁盘文件。

    DISKPART> exit
    ```

完成！

## 参考

* 实际方案： [microsoft/WSL](https://github.com/microsoft/WSL/issues/4699#issuecomment-627133168)
* 使用 docker/desktop-reclaim-space : [alexeysamoshkin@medium](https://medium.com/@alexeysamoshkin/reclaim-disk-space-by-removing-stale-and-unused-docker-data-a4c3bd1e4001)
