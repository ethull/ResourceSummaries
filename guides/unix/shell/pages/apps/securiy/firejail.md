# default options
    /boot – blacklisted, /bin – read-only, /dev – read-only; similar to the home directory, only a skeleton filesystem is available
    /etc – read-only; /etc/passwd and /etc/group have been modified to reference only the current user, /home – only the current user is visible
    /lib, /lib32, /lib64 – read-only, /proc, /sys – re-mounted to reflect the new PID namespace; only processes started by the browser are visible
    /sbin – blacklisted, /selinux – blacklisted, /usr – read-only; /usr/sbin blacklisted, /var – read-only; similar to the home directory, only a skeleton filesystem is available

    // --appimage: sandbox an AppImage application, 
    // --blacklist=[filename]: blacklist directory or file, --noblacklist=[filename]: disable blacklist for file or directory .
    //--whitelist=[fileName]
    // --chroot=[dirname]: chroot into directory, --private=[dirPath]: temporary home directory.
    // --hostname=[name]: set sandbox hostname, --hosts-file=[file]: use file as /etc/hosts, --dns=[address]: set DNS server.
    // --ip=[address]: set interface IP address.
    // --netstats: monitor network statistics, 
    // --noroot: install a user namespace with only the current user, --nosound: disable sound system, --novideo: disable video devices
    //--net=eth0: new network space, --ip=[custom IP]: 192.168.1.155
    //  --tree: print a tree of all sandboxed processes, --list: List all running sandboxes
    // --trace: trace open, access and connect system calls, --tracelog: add a syslog message for every access to files or directoires blacklisted by the security profile.
    // --x11: enable X11 sandboxing. The software checks first if Xpra is installed, then it checks if Xephyr is installed. If all fails, it will attempt to use X11 security extension.
    // -no-remote
    // --profile=[fileName] - use a custom profile, --profile.print=name|pid - print the name of profile file, --profile-path=directory - use this directory to look for profile files.	
    firejail {} [programName]

    //firejail all programs running by a user by changing default share
    chsh shell /usr/bin/firejail

    //EG
    firejail --blacklist=/home/emera;/mnt;/media --noroot --netstats --private=/home/virtualHome [programName] 

    firejail --net=none [pn]
    //view running sandboxes
        firejail --list
        firejail --top
    
    //use config from /etc/firejail/[pn].profile
    firejail [pn]
    
//todo 
add isolated network stack
