# links
    https://wiki.archlinux.org/index.php/Mutt
    https://github.com/firecat53/urlscan
    https://github.com/wofr06/lesspipe
    https://github.com/Konfekt/vim-mutt-aliases/
# dirs
    /usr/share/doc/mutt/examples


# shell
    //setup
        mkdir ~/.config/mutt
        cd ~/.config/mutt
        touch muttrc
        wget https://gist.githubusercontent.com/LukeSmithxyz/de94948264649a9264193e96f5610c44/raw/d274199d3ed1bcded2039afe33a771643451a9d5/colors.muttrc 

    //send msg
        mutt -s "Subject" somejoeorjane@someserver.com < /var/log/somelog
        mutt -s "Subject" somejoeorjane@someserver.com -a somefile < /tmp/sometext.txt

    //open second mutt (in recomended read-only mode)
        mutt -R

# keys
    move
        basics
            vim keys: h j k l
            enter: open
            q: quit
        ?: get bound keys
        r: reply
        c: open mail boxes
