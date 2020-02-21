### linux分区(partition)

硬盘分区的表示：在Linux 是通过hd*x 或 sd*x 表示的，
其中 * 表示的是a、b、c …… ;
x表示的数字 1、2、3 …… ;
hd大多是IDE硬盘；sd大多是SCSI或SATA；

fdisk -l

df -ah

### 文件系统

操作系统中的数据分为文件内容和文件属性两部分，其中文件内容就是文件的实体数据，而文件属性就是文件类型、权限、属主、修改时间等信息。操作系统会将上述文件的内容放入磁盘文件系统的inode中，而把文件的实体数据存放于对应的block中。除了inode和block信息外，操作系统还会记录文件系统的整体信息于superblock中，这个superblock包括整个文件系统的inode和block的总的数量，已经使用的数量，剩余数量等。

在linux支持的文件系统类型中，其中ext2、ext3、ext4是Red hat和Centos采用的默认文件系统类型,其中ext2、ext3、ext4是依次升级的ext文件系统版本，这些不同的文件系统的高版本是向下兼容的，因此，我们就从ext2文件系统开始给大家文件系统的相关知识体系。

Centos7采用XFS文件系统,Centos6采用ext4文件系统,Centos5采用ext3文件系统.

一个ext2文件系统一般都会包含至少inode内容与block区域这两个部分

mkfs(make filesystem)命令，用来在特定的分区建立Linux文件系统，mkfs本身并不执行建立文件系统的工作，
而是调用相关程序来执行。

`mkfs  -t  ext2  -b  4096  -i  1024  /dev/sdb1`

文件系统小结：

1、文件系统是对一个存储设备上的数据和元数据进行组织的一种机制

2、分区必须格式化创建文件系统才能存放数据

3、一个分区只能有一种文件系统

4、linux下常见文件系统ext2、ext3、ext4、zfs、xfs（Centos7）和Reiserfs（单独安装）。


swap


### 磁盘管理命令总结：

磁盘与目录的容量：df，du

df：列出文件系统的整体磁盘使用量

du：评估文件系统的磁盘使用量（常用于评估目录所占容量）,查看某个目录所占空间大小

1、将容量结果以易读的容量格式显示出来：df  -h

2、将 /etc 下面的可用的磁盘容量以易读的容量格式显示出来：df  -h  /etc

3、将目前各个分区当中可用的 inode 数量列出：df  -ih

4、检查根目录下面每个目录所占用的容量：du  -sm  /*

磁盘分区：fdisk

`fdisk -l`

`fdisk /dev/sda`


blkid 这个命令是用来显示磁盘分区uuid的，uuid其实就是一大串字符，在linux系统中每一个分区都会有唯一的一个uuid


磁盘检验：fsck，badblocks

开机挂载:

/etc/fstab 及 /etc/mtab:

将/dev/sdb2每次开机都自动挂载到/tmp/sdb2。编辑/etc/fstab, 写入：

`/dev/sdb2     /tmp/sdb2     ext3     dufaults     0     0`

其他：

mount/unmount 挂载卸载文件系统

tune2fs 修改文件系统信息

megacli 查看raid信息

ipmitools 查看硬件信息工具

resize2fs 调整文件系统大小（LVM，drbd）
