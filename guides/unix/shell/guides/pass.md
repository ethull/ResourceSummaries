//console password manager
# shell
## setup
https://www.passwordstore.org/
//init
    pass init
//search
    //ls [subfolder]?: list folders
    //find [pwName]: list pw names close to pwName
    //grep [searchQuery] {}?...: search pw-files containing sQ
        pass {add pw to dir}
    pass -c [pwName]
//edit store
    //-m: multiline entry
        pass insert {}... [pwName]
    pass-edit [pwName]
    //Remove existing password or directory
        pass rm {--recursive, -r; --force, -f;} pass-name
    //Renames or moves old-path to new-path
        pass mv {--force,-f} [old-path] [new-path]                          
    , optionally forcefully
ectively reencrypting.                                          

//other
    //gen pw
    //-c: output to clipboard, -i: output inplace 
    //-f: force overwrite
        pass generate {}?... [pwName] [pwLength]

