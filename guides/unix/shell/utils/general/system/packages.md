# Package manager 
## apt-get 
Debian package manager, apt or apt-get
  //Update sources (servers with package info/download links)
      sudo apt update
  //Update installed software
      sudo apt upgrade
  //Download/remove software
      sudo apt install [packageName]
      sudo apt remove [packageName]
      //Fix installation problems
          //Force package installation
          sudo apt-get install -f
          //try to remove problomatic package or run CLean commands
      
  //Get info on a package
      aptitude show [packageName]
  //Search for a package
      sudo apt search [packageName]
  //Clean
      //Remove half-installed packages
      sudo apt-get autoclean
      //Remove apt cache
      sudo apt-get clean
      //Remove uneccessary software dependancies
       sudo apt-get autoremove
  //list explicitly installed programs

      comm -23 <(apt-mark showmanual | sort -u) <(gzip -dc /var/log/installer/initial-status.gz | sed -n 's/^Package: //p' | sort -u)
      comm -23 <(aptitude search '~i !~M' -F '%p' | sed "s/ *$//" | sort -u) <(gzip -dc /var/log/installer/initial-status.gz | sed -n 's/^Package: //p' | sort -u)

      aptitude search '~i !~M' -F '%p' --disable-columns | sort -u > currentlyinstalled.txt
      wget -qO - http://mirror.pnl.gov/releases/precise/ubuntu-12.04.3-desktop-amd64.manifest \
        | cut -f1 | sort -u > defaultinstalled.txt
      comm -23 currentlyinstalled.txt defaultinstalled.txt
## pacman 
arch linux package manager
    //-S: sync packages
     //[packageName]: install package, -u: upgrade installed packages, y: download fresh pacakges, --noconfirm: dont ask, --ignore: ignore a packages upgrade
     //s: search for package
      //[packageName]: apply previous option to selected package 				    					    
    //-R [packageName]: remove software
    //-Q: query local packages
     //-u: list outdated packages, -i: view package info, -l: list packages
          //[packageName]: apply previous option to selected package 
    //-U [packageName]: upgrade specific package
            sudo pacman {} {}?...
    //EG Upgrade all packages 
        sudo pacman -Syu
## AUR
arch user repositary
    //first install base level package group
        pacman -S --needed base-devel
    //manual
        //download package
            git clone https://aur.archlinux.org/package_name.git
        //or go to the AUR website and download snapshot, then extract
            tar -xvf package_name.tar.gz
        //download from link, extract afterwards
            curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/package_name.tar.gz
       cd package_name
       //check contents
            less package
        //build with the arch build system
            //--s: resolves and installs dependancies, -i: installs package
                makepkg -si
            //or run makepkg -s and then instal yourself
                pacman -U package_name.pkg.tar.xz
    //aur helpers
        yaourt 
            wrapper for pacman that includes support for AUR
            //install
                
            //can use all pamac commands
                //[package name]: search AUR for package (then type its number), --stats: package installation stats, 
                yaourt {}...
        yay 
            //more modern pacman wrapper
## yum 
    //Fedora package manager, automatically updates sources	
    //Upgrade installed software, blank: Update all software, [packageName]
        sudo yum update {}
    //Download/remove software
        sudo yum install [packageName]
        sudo yum remove [packageName]
    
    
# Packaged software 
## dpkg 
    // .deb files are packages generated for Debian-based distros, install with dpkg
    // Install debian package
        sudo dpkg -i /home/user/cowsay.deb
    //Remove
        sudo dpkg -r cowsay
    //List packages installed
        dpks -l
    //RE-configure package database (fix dpkg corruption problems)
        sudo dpkg --configure -a
## rpm 
    //Generated for Fedora-based distros
    //Install
        rpm -Uvh /home/user/cowsay.rpm
    //Remove
        rpm -e cowsay
    //List packages
        rpm -qa
# Building from source 
    //Look for INSTALL txt file
    //cd into direcotry with source code ->  run configure ELF (executable) file to generate system specific makefile ->   run make to compile source code into an ELF ->  
     // run sudo make install to move binary created and required files to system folders (now they can be used anywhere)	
        cd [pathToSourceCode]
        make [sourceCode]
        make install 
        
    git clone [url]
    cd [projectBin]
    mkdir build
    cd build
    cmake ..
    //Raise number after j relative to PC hardware
    make -j8
    sudo make install		