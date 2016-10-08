---
layout: post
title: Windows扩容磁盘分区
author: HelloDBA
tags: [Windows]
category: Windows
---

进入cmd界面，输入diskpart命名：

```
C:\Documents and Settings\Administrator>diskpart

Microsoft DiskPart Copyright (C) 1999-2001 Microsoft Corporation.
On computer: HBSCSHARE

DISKPART>

```
输入list disk,显示磁盘列表

```
DISKPART> list disk

  磁盘 ###  状态        大小     可用     动态 Gpt
  --------  ----------  -------  -------  ---  ---
  磁盘 0    联机          70 GB    10 GB
  磁盘 1    联机         100 GB   672 KB   *

```
输入select disk 0,选择磁盘0（我们这次对磁盘0进行扩容）

```
DISKPART> select disk 0

磁盘 0 现在是所选磁盘。

```
输入list partition,显示磁盘0的分区列表

```
DISKPART> list partition

  分区 ###       类型              大小     偏移
  -------------  ----------------  -------  -------
  分区 1    主要                  20 GB    32 KB
  分区 2    扩展的                 40 GB    20 GB
  分区 3    逻辑                  40 GB    20 GB

```
输入select partition 3,选择分区3

```
DISKPART> select partition 3

分区 3 现在是所选分区。

```
输入list partition,看看分区3是否被选中（*标识的为选中）

```
DISKPART> list partition

  分区 ###       类型              大小     偏移
  -------------  ----------------  -------  -------
  分区 1    主要                  20 GB    32 KB
  分区 2    扩展的                 40 GB    20 GB
* 分区 3    逻辑                  40 GB    20 GB

```
输入extend

```
DISKPART> extend

DiskPart 成功地扩展了卷。

```
扩容成功了，不需要安装第三方软件，也不需要重启系统。