## nvidia xrun
    https://github.com/Witko/nvidia-xrun
    //notes
        //doesnt work with vulkan 
        //possibly highest FPS solution
    //install
        AUR
            yaourt nvidia-xrun
        fedora
            sudo dnf copr enable ekultails/nvidia-xrun
            sudo dnf install nvidia-xrun    
    //setup
        //autorun window manager 
            vim ~/.nvidia-xinitrc 
                if [ $# -gt 0 ]; then
                    $*
                else
                    openbox-session
                #   startkde
                fi
            //can now just use 
                nvidia-xrun
        //if errors
            //bbswitch error msgs
                //get busid
                    lspci | grep -i nvidia | awk '{print $1}' 
                //set busid correctly                
                    vim /etc/X11/nvidia-xorg.conf.d/30-nvidia.conf
                        Section "Device"
                            Identifier "nvidia"
                            Driver "nvidia"
                            BusID "PCI:2:0:0"
                        EndSection
    //use
         //swap tty
            ctrl+alt+F<TTYNumber> 
            //normally free
                ctrl+alt+F2 to ctrl+alt+F6
            //swap back to intel tty (most distros)
                ctrl+alt+F7
        //login
        nvidia-xrun <app> 
