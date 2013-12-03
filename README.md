#Quick steps to sync and build Evervolv

###Getting Started

#####Install the Android SDK.

Not strictly required to build, but if you want adb / fastboot / monitor, you'll want it [see googles instructions](http://developer.android.com/sdk/index.html)

#####Install the Build Packages

See [initializing the environment](http://source.android.com/source/initializing.html) for googles detailed instructions

######For Ubuntu 12.04 64-bit:

    $ sudo apt-get install git gnupg flex bison gperf build-essential \
        zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
        libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
        libgl1-mesa-dev g++-multilib mingw32 tofrodos \
        python-markdown libxml2-utils xsltproc zlib1g-dev:i386
    $ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so

If you have dependency issues, try installing only a couple of packages at a time.

######Install Java

As of KitKat, Android cannot be built with openjdk6. You will have to install
[Oracle's JDK6](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html) manually.

Alternatively. KitKat experimentally supports openjdk7. On Ubuntu install ```openjdk-7-jdk```
and you must put ```export EXPERIMENTAL_USE_JAVA7=true``` in your bashrc in order to use it.

#####Create the Directories

######You will need to set up some directories in your build environment.

    mkdir ~/bin
    mkdir -p ~/android/system

#####Install Repo

######Enter the following to download and make executable the "repo" binary:

    $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    $ chmod a+x ~/bin/repo

Add ```export PATH=~/bin:$PATH``` to your bashrc
You may need to logout/login for path changes to take effect.

#####Download Source

######Now enter the following to initialize the repository:

    cd ~/android/system/
    repo init -u git://github.com/Evervolv/android.git -b kitkat
    # Then to start the Sync. (This is gonna take awhile)
    repo sync -f

All proprietary files are kept in the repo, no need to pull them from your device (unless adding devices).

#####Configure Build

######Now, your environment must be configured to build specifically for your device. To set up your build environment:

    . build/envsetup.sh
    lunch

And choose your correct device.

The first time you run lunch the device, vendor, and any dependencies will be automatically downloaded.

#####Building

######Next, we will build the actual ROM.
We will specify the amount of jobs you want to run (replace # with number of cpu cores, or be adventurous and max it out!)
'otapackage' creates the zip file that can be flashed through recovery

    make -j# otapackage

#####Completed!
Your compiled Evervolv rom will be located at:

    ~/android/system/out/target/product/<product>/ev_<product>-<release>.zip
