#to xfce4
    sudo apt install xfce4
    //better looking terminal, can also change gnome terminal colourscheme instead
    sudo apt install xfce4-terminal 
    //batttery indicator for panel, may need to add manully
    sudo apt install xfce4-battery-plugin
# changing default environment
    //get sessions
    ls /usr/share/xsessions
    //change default session, manually or auto
        sudo vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
        sudo sed -i "/user-session/ s/[currentDefaultDesktop]/[newDefaultDesktop]/" /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf