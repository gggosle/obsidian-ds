lsblk - show the available disks and their partitions
fdisk + the name of the disk (/dev/sda ) - interactive utilily to make partitions on the disk 
mkfs.ext4(or whatever else there was) - create a fs 
mount /dev/sdb1 /some-dir-idk 
umount 
/etc/fstab - mounting points on startup
mount -a -- to activate the above
ro, noatime(do not change the access time)
