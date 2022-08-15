# rm删除文件

-f 删除文件夹

-r 强制删除

-rf  危险

# touch创建文件

stat 查看文件状态

>File: ‘libai’
Size: 13        	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 67172206    Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2022-03-31 21:58:16.943008014 +0800 //访问时间
Modify: 2022-03-31 21:57:51.730007800 +0800  // 源数据发生变化的时间
Change: 2022-03-31 21:57:51.730007800 +0800

# ln 链接



## 软链接

 ln -s qiangjinjiu slink

## 硬链接

ln qiangjinjiu hlink

## ln 原理

> 软链接就是软拷贝。
> 	创建一个link文件，link文件指向link对象
> 	用于指向文件
> 硬链接就是深拷贝。
> 	创建一个指针，指向link文件。
> 	可以保护文件

> 但是Linux有指针计数器,在有多个link指向文件时,不会删除文件物理数据。只会删除他的link
