# flutter
## setup
    //predownload plaform specifc dev binaries (isntead of at runtime)
        flutter precache
    //look for dependancies needed to complete setup
        flutter doctor
    //disable reporting
        flutter config --no-analytics
    //android
        install android studio, go through setup wizard
        own phone
            android dev options and usb debugging
            install google usb driver (win10) and plug in
            verify
                flutter devices
        setup emulator
            Android Studio > Tools > Android > AVD Manager > 
    Create Virtual Device definition >  Next.
            >1 system images > Next 
            Under Emulated Performance > select Hardware - GLES 2.0 (hardware acceleration)
            Android Virtual Device Manager > run (toolbar)                 
    //maintanence
        //swap flutter channel, stable, beta, dev, master:,
            flutter channel {}
        flutter upgrade
        //swap to specific version
            flutter version v1.9.1+hotfix.3
# install/upgrade packages 
    (from pubspec.yaml file)
    //https://pub.dev/
        flutter pub get
        flutter pub upgrade
# create app    
    flutter create myapp
    //if have listed device, run app
    flutter run
        //reload, save changes
            r 
## tools
    https://dart.dev/guides/language/language-tour
    https://dart.dev/guides/language/effective-dart/design#types
    https://dart.dev/tools/sdk
    https://flutter.dev/docs

    https://github.com/thosakwe/vim-flutter
    https://github.com/dart-lang/dart-vim-plugin

    https://github.com/natebosch/vim-lsc
    https://github.com/natebosch/vim-lsc-dart

    https://github.com/prabirshrestha/vim-lsp

