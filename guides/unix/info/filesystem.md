# filesystem hierarchy
    //notes
        on most systems path1 will be symlinked to path2
        [path1] -> [path2]

## /boot
kernel and boot loader files
    vmlinuz-[kernel version]x[architecture] - kernel in obj form
    initrd.[ver].img (initialising ram disk) and initramfs - loads temp root file system into memory
    config.[ver].el5 - kernel config options

    /boot/efi/ - contains boot loader execcutables
    /boot/grub/ - contains boot loader configuration files
        /boot/grub/grub.cfg

## /home
//contain user home directories, can be a different partition
    /home/$USER, $HOME, ~
        //config files
            ~/.config - XDG confgi
            ~/.ssh
            ~/.[individualConfigFileName]
        ~/.local/share - shared local resources
        ~/.local/bin - current user, binaries
        ~/.local/lib - local shared libraries

            
## /root	
home dir for root user

## dev (device files) (virtual files that repersent devices)
    references to drives
        ./sdax ./sdbx ./nvme0n1px
    virtual consoles/terminals
        ./tty - the controlling terminal
        ./tty[virtualConsoleNumber, 0-63] - ref to specific virtual consoles
        ./ttyS[virtualConsoleNumber, 0-31] - serial port terminals
        ./console - terminal used in runlevel 1
        pseudo terminals
            ./pts/
    pseudo-devices (device nodes that dont correspond to physical devices)
        ./random ./urandom - gen random chars
        ./null - gen infinite null bytes
        ./zero - gen infinite zeros
        ./loop[0-7] - useful to mount stuff
    ./char/ - legacy char file locations (symlinks now)
    ./block/ - legacy blk device location (symlinks now)
    ./ppp - communicates with GPS/GPRS enabled hardware file to send data





## /sys
info about system
    mostly hardware or low level system software info

## /bin
    //essentional binaries, most gnu utils/cmds
    //generally /bin binaries are needed earily in the boot process or are more important
    /bin -> /usr/bin
    /sbin
        essentional special/system binaries, generally root/only
        /sbin -> /usr/sbin

## library folders
    shared libraries for binaries, including kernel modules
    //essentian
    /lib - essentional, normally for /bin or /sbin binaries
        multi arch folders
            /lib/x86_64-linux-gnu - binaries compiled for amd64 architecture
        ./firmware - hardware firmware code
        ./modprobe.d - config dir for modprobe cmd
        ./modules - kernel modules, subdirs for each kernel
        ./hdparm - SATA/IDE params
    /lib64 /lib32 - alternate format essentional libs for 64 and 32 bit archs
    /libx32 - format for x32 archs
        
    //contains mounted paritions
        /media
            //removalble media
        /mnt
            //tmp mounts

## /proc (processess)
    info about currently running proccesses as viewed by kernel
    virtual file system, each numbered directory repersents a PID and contain a command that occ
    EG subdirs
        /proc/meminfo - system memory
        /proc/cmdline – Kernel command line information.
        /proc/console – Information about current consoles including tty.
        /proc/devices – Device drivers currently configured for the running kernel.
        /proc/dma – Info about current DMA channels.
        /proc/fb – Framebuffer devices.
        /proc/fss – Current filesystems supported by the kernel.
        /proc/iomem – Current system memory map for devices.
        /proc/ioports – Registered port regions for input output communication with device.
        /proc/loadavg – System load average.
        /proc/locks – Files currently locked by kernel.
        /proc/meminfo – Info about system memory (see above example).
        /proc/misc – Miscellaneous drivers registered for miscellaneous major device.
        /proc/modules – Currently loaded kernel modules.
        /proc/mounts – List of all mounts in use by system.
        /proc/partitions – Detailed info about partitions available to the system.
        /proc/pci – Information about every PCI device.
        /proc/stat – Record or various statistics kept from last reboot.
        /proc/swap – Information about swap space.
        /proc/uptime – Uptime information (in seconds).
        /proc/version – Kernel version, gcc version, and Linux distribution installed.	
        /proc/sys/ - kernel params categories & kps
                dev - params for specific devices
                fs - filesystem config
                net - network config
                vm - use of kernels virtual mem

## /run
    info about running sys since last boot
    storage of transient state files (these contain run-time information that may or may not need to be written early in the boot process and which does not require preserving across reboots)
        /run/media - distro specifc files, some distros mount partitions here
        /var/run - /run predecesor (didnt allow earily writing into the boot process), generally now symlinks to /run

## /tmp
    temporay files, these are cleany up in a certain amount of time

## /etc
    core system global configeration files, EG grub, ufw	
        /etc/default
        /etc/fstab - contains items to be automounted on boot 
        //system infomation
            //multi-platform
               /etc/os-release
            /etc/{}-release - debian, ubuntu, mint based
            /etc/arch-release - arch based
        /etc/inittab - initrd config

## /usr (unix system resources)
files for all/multi users
    //multi user config files
        /usr/share
            /usr/share/doc
        //data
        //libraries
    symlinked binaries - /bin, /sbin
    /usr/local/bin - local binaries, can put user defined binaries here
    
## /var
info subject to quick change over the course of system operation
system logging, user tracking, caches
    variables
        /var/cache - application cache
        /var/lib - contains dynamic data libraries and files

    /var/lock
        contains lock files created by programs to indicate that they are using a particular file or device
    /var/log - contains log files
    /var/run 
        contains PIDs and other system information that is valid until the system is booted again
    /var/spool - contains mail, news, printer queues

## /opt
    optional application software packages

## /srv
    site-specific data which are served by the system


# types
    The Virtual File System (VFS) abstraction layer is a layer between apps and the different fs types, so an app can work with any filesystem
    journaling
        writes a log file (journal) before cping files and journal marks task as complete after cp.
        causes the fs to be in a constant consistent state even if the system shuts down mid-copy. 
        also decreases boot time as only journal needs to be read (not the entire fs)

    ext4
        native linux fs, compatible with older ext2 and ext3.
        supports disk volumes <= 1 exabyte and file sizes <= 16 terabytes
    Btrfs (Better or Butter FS) 
        newer linux fs so unstable/compatiblity issues, supports snapshots, incremental backups, performance increase
    XFS 
        High performance journaling fs, suits large files such as with a media server
    NTFS and FAT
        Windows fss
    HFS+ 
        Macintosh fs

# disk anatomy
    subdivide disk into multiple blk devices
    partition table
        one per disk, tells sys how disk is partitioned
        partition start/end, bootable, disk sectors matched up to partitions
        MBR (master boot record)
            can have 4 primary, or 3 and an extended
            can add any num of logical inside extened
            supports disks upto 2 terabytes
        GPT (GUID partition table)
            has partition type with no limits
            each partition got globally unique ID (GUID)
            generally used in conjuntion with UEFI (as opposed to MBR and BIOS)
    fs low level structure
        boot block
            at first few sectors
            only one needed to boot the os, though each partition has one
        super block 
            single block that comes after the boot block
            contains fs context info, EG size of the inode table, logical blocks size and the fs size
        inode table 
            each file/dir has a unique entry in the inode table which contains file metadata
        data blocks 
            actual data for the files/dirs
# operation
## disk partitioning
    fdisk - basic cli partitioning tool, it doesn't support GPT
    gdisk - fdisk, only supports GPT
    gparted - GUI version of parted
    parted - cli tool that supports both MBR and GPT partitioning
        sudo parted
        select /dev/sdb2
        //type, start, end
        mkpart primary 123 4567
        //num, new start, new end
        resize 2 1245 3456
## make fs
    sudo mkfs -t ext4 /dev/sdb2
## mount
    sudo mount -t ext4 /dev/sdb2 /mydrive
    sudo mount UUID=130b882f-7d79-436d-a096-1e594c92bb76 /mydrive

    sudo umount /mydrive
    sudo umount /dev/sdb2
    //read free disk space
        df -h
    //read current dir disk useage
        du -h
## /etc/fstab
    auto mount fs at starup
    fields
        UUID - device identifier
        mount point - dir the fs is mounted to
        fs type
        options - other mount options
        dump - used by the dump utility to decide when to make a backup, default to 0
        pass - used by fsck to decide what order fs's should be checked, 0 = not checked
## swap
    allocate virtual mem to sys when out of RAM
    //initialize swap areas (needs blank partition), enable the swap device, disable
        mkswap /dev/sdb2  
        swapon /dev/sdb2 
        swapoff /dev/sdb2
    //add entry to /etc/fstab to persist on bootup
    2*RAM if RAM < 4GB

check/repair damaged fs
    sudo fsck /dev/sda

## inodes
    inode (index node) is an entry in inode table, one per file (dir is also a file)
    contains file metadata/context
    metadata
        File type - regular file, directory, character device, ...
        Owner, Group, Access permissions
        Timestamps - mtime (time of last file modification), ctime (time of last attribute change), atime (time of last access)
        num hardlinks to the file, file size
        num of blks allocated to the file
        pointers to the data blks of the file
            12 direct pointers, 13th to blk containing more pointers, 14th to more nested pointers, ...
        dont have: filename and contents themself
    created with filesystem relative to its size by an algorithm, cant run out if too many small files
    read inodes
        df -i
    identified by sequential number
        ls -i
        stat

## symlinks (symbolic/soft links)
    creates new file thats inode metadata points to anouther files inode
    shortcut link to file via path
    ln -s myfile myfilelink
    some filesystems will store these inline for quicker access time if they <60 bytes
## hardlink
    shares a files inode (doesnt create a new file, but gives it anouther name/path)
        cant be refered across fs's
        can edit either file to reflect changes
    ln myfile2 myhardlink
    link count: num hardlinks to inode, inodes delled when 0
