电脑上本来有两个系统，Ubuntu和win10，所以刚攒电脑的时候买了两块硬盘，后来不想用Windows了，就把win卸了，就空出一个1T的机械硬盘，就想着把它挂载到Ubuntu下。


# 查询现有硬盘情况

    # fdisk -l

结果如下：

    root@jerry-Z170X-UD3:/home/jerry# fdisk -l
    Disk /dev/sda: 232.9 GiB, 250059350016 bytes, 488397168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: gpt
    Disk identifier: DBD744F1-F9CC-4EE6-AE6D-FA926FB0077D

    Device         Start       End   Sectors   Size Type
    /dev/sda1       2048   1050623   1048576   512M EFI System
    /dev/sda2    1050624 421392383 420341760 200.4G Linux filesystem
    /dev/sda3  421392384 488396799  67004416    32G Linux swap

    # 这个/dev/sdb就是我的第二块硬盘
    Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: gpt
    Disk identifier: B5DA5FB1-7D69-49A4-9FAD-5514DF18B389


# 挂载硬盘

## 创建未被挂载的磁盘分区


    # fdisk /dev/sdb

    Welcome to fdisk (util-linux 2.27.1).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.

    Command (m for help): p
    Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: gpt
    Disk identifier: B5DA5FB1-7D69-49A4-9FAD-5514DF18B389

    Command (m for help):

此时会进入到Command状态，操作是这样的：

* 输入m查看帮助
* 输入p查看/dev/sdb/分区状态
* 输入n创建sdb这块硬盘的分区
* 输入w保存并退出Command状态

下面是命令行的操作过程：

    Command (m for help): m   //m: 查看帮助

    Help:

      Generic
       d   delete a partition
       F   list free unpartitioned space
       l   list known partition types
       n   add a new partition
       p   print the partition table
       t   change a partition type
       v   verify the partition table
       i   print information about a partition

      Misc
       m   print this menu
       x   extra functionality (experts only)

      Script
       I   load disk layout from sfdisk script file
       O   dump disk layout to sfdisk script file

      Save & Exit
       w   write table to disk and exit
       q   quit without saving changes

      Create a new label
       g   create a new empty GPT partition table
       G   create a new empty SGI (IRIX) partition table
       o   create a new empty DOS partition table
       s   create a new empty Sun partition table



    Command (m for help): p      //p：查看分区状态
    Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: gpt
    Disk identifier: B5DA5FB1-7D69-49A4-9FAD-5514DF18B389


    Command (m for help): n     //n：分区
    Partition number (1-128, default 1): 1    //区号，默认1，如果没有特殊需求，直接回车即可
    First sector (34-1953525134, default 2048):     //开始扇区，因为就分一个区，直接回车即可
    Last sector, +sectors or +size{K,M,G,T,P} (2048-1953525134, default 1953525134):   //最后一个扇区，回车即可

    Created a new partition 1 of type 'Linux filesystem' and of size 931.5 GiB.   //创建成功的提示


    Command (m for help): p     //p:在查看分区情况
    Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: gpt
    Disk identifier: B5DA5FB1-7D69-49A4-9FAD-5514DF18B389

    Device     Start        End    Sectors   Size Type
    /dev/sdb1   2048 1953525134 1953523087 931.5G Linux filesystem
    //已经分好了


    Command (m for help): w     //w：保存
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.



再查看现有硬盘情况

    # fdisk -l

    root@jerry-Z170X-UD3:/home/jerry# fdisk -l

    // sda
    Disk /dev/sda: 232.9 GiB, 250059350016 bytes, 488397168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: gpt
    Disk identifier: DBD744F1-F9CC-4EE6-AE6D-FA926FB0077D

    Device         Start       End   Sectors   Size Type
    /dev/sda1       2048   1050623   1048576   512M EFI System
    /dev/sda2    1050624 421392383 420341760 200.4G Linux filesystem
    /dev/sda3  421392384 488396799  67004416    32G Linux swap


    //sdb
    Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: gpt
    Disk identifier: B5DA5FB1-7D69-49A4-9FAD-5514DF18B389

    Device     Start        End    Sectors   Size Type
    /dev/sdb1   2048 1953525134 1953523087 931.5G Linux filesystem


## 格式化未被挂载的硬盘

使用 mkfs.ext3 /dev/sdb1命令


    root@jerry-Z170X-UD3:/home/jerry# mkfs.ext3 /dev/sdb1
    mke2fs 1.42.13 (17-May-2015)
    /dev/sdb1 contains a ntfs file system labelled '新加卷'
    Proceed anyway? (y,n) y
    Creating filesystem with 244190385 4k blocks and 61054976 inodes
    Filesystem UUID: aa976e73-6dd5-472f-9627-0d7f8f8a43ad
    Superblock backups stored on blocks:
    	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
    	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
    	102400000, 214990848

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done  
## 查看磁盘的UUID

使用blkid命令


    root@jerry-Z170X-UD3:/home/jerry# blkid
    /dev/sda3: UUID="c203de07-d396-4342-8690-be6437fa8551" TYPE="swap" PARTUUID="b2e16971-deed-4baf-92d6-5775e5a5079f"
    /dev/sda1: UUID="BB41-3648" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="581945ac-51de-4651-8a5f-ae0a7da532fc"
    /dev/sda2: UUID="bd474838-588a-412c-adde-4e0d95272eaf" TYPE="ext4" PARTUUID="f5a28048-2dbf-4edb-b5af-c113f81f8f49"
    /dev/sdb1: UUID="aa976e73-6dd5-472f-9627-0d7f8f8a43ad" SEC_TYPE="ext2" TYPE="ext3" PARTUUID="0ae2cad0-0580-4062-a2c5-9551e169476b"

复制下sdb1的UUID

## 编辑/etc/fstab

    root@jerry-Z170X-UD3:/home/jerry# editor /etc/fstab

在/etc/fstab中添加下面一句：

    UUID=aa976e73-6dd5-472f-9627-0d7f8f8a43ad /home           ext4   defaults,error=remount-ro	0

/home为你要挂载的目录

##  重启电脑

    # reboot

哦了
