```sh
# disk free -human
# df -h
df -h
```

Block device:
  Set of addressable blocks of data
  - Magnetic hard drive
  - SSD
  - USB Flash storage

Filesystem:
  -
Filesystem journaling
  - Prevent data loss or corruption due to power loss
  - Adds overhead to writes
  - Slightly more performant

1. File  write starts: (Update block 3 with data)
2. Journal: (Work being done on block 3)
3. Data is written to block 3
4. Journal entry is deleted

ext  - Extended file system (1992)
ext2 - First FS to support extended attributes (x-attrs) and 2tb+ drives (Better for flash memory)
ext3 - First FS to support journaling
ext4 -  Designed to be backwards compatible and added additional features(?)

btr-fs - B-Tree File System. Allows drive pooling, snapshots, compression, online defragmentation

reiserFS - 2001 more efficient for small text files. Unlikely to continue dev

ZFS - Designed by sun microsystems. Drive pooling, snapshots, dynamic disk stripping. Each file has a checksum. Open source. Easy to install

XFS - 1994 Similar to ext4 Can grow but not shrink on demand Great for large files bad for small files

JFS - Journaled File System. Developed by IBM. Low CPU usage. Good performance regardless of size. Partitions can be dynamically enlarged

Swap - Not a file system, virtual memory. Scratch space for data that won't fit in ram. Hibernating. Analgous to windows paging file

FAT16, FAT32, exFAT - Microsofts unjournaled File Allocation Table FS. Great for USBS that will be used with windows, linux and apple hardware


Review
 - What is a FS
 - What is journaling
 - What is EXT4
 - What is BTRFS
 - What are ReiserFS, ZFS, XFS, JFS
 - How is FAT cross platform
 - What is swap space


> x-attrs -

```
# a; page
cut -d ";"  -f1 data.txt
# -> a


```
