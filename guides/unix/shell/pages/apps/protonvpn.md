# shell
    sudo protonvpn-cli -init
    //or
    sudo pvpn -init
    //Login
    //Connect
        sudo protonvpn-cli -connect
        protonvpn-cli -c [server-name] [protocol]
        protonvpn-cli -r, --random-connect
        protonvpn-cli -l, --last-connect
        protonvpn-cli -f, --fastest-connect
        
    //Dialog
        sudo protonvpn-cli -m
    protonvpn-cli --reconnect
    sudo protonvpn-cli -disconnect

    protonvpn-cli --ip
