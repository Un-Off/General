## List, create, delete, and modify physical storage partitions

    lsblk (list block)  [type with part are partitions]

    sda, sdb are two different disks
    sda1, sda2 are two partitions in a disk

    /dev/ (disk path)
    sudo fdisk --list /dev/sda

    gpt-GUID Partition Table

    (to list, create, delete, and modify physical storage partitions)
    cfdisk /dev/sdb (or)
    gdisk /dev/sdb  (?, n - new partition)

## Configure and manage swap space
    when ram is full --> request for ram space --> long sleeping process is transferred to swap space --> new request can access ram

    swapon --show
    sudo mkswap /dev/vdb3
    sudo swapon --verbose /dev/vdb3 (do not persist on reboot)
    let's now swapoff /dev/vdb3

    sudo dd if=/dev/zero of=/swap bs=1M count=128 status=progress(dd write 1megabyte block 128 times)

    sudo chmod 600 /swap

    after doing the above make sure to formate them to swap but this time /swap not /dev/vdb3
    sudo mkswap /swap
    sudo swapon --verbose /swap
    swapon --show



## Create and configure file systems
    sudo mkfs.ext4 /dev/sdb1

    man xfs
    -L "12 char label can be here",  -i size=512 (i node size)
    xfsadmin to change properties for xfs

    man ext4 (mke2fs & ext4 are hard linked)
    mkfs.ext4 -L "backUpVolume" -N 500000 /dev/sdb2 (-N number of Inodes)
    sudo tune2fs -l /dev/sdb2  (-l list, tune2fs )
    tune2fs to change properties for xfs

## Mount file systems at or during boot
    ls /mnt/
    sudo mount /dev/vdb1 /mnt/
    sudo touch /mnt/testFile
    ls -l /mnt/
    lsblk
    sudo umount /mnt/
    (above steps do not persist on reboot)


    sudo vi /etc/fstab (contain 6 field)

    <disk_partition_path>   <mount_point>   <fileSystem_type>   <mount_type>    <dump (back-up)>    <error_scan (0 no, 1 high priority, 2 low priority)>
    /dev/vdb1               /mybackups      xfs                 defaults        0                   2     

    sudo systemctl daemon-reload
    sudo systemctl reboot (above changes will appear only after reboot)

    <disk_partition_path (preferred UUID as blkid />dev/vdb1)>

#### swap files
    /dev/vdb3               none      swap                 defaults        0                   0

## Mount file systems on demand
    sudo dnf install autofs
    sudo systemctl enable --now autofs.service

    sudo dnf install nfs-utils
    sudo systemctl enable --now nfs-server.service

    sudo vi  /etc/exports
    /etc 127.0.0.1(ro)   (clients can read only)
    sudo systemctl reload nfs-server.service

    sudo vim /etc/auto.master
        /shares/ /etc/auto.shares --timeout=400
    sudo vi /etc/auto.shares
        mynetworkshare -fstype=auto 127.0.0.1:/etc (-fstype =nfs4 =auto,ro nfs1.example.com:/etc)
        myext4files -fstype=auto :/dev/vdb2
    sudo systemctl reload autofs
    ls /shares/ (nothing will be present)
    ls /shares/mynetworkshare (we can see it as on-demand works)

    above is parent child.
    below is direct mounting. /- . / absolute_path

    sudo vi /etc/auto.master
        /- /etc/auto.master --timeout=400
    sudo vi /etc/auto.shares
        /mynetworkshare -fstype=auto 127.0.0.1:/etc
        /localfiles/myext4files -fstype=auto :/dev/vdb2
    sudo systemctl reload autofs
    ls /mynetworkshar
    ls /localfiles/myext4files

## Evaluate and compare the basic file system features and options
    findmnt (messed. shows virtual fs mounts too and only currently mounted fs.)
    findmnt -t xfs,etx4 (OPTIONS )
    sudo mount -o ro /dev/vdb2 /mnt

    sudo umount /dev/vdb1
    sudo mount -o allocsize=32k /dev/vdb1 /mybackups

    to mount on boot edit the <mount_type> option in the /etc/fstab



## Manage and configure LVM storage
    sudo dnf install lvm2
    pv - physical volume
    vg - volume group
    lv - logical volume

    man lvm
    sudo lvmdiskscan

    sudo pvcreate /dev/sdc /dev/sdd
    sudo pvs

    sudo vgcreate my_volume /dev/sdc /dev/sdd
    to extend the space for a vg
        1. make sure for a pv to be added into vg
            sudo pvcreate /dev/sde
        2. add the pv to vg
            sudo vgextend my_volume /dev/sde
    to reduce space ina vg
        sudo vgreduce my_volume /dev/sde

    sudo lvcreate  --size 2G --name <partition_name (or lv_name)> <volume_name>
    sudo lvcreate  --size 2G --name partition1 my_volume
    (note: lv is partition. multiple partition is possible a vg.)

    {pv1 (10), pv2 (20)} --> [vg(30)] --> {part1 (5), part2 (25)}

    sudo lvresize --extends 100%VG my_volume/partition1
    sudo lvresize --size 2G my_volume/partition1
    sudo lvdisplay

    sudo mkfs.xfs /dev/my_volume/partition1
    sudo lvresize --resizefs --size 2G my_volume/partition1


## Create and configure encrypted storage
    cryptsetup encrypts storage in two types : plain and luks
    sudo cryptsetup --verify-passphrase open --type plain /dev/vde mysecuredisk
    --verify-passphrase (ask to enter password twice on creation)
    open is a action to cryptsetup (open disk to write - read encrypted data)

    sudo mkfs.xfs /dev/mapper/mysecuredisk
    sudo mount /dev/mapper/mysecuredisk /mnt
    sudo umount /mnt
    sudo cryptsetup close mysecuredisk

    (luks type)
    sudo cryptsetup luksFormat /dev/vde
    sudo cryptsetup open /dev/vde mysecuredisk
    sudo mkfs.xfs /dev/mapper/mysecuredisk
    sudo cryptsetup close mysecuredisk
    sudo cryptsetup luksChangeKey /dev/vde (to change password)

    sudo cryptsetup luksFormat /dev/vde2
    sudo cryptsetup open /dev/vde2 mysecuredisk



## Create and manage RAID devices

### RAID 0
    N number of disk with X storage space = N*X 
    no redundant data, the disk is defective all the data init is lost
### RAID 1
    Mirrored array. one file is stored in all the disks.
    a file is mirrored in all the disks
### RAID 5
    minimum 3 disk required
    each disk has a parity of a file. (party: small information used to recover data)
    if 1 disk fails. data can be recovered from other 2
### RAID 6
    minimum 4 disk required
    each disk has a parity of a file. (party: small information used to recover data)
    if 2 disk fails. data can be recovered from other 2

### RAID 10 (1+0)
    RAID0 { (RAID1{1TB + 1TB}=1TB)  +  (RAID1{1TB + 1TB}=1TB) } = 2TB

    make sure remove LVM
    sudo vgremove --force my_volume
    sudo pvremove /dev/vdc dev/vdd dev/vde

    sudo mdadm --create /dev/md0 --level=0 --raid-devices=3 /dev/vdc dev/vdd dev/vde
    now /dev/md0 is new RAID storage space. we create file structure out of it as below
    sudo mkdir.ext4 /dev/md0

    to stop r deactivate a array
    sudo mdadm --stop /dev/md0

    on linux boot-up, linux scans for superblock, any devices have any information on them to raid array, if they have it will reassemble them at /dev/md127
    to avoid the above we can wipe super block as below
    sudo mdadm --zero-superblock /dev/vdc dev/vdd dev/vde

    to add spare disk_partition_path
    sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdc dev/vdd 
    sudo mdadm --manage /dev/md0 --add dev/vde
    (or to do it together follow the bellow)
    sudo mdadm --create /dev/md0 --level=1 --raid-devices=1 /dev/vdc dev/vdd --spare-devices=1 dev/vde

    cat /proc/mdstat

    sudo mdadm --manage /dev/md0 --remove /dev/vde

## Create, manage and diagnose advanced file system permissions

    echo "file content" > exFile
    sudo chown adm:ftp exFile
    exFile (rw-rw-r--)

    sudo setfacl --modify user:aaron:rw exFile
    (now exFile is with adm owner and ftp group. still aaron can read and write  )
    exFile (rw-rw-r--+)
    getfacl exFile
        user::rw-
        group::rw-
        other::r--
        mask:rw-
    mask is the maximum permission. if mask is r--. user, group, other can't write and execute. we can do this as bellow  
    sudo setfacl --modify mask:r exFile 
    sudo setfacl --remove user:aaron exFile 

    mkdir dir1
    setfacl --recursive -m user:aaron:rwx dir1/   (-m modify)
    setfacl --recursive --remove user:aaron dir1/   (-m modify)

    to append only mode
    echo "this is old content" > newfile
    echo "replace content" > newfile  (permission will be dined)
    echo "replace content" >> newfile  (permited)

    to change permission to immutable
    even with sudo permission immutable files can be deleted
    lsattr <filename> (to check is it immutable)
    sudo chattr -i newfile

## Setup user and group disk quotas for file systems
    sudo dnf install quota
    sudo vim /etc/fstab
        change to default,usrquota,grpquota 0 2
    sudo systemctl reboot

    sudo quotacheck --create-files --user --group /dev/vdb2
    will create aquota.group aquota.user to keep track of how much each using storage space in /dev/vdb2

    sudo quotaon /mnt/

    sudo mkdir /mybackups/aaron/
    sudo chown aaron:aaron /mybackups/aaron
    fallocate --length 100M /mybackups/aaron/100Mfile
    sudo edquota --user aaron
    fallocate --length 60M /mybackups/aaron/60Mfile
    sudo edquota --user aaron
    sudo edquota --edit-perioda
