---
layout: post
title: vmware挂载硬盘
categories: Blog
description: vmware挂载硬盘。
keywords: 挂载, 硬盘
---

## vmware挂载硬盘

1.**查看分区**

	fdisk -l    # 打印当前的磁盘分区表，这时我们可以看到磁盘的总量的确增加到40GB了，但是分区只有以前的那几个原有的分区。

2.**创建新的分区**
	
	fdisk /dev/sda   # sda 就是经过扩容的硬盘，为SCSI硬盘，IDE类型硬盘对应为hda，是对该硬盘进行操作	

	我们在这里是要添加一个新分区，即将扩容出来的那部分做成一个新分区，这样才能被操作系统挂载识别。	

	键入：    n    # 命令 n 用于添加新分区 	

	此时， fdisk 会让你选择添加为逻辑分区呢（编号从5开始）还是主分区（编号1 到4）。选择主分区，则键入p；选择逻辑分区键入l 。	

	由于我们选择的是主分区，因此	

	键入：    p    # 选择创建主分区	

	此时，fdisk会让你选择主分区的编号，如果已经有了主分区sda1，sda2，那么编号就选 3 依次类推，即要创建的该分区为 sda3。	

	键入：    3	

	此时，fdisk又会让你选择该分区的开始值这个就是分区的Start 值（start cylinder）；这里最好直接按回车，

	如果您输入了一个非默认的数字，会造成空间浪费。	

	此时键入： w    # 保存所有并退出，分区划分完毕 	

	我们现在还不能用这个分区，因为还没有格式化，这时要重启系统就能够在dev下面看到sda3，如果不重启就不能进行下面操作。	

	重启系统

3.**格式化该新添加的分区**		

	键入： mkfs -t ext4  /dev/sda3	
	或者   mkfs.ext4  /dev/sda3 格式化指定的分区，依次类推，现在的系统大部分都是ext4格式，如果你需要其它的，可以查看 mkfs 的帮助。

4.**挂载该分区**

	手动挂载，则键入：mount /dev/sda3  /home/mark/ework  表示将该新分区挂载到 /home/mark/ework 这个目录下面
	
	设置开机自动挂载，修改 /etc/fstab 文件，在这个文件里面添加一行：
	
	/dev/sda3      /home/mark/ework       ext4   defaults        0       1
	
	这一步建议不要照我的抄写，最好是在该文件中复制一行进行修改
	
	这时我们就能使用扩展的空间了。
	
	这时 df 一下看看。

5.**开机自动挂载说明**
	
	CentOS服务器可能有多个磁盘，但是突然掉电，原先手动挂载的磁盘，就不在了。于是，需要系统在开机时自动挂载我们需要的磁盘和文件夹。
    编辑/ect/fstab，可看到当前所有的挂载点，在最后追加需要挂载的信息。每一列代表的信息，如下
	device name    mount point    fs-type    options    dump-freq    pass-num 	

	- device name，设备名，例如/dev/sda1。
	- mount point，挂载点，是一个系统上存在的文件夹。
	- fs-type，要挂载设备的类型，例如ext4。用man fstab可以查到支持的类型。
	- options，挂载时采用的参数，一般是defaults。
	- dump-freq和pass-num，一般都设置为0，启动时不检查要挂载的设备。
