# shell
    //host
        //click insert guest additions, or install with apt (in guest)
            sudo apt install virtualbox-guest-additions-iso
    //guest
        chmod +x ./virtual_box_guest_additions.txt
        chmod +x /media/$USER/VBox_GAs_[version]
        ./virtual_box_guest_additions.txt

    //errors
        //if read only file system when trying to run 
        cp [pathToIso]/cdrom/VBLinuxAdditions.run ~/desktop

        //if Virtualbox guest additions vboxclient failed to register "resizing support" 
            // go to settings -> screen -> graphic driver: VBoxSVGA
