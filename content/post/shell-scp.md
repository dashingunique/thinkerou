---
title: 命令行艺术之 scp
categories: [
  "Shell"
]
tags: [
  "Shell"
]
date: 2015-12-20
image: "speakers.jpg"
---

## 概览

在 linux 服务器上工作，免不了有这样的场景：

> 从远程服务器上拷贝文件（文件夹）到本地或者另一台远程服务器上

如果是远程服务器跟本地相关的文件下载、上传操作，可以使用 `sz` 和 `rz` 即可完成，但是在两个远程服务器之间就得另寻他法了，这时就是命令 `scp` 该上场了。

## 命令详解

命令 `scp` 是 `Secure copy` 的缩写，类似的有 `cp` 命令，但它用于本机文件拷贝不能跨服务器，同时，命令 scp 是加密传输的。另外，命令 `rsync` 也能完成 scp 的工作，但当小文件较多时，rsync会导致磁盘 IO 过高。

命令 `scp` 的主要工作是：

> 在 linux 系统下基于 `ssh` 登录进行安全的远程文件拷贝命令

> 服务器之间的文件拷贝和目录拷贝

#### 1. 命令格式

> scp [参数] [原始路径] [目的路径]

在终端上直接输入 scp 则会有这样的帮助信息：

	usage: scp [-12346BCEpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           	   [-l limit] [-o ssh_option] [-P port] [-S program]
           	   [[user@]host1:]file1 ... [[user@]host2:]file2
           
#### 2. 常用命令参数

> -B 使用批处理，传输时不询问传输口令
> -C 使用压缩功能
> -p 保留原始文件的修改时间、访问时间和权限
> -r 递归拷贝目录，**重要，常用！**
           
#### 3. 使用示例

**从本地（本机）服务器拷贝到远程服务器**

> scp local_file remote_username@remote_ip:remode_folder

> scp -r local_folder remote _username@remote_ip:remode_folder

**从远程服务器拷贝到本地（本机）服务器**

> scp remote_username@remote_ip:remode_folder local_file 

> scp -r remote _username@remote_ip:remode_folder local_folder

## 参考资料

- [鸟哥的 linux 私房菜](http://linux.vbird.org/linux_basic/)
