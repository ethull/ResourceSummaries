# shell
    //install
        apt install wine winetricks
    //make new wine config
        mkdir wine, cd wine
        //Set architecture, win32:, win64:,
            WINEARCH={} WINEPREFIX=~/wine/[bottleName] winetricks
    //edit wine config
        //In GUI: Select default wine prefix -> install dll or component: what u want, dotnet34 and dotnet 452, tahoma -> run wine.cfg -> drop applications into drive_c
