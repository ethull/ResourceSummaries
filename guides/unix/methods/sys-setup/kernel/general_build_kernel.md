# packages
    sudo pacman -S base-devel
    sudo pacman -S xmlto, kmod, inetutils, bc, libelf, git

#prep
## get source, check and clean
    mkdir ~/kernelbuild
    cd ~/kernelbuild
    wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.4.5.tar.xz
    //get signiture and pub key
        wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.4.5.tar.sign 
        gpg --list-packets linux-5.4.5.tar.sign
        gpg --recv-keys <fingerprint-from-previous-step>
    //prep kernel source and check signiture
        unxz linux-5.4.5.tar.xz
        gpg --verify linux-5.4.5.tar.sign linux-5.4.5.tar
    //unpack archive
        tar -xvJf linux-5.4.5.tar.xz
    //clean source tree
        cd linux-5.4.5 
        make mrproper

# config
## using custom config

### on arch linux
    zcat /proc/config.gz > .config
### general
    //if using an older config file
        /usr/src/linux-headers-5.4.5-24-generic/.config > .config
        //account for broken/new options by providing them defaults
        make olddefconfig
//change CONFIG_LOCALVERSION adn CONFIG_DEFAULT_HOSTNAME
sed -i "s/.*CONFIG_DEFAULT_HOSTNAME.*/CONFIG_DEFAULT_HOSTNAME=\"minimal\"/" .config

## or generate new config
    //create .config file that only enables options currently in use by current running kernel
        //plug in all required devices before using this command
            make localmodconfig
    // or advanced config, using any of the offered guided wizards
        //ncurses
            make nconfig
        //GUI, needs packagekit-qt5
            make xconfig
        //gtk GUI
            make gconfig

# compile
    //use parrallisation to compile
    make -j<numCores+1>

# install
    sudo su
    //kernel modeules
        make modules_install
    //kernel bzImage
        cp -v arch/x86_64/boot/bzImage /boot/vmlinuz-linux54
## make initial ram disk
# use present file
    cp /etc/mkinitcpio.d/linux.preset /etc/mkinitcpio.d/linux54.preset
    vim /etc/mkinitcpio.d/linux48.preset
        ...
        ALL_kver="/boot/vmlinuz-linux54"
        ...
        default_image="/boot/initramfs-linux54.img"
        ...
        fallback_image="/boot/initramfs-linux54-fallback.im
    mkinitcpio -p linux54
# or use mkinitcpio to gen initramfs manually
    mkinitcpio -k linux-5.4.5 -g /boot/initramfs-linux54.img
    //copy System.map, symlinks need to be supported (no FAT32)
        cp System.map /boot/System.map-linux54
        ln -sf /boot/System.map-linux54 /boot/System.map


#TODO complete patching
#=apply git patch
git apply file.patch

#=manually
patch -p1 < ~/anyname/0001-base-packaging.patch
