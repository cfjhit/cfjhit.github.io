---
title: windows10安装
description: 原生windows10安装并更改用户文件夹到d盘
date: 2025-03-25 11:06:00 +0800
categories: [Blogging, 操作系统]
tags: [安装]
---

## 前言
无论您是首次安装操作系统，还是升级旧版本、重装系统以提升性能。

在开始安装前，请务必做好以下准备：
备份重要数据：避免因意外情况导致文件丢失。
检查硬件兼容性：确保您的设备满足 Windows 10 的最低配置要求。
准备安装介质：U 盘或光盘（需提前制作启动盘），或确保网络安装环境稳定。

## 制作启动盘
1. [镜像下载](https://massgrave.dev/windows_10_links)
2. 镜像写入软件[rufus](https://rufus.ie/zh/)

## 设置BIOS系统从U盘启动
安装步骤
第一个安装界面shift + F10 打开控制台
diskpart 分区
```bash
select disk 0

clean

convert gpt

create partition efi size=512
create partition msr size=512
create partition primary size=102407
```
上面是100g的主分区500g = 512002

分好区以后按照步骤安装即可。
## 维护模式
打开win10的administrator账户，以维护模式进入迁移用户文件夹创建软连接
```bash
# 复制C:\Users下的所有文件到D:\Users
##参数说明：此命令为Windows的“强健文件拷贝”命令。
##		/E 表示拷贝文件时包含子目录（包括空目录）
##		/COPYALL 表示拷贝所有文件信息
##		/XJ 表示不包括Junction points（默认是包括的）
## 		/XD "C:\Users\Administrator" 表示不包括指定的目录,此处指定目录为："C:\Users\Administrator"
robocopy "C:\Users" "D:\Users" /E /COPYALL /XJ /XD "C:\Users\Administrator"
## 删除C:\Users文件夹 
##参数说明：此命令删除指定目录。
##		/S 删除指定目录及其中的所有文件。用于删除目录树。
##		/Q 安静模式。删除时不询问。  
rmdir "C:\Users" /S /Q   
## 创建(目录)软连接 C:\Users 指向 D:\Users
## 参数说明：此命令创建符号连接。
##		/J 连接类型为目录连接
mklink /J "C:\Users" "D:\Users"
```
登录系统后关闭administrator账户即可，完成了用户文件夹的迁移。
