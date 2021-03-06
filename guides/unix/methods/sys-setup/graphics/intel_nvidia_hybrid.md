# links
    https://wiki.archlinux.org/index.php/pacman
    https://unix.stackexchange.com/questions/320642/hdmi-port-doesnt-work-nvidia-intel-bumblebee-driver-for-laptop-with-manjaro-lin/440283
    https://forum.manjaro.org/t/solved-nvidia-prime-synchronization/18404
    https://forum.manjaro.org/t/hdmi-port-not-working-on-dell-inspiron-7567/41791/27
    https://forum.manjaro.org/t/external-monitors-with-bumblebee/20726/10

    https://wiki.archlinux.org/index.php/NVIDIA#ConnectedMonitor
    https://wiki.archlinux.org/index.php/NVIDIA/Tips_and_tricks#Enabling_SLI
    https://github.com/Bumblebee-Project/Bumblebee/issues/21
    https://forum.manjaro.org/t/howto-set-up-prime-with-nvidia-proprietary-driver/40225

    https://docs.xfce.org/xfce/xfce4-settings/display

    https://wiki.archlinux.org/index.php/NVIDIA_Optimus
    https://github.com/dglt1/optimus-switch

# listing hardware
    inxi -Fxxxz
    lspci

# arch linux
## optimus switcher
    //add a command that swaps activate GPU and drivers on reboot
    //force display manager to detect unrecognised screens
        //fix process, no reboot till the end
            //prep drivers
                sudo systemctl disable bumblebeed --now
                sudo mhwd -r pci video-hybrid-intel-nvidia-bumblebee
                sudo mhwd -i pci video-nvidia
                sudo pacman -S linux419-headers dkms acpi_call-dkms xorg-xrandr xf86-video-intel git
                sudo modprobe acpi_call
            //install optimus switch (so you can swap graphics card)
                cd ~
                git clone https://github.com/dglt1/optimus-switch.git
                cd ~/optimus-switch
                chmod +x install.sh
                sudo ./install.sh
            //setup configs
                //now edit lightdm.conf
                    sudo nano /etc/lightdm/lightdm.conf
                //go to the bottom and find this line, uncomment it and edit it to match
                    display-setup-script=/usr/local/bin/optimus.sh
                //save/exit

        //check results
            mhwd -li
            ls -laR /etc/X11 ; cat /etc/X11/xorg.conf.d/*.conf
            ls -la /etc/modprobe.d ; cat /etc/modprobe.d/*.conf
            ls -la /etc/modules-load.d ; cat /etc/modules-load.d/*.conf
            cat /etc/lightdm/lightdm.conf
            pacman -Qs | grep -Ei 'xrandr|nvidia|acpi_call|xf86-video|bumble|bbswitch|primus'

        //useage scripts to run
            //get info on current gpu
                sudo /etc/switch/gpu_switch_check.sh
            //change to intel only
                set-intel.sh
            //change to nvidia optimus mode
                set-nvidia.sh

        //if nvidia continues running after switching to intel-only mode
            sudo /etc/switch/gpu_switch_check.sh
            //take note of outputs that are followed with "works!"
                Trying \_SB.PCI0.PEG0.PEGP._OFF: /etc/switch/gpu_switch_check.sh: line 63: warning: command substitution: ignored null byte in input
                    works!
                Trying \_SB.PCI0.GFX0._DSM: /etc/switch/gpu_switch_check.sh: line 63: warning: command substitution: ignored null byte in input
                    works!
                Trying \_SB_.PCI0.RP01.PXSX._DSM: /etc/switch/gpu_switch_check.sh: line 63: warning: command substitution: ignored null byte in input
                    works!
                Trying \_SB_.PCI0.PEG0.PEGP._DSM: /etc/switch/gpu_switch_check.sh: line 63: warning: command substitution: ignored null byte in input
                    works!
                    
            sudo nano /etc/switch/intel/no-optimus.sh
                //uncomment last 2 line of the file, if acpi_call is not in one of the working calls, replace it
                //also change bus id of nvidia-card (if it isn't 1:00:0), get bus-id
                    inxi -Gx
