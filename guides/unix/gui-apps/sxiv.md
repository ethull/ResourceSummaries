# cmds
    //-i: read file name from stdin
    //-t: start in thumbnail mode 
    //-z [num]: zoom to num%
    sxiv {}?... [fileName]
    
# keys
    //move between images
        0-9 - count, prefix cmds like p and n
        n - next, p - previous
        g - first, G - last
        enter - thumbnail mode
        //within thumbnail mode
            h, j, k, l - move between
    r - reload img
    D - rm img 
    //zoom
        +, -: zoom in/out
        =: zoom to 100%
        w: zoom to 100% but some fill window, W: fit img to win
    //rotate
        <: anti-clockwise, >: clockwise
        ?: 180 degrees
    //flip image
        |: horizontal, -: vertically
    m - mark/unmark img, M - reset all marks

