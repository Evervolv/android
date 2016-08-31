#Quick steps to sync and build Evervolv

###Getting Started

#####Install the Android SDK.

Not strictly required to build, but if you want adb / fastboot / monitor, you'll want it [see googles instructions](http://developer.android.com/sdk/index.html)

#####Install the Build Packages

See [initializing the environment](http://source.android.com/source/initializing.html) for googles detailed instructions

######Required Packages

#####Ubuntu 14.04+ (64-bit)
    $ sudo apt-get install git-core gnupg flex bison gperf build-essential \
      zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
      lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache \
      libgl1-mesa-dev libxml2-utils xsltproc unzip

If you have dependency issues, try installing only a couple of packages at a time.

######Install Java

Nougat requires Java 8, OpenJDK is recommended

######For Ubuntu 15.04+
    $ sudo apt-get update
    $ sudo apt-get install openjdk-8-jdk

######For Ubuntu 14.04

See [initializing the environment](http://source.android.com/source/initializing.html) for googles detailed instructions

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
    repo init -u git://github.com/Evervolv/android.git -b ng-7.0
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
