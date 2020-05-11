//terminal multipluxer
# shell
    //list-keys: list all bound keys, list-commands: list every tmux
    command
        //info: list every session/window/pane/pid, source-file ~/.tmux.conf: reload config
            tmux {}
        //list sessions
            tmux ls
        //attach to session and detact other clients connected to it
            tmux attach -d -t <session id>
        //detach session
            tmux detach -t <session id>
            tmux detach //current session
        //kill sessions
            tmux kill-session -t mysession
            tmux kill-session -a

# mostly keys
    //prefix = Ctrl + B
    //sessions
        //new session
            //-s [sessionName]: name session, -n [windowName]: set name of first window
                tmux {}?...
            //-s [sessionName]: name session
                :new {}?
        //kill session
            //-t [sessionName]: kill specific session, -a: kill all sessions, -a -t [sN]: kill all but that session
                tmux kill-ses {} 
        //rename session
            [prefix] $
        //detach from session, detach others on session
            [prefix] d, :attach -d
        //list sessions
            tmux ls
            [prefix] s
        //none: attach to last session, -t: attach to session via name
            tmux attach {}?
        //move session
            //previous, next
                [prefix] (, [prefix] )
                
    //Windows
        //Create window
            [prefix] c
        //Rename current window
            [prefix] ,
        //Close current window
            [prefix] &
        //move window
            //p: Previous window, n: Next window, [0 to 9]: Switch/select window by number
                [prefix] {}
        //Reorder window, swap window number 2(src) and 1(dst)
            swap-window -s 2 -t 1
        //Move current window to the left by one position
            swap-window -t -1
    //Panes
        //Toggle last active pane
            [prefix] ;
        //Split pane vertically, Split pane horizontally
            [prefix] %, [prefix] "
        //Move the current pane left, Move the current pane right
            [prefix] {, [prefix] }
        //Switch to pane to the direction
            [prefix] up, [prefix] down
            [prefix] right, [prefix] left
        //Toggle synchronize-panes(send command to all panes)
            :setw synchronize-panes
        //Toggle between pane layouts
            [prefix] Spacebar
        //Switch to next pane
            [prefix] o
        //Show pane numbers
            [prefix] q
        //Switch/select pane by number
            [prefix] q 0 ... 9
        //Toggle pane zoom
            [prefix] z
        //Convert pane into a window
            [prefix] !
        //Resize current pane height(holding second key is optional)
            [prefix] + Up
            [prefix] Ctrl + Up 
            [prefix] + Down
            [prefix] Ctrl + Down 
        //Resize current pane width(holding second key is optional)
            [prefix] + Right
            [prefix] Ctrl + Right 
            [prefix] + Left
            [prefix] Ctrl + Left 
        //Close current pane
            [prefix] x
    
    //Copy Mode
        //use vi keys in buffer
            :setw -g mode-keys vi
    //Enter copy mode
        [prefix] [
    //Enter copy mode and scroll one page up
        [prefix] PgUp
    //Quit mode
        q
    //Go to top line, Go to bottom line
        g, G
    //Scroll up, Scroll down
        up, down
    //Move cursor left, down, up, right
        h, j, k, l
    //Move cursor forward one word at a time, Move cursor backward one word at a time
        w, b

    //Search forward, Search backward
        /, ?
    
    //Next keyword occurance, Previous keyword occurance
        n, N
    //Start selection, Clear selection, Copy selection
        Spacebar, Esc, Enter
    //Paste contents of buffer_0
        [prefix] ]
    //display buffer_0 contents
        :show-buffer
    //copy entire visible contents of pane to a buffer
        :capture-pane
    //Show all buffers
        :list-buffers
    //Show all buffers and paste selected
        :choose-buffer
    //Save buffer contents to buf.txt
        :save-buffer buf.txt
    //delete buffer_1
        delete-buffer -b 1
    
//Misc
    //Enter command mode
        [prefix] :
    //Set OPTION for all sessions
        :set -g OPTION
    //Set OPTION for all windows
        :setw -g OPTION

//Help
    //Show every session, window, pane, etc...
        tmux info
    //Show shortcuts
        [prefix] ?			
    //show key bindings
        tmux --list-keys | grep [commandName|keyName]
            