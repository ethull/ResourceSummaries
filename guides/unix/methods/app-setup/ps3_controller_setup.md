# shell
## ps3/xbox controller on ps2 
    sudo apt-get install xboxdrv
    //plug in controller and press home
    //see if it worked
        grep -i /var/log/syslog Sony
            usb 4-2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
            usb 4-2: Product: PLAYSTATION(R)3 Controller
            usb 4-2: Manufacturer: Sony
            sony 0003:054C:0268.0008: Fixing up Sony Sixaxis report descriptor
            input: Sony PLAYSTATION(R)3 Controller as /devices/pci0000:00/0000:00:13.2/usb4/4-2/4-2:1.0/input/input18
            sony 0003:054C:0268.0008: input,hiddev0,hidraw0: USB HID v1.11 Joystick [Sony PLAYSTATION(R)3 Controller] on usb-0000:00:13.2-2/input0

        dmesg | grep sony
            sony 0003:054C:0268.0003: can't set operational mode
            sony: probe of 0003:054C:0268.0003 failed with error -38

## load driver
    sudo xboxdrv --detach-kernel-driver --led 2

# steam setup
    //open steam in big picture mode
    //change settings for specific games controller (unless supported)

## skyrim controller setup
    action -> keyboard -> xbox/ps3
    forward       w       up
    back          s       down
    left          a       left
    right         d       right
    left hand     M2      left trigger
    right hand    M1      right trigger
    activate      E       y/triangle
    ready/sheathe R       b/circle
    Char Menu     TAB
    toggle POV    F
    Jump          space   a/x   
    sprint        alt     left stick continous push  
    shout/power   z       right bumper
    sneak         ctrl    right stick push
    run           shift    
    toggle          
     always run   caps      
    auto-move     c
    favarites     q
    quicksave     F5      select
    quickload     F9
    wait          t
    journal       j
    system tab    esc    start
    quick 
     inventory    l 
    quick magic   p
    quick stats   ? /
    quick map     M
