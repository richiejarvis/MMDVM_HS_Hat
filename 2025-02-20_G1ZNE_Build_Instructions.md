# Building MMDVM_HS_Hat Firmware v1.6.2

## Richie Jarvis - G1ZNE - 2025-02-20
1. Make sure you are using the generic MMDVM_HS_Hat:

  - Copy and run this commandline:

    `sudo wpsd-detectmodem`

  - The output you will see will be similar to this:

    ```
    pi-star@wpsd:~ $ sudo wpsd-detectmodem
    Detected MMDVM_HS Port: /dev/ttyAMA0 (GPIO) Baud: 115200 Protocol: Unknown
    Modem Data: MMDVM_HS_Hat-v1.5.2 20201108 14.7456MHz ADF7021 FW by CA6JAU GitID #89daa2000600056590000094E545047
    ```

1. The bits to check:

    - Detected HAT Type:
    
      `Detected MMDVM_HS`
    
    - Port settings - these should match your settings in WPSD Configuration screen
    
      `Port: /dev/ttyAMA0 (GPIO) Baud: 115200`
    - The Hardware type
    
      `ADF7021`
    - My MMDVM currently has v1.5.2 loaded - in this example I am updating to v1.6.2
    
      `MMDVM_HS_Hat-v1.5.2`
      
1. Install the build requirements 
    - Note: Versions/exact output will be different!  Don't worry about it.
    - Copy and run this commandline:

      ```
      sudo apt-get update
      sudo apt-get install gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib libnewlib-arm-none-eabi
      ```
        
1. Update your Raspbian install

      _**Note:** Versions and exact output will be different!  Don't worry about it._
      
      <span style="color:red">
      
      **Very IMPORTANT: Ignore the message "The following packages have been kept back:"**
      
      **If you attempt to fix it, you may well break your WSPD installation.**
      
      </span>
    
    - Copy and run this commandline:      
        
        ```
        sudo apt update ; sudo apt upgrade -y ; sudo apt autoremove -y
        ```
        
    - Output will be similar to this:
      
      ```
      pi-star@wpsd:~ $ sudo apt update ; sudo apt upgrade -y ; sudo apt autoremove -y
      Get:1 http://archive.raspberrypi.com/debian bookworm InRelease [39.3 kB]
      Get:2 http://raspbian.raspberrypi.com/raspbian bookworm InRelease [15.0 kB]
      Get:3 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf Packages [14.5 MB]
      Get:4 http://archive.raspberrypi.com/debian bookworm/main armhf Packages [562 kB]
      Get:5 http://archive.raspberrypi.com/debian bookworm/main arm64 Packages [533 kB]
      Fetched 15.7 MB in 18s (875 kB/s)
      Reading package lists... Done
      Reading package lists... Done
      Building dependency tree... Done
      Reading state information... Done
      Calculating upgrade... Done
      The following package was automatically installed and is no longer required:
        libcamera0.3
      Use 'sudo apt autoremove' to remove it.
      The following NEW packages will be installed:
        libcamera0.4
      The following packages have been kept back:
        initramfs-tools initramfs-tools-core linux-headers-rpi-v6 linux-headers-rpi-v7 linux-headers-rpi-v7l linux-image-rpi-v6
        linux-image-rpi-v7 linux-image-rpi-v7l linux-image-rpi-v8:arm64 linux-libc-dev raspberrypi-net-mods raspberrypi-sys-mods
        raspi-config raspi-firmware rpi-eeprom
      The following packages will be upgraded:
        git git-man libcamera-apps-lite libcamera-ipa libgnutls30 libpisp-common libpisp1 libtasn1-6 openssh-client openssh-server
        openssh-sftp-server rpicam-apps-lite ssh
      13 upgraded, 1 newly installed, 0 to remove and 15 not upgraded.
      Need to get 13.3 MB of archives.
      After this operation, 10.8 MB of additional disk space will be used.
      Get:1 http://archive.raspberrypi.com/debian bookworm/main armhf libpisp1 armhf 1.1.0-1 [290 kB]
      Get:3 http://archive.raspberrypi.com/debian bookworm/main armhf libpisp-common all 1.1.0-1 [4,908 B]
      Get:4 http://archive.raspberrypi.com/debian bookworm/main armhf libcamera-ipa armhf 0.4.0+rpt20250213-1 [838 kB]
      Get:5 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf git armhf 1:2.39.5-0+deb12u2 [6,158 kB]
      Get:7 http://archive.raspberrypi.com/debian bookworm/main armhf libcamera0.4 armhf 0.4.0+rpt20250213-1 [651 kB]
      Get:10 http://archive.raspberrypi.com/debian bookworm/main armhf rpicam-apps-lite armhf 1.6.0-2 [475 kB]
      Get:12 http://archive.raspberrypi.com/debian bookworm/main armhf libcamera-apps-lite all 1.6.0-2 [3,584 B]
      Get:6 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf openssh-sftp-server armhf 1:9.2p1-2+deb12u5 [55.7 kB]
      Get:8 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf openssh-server armhf 1:9.2p1-2+deb12u5 [378 kB]
      Get:9 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf openssh-client armhf 1:9.2p1-2+deb12u5 [821 kB]
      Get:2 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf git-man all 1:2.39.5-0+deb12u2 [2,053 kB]
      Get:11 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libtasn1-6 armhf 4.19.0-2+deb12u1 [42.6 kB]
      Get:13 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libgnutls30 armhf 3.7.9-2+deb12u4 [1,307 kB]
      Get:14 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf ssh all 1:9.2p1-2+deb12u5 [174 kB]
      Fetched 13.3 MB in 8s (1,633 kB/s)
      apt-listchanges: Reading changelogs...
      Preconfiguring packages ...
      (Reading database ... 67172 files and directories currently installed.)
      Preparing to unpack .../0-git-man_1%3a2.39.5-0+deb12u2_all.deb ...
      Unpacking git-man (1:2.39.5-0+deb12u2) over (1:2.39.5-0+deb12u1) ...
      Preparing to unpack .../1-git_1%3a2.39.5-0+deb12u2_armhf.deb ...
      Unpacking git (1:2.39.5-0+deb12u2) over (1:2.39.5-0+deb12u1) ...
      Preparing to unpack .../2-openssh-sftp-server_1%3a9.2p1-2+deb12u5_armhf.deb ...
      Unpacking openssh-sftp-server (1:9.2p1-2+deb12u5) over (1:9.2p1-2+deb12u4) ...
      Preparing to unpack .../3-openssh-server_1%3a9.2p1-2+deb12u5_armhf.deb ...
      Unpacking openssh-server (1:9.2p1-2+deb12u5) over (1:9.2p1-2+deb12u4) ...
      Preparing to unpack .../4-openssh-client_1%3a9.2p1-2+deb12u5_armhf.deb ...
      Unpacking openssh-client (1:9.2p1-2+deb12u5) over (1:9.2p1-2+deb12u4) ...
      Preparing to unpack .../5-libtasn1-6_4.19.0-2+deb12u1_armhf.deb ...
      Unpacking libtasn1-6:armhf (4.19.0-2+deb12u1) over (4.19.0-2) ...
      Setting up libtasn1-6:armhf (4.19.0-2+deb12u1) ...
      (Reading database ... 67172 files and directories currently installed.)
      Preparing to unpack .../libgnutls30_3.7.9-2+deb12u4_armhf.deb ...
      Unpacking libgnutls30:armhf (3.7.9-2+deb12u4) over (3.7.9-2+deb12u3) ...
      Setting up libgnutls30:armhf (3.7.9-2+deb12u4) ...
      (Reading database ... 67172 files and directories currently installed.)
      Preparing to unpack .../0-libpisp1_1.1.0-1_armhf.deb ...
      Unpacking libpisp1:armhf (1.1.0-1) over (1.0.7-1) ...
      Preparing to unpack .../1-libpisp-common_1.1.0-1_all.deb ...
      Unpacking libpisp-common (1.1.0-1) over (1.0.7-1) ...
      Preparing to unpack .../2-libcamera-ipa_0.4.0+rpt20250213-1_armhf.deb ...
      Unpacking libcamera-ipa:armhf (0.4.0+rpt20250213-1) over (0.3.2+rpt20241119-1) ...
      Selecting previously unselected package libcamera0.4:armhf.
      Preparing to unpack .../3-libcamera0.4_0.4.0+rpt20250213-1_armhf.deb ...
      Unpacking libcamera0.4:armhf (0.4.0+rpt20250213-1) ...
      Preparing to unpack .../4-rpicam-apps-lite_1.6.0-2_armhf.deb ...
      Unpacking rpicam-apps-lite (1.6.0-2) over (1.5.3-1) ...
      Preparing to unpack .../5-libcamera-apps-lite_1.6.0-2_all.deb ...
      Unpacking libcamera-apps-lite (1.6.0-2) over (1.5.3-1) ...
      Preparing to unpack .../6-ssh_1%3a9.2p1-2+deb12u5_all.deb ...
      Unpacking ssh (1:9.2p1-2+deb12u5) over (1:9.2p1-2+deb12u4) ...
      Setting up openssh-client (1:9.2p1-2+deb12u5) ...
      Setting up libpisp-common (1.1.0-1) ...
      Setting up git-man (1:2.39.5-0+deb12u2) ...
      Setting up libpisp1:armhf (1.1.0-1) ...
      Setting up openssh-sftp-server (1:9.2p1-2+deb12u5) ...
      Setting up openssh-server (1:9.2p1-2+deb12u5) ...
      rescue-ssh.target is a disabled or a static unit not running, not starting it.
      ssh.socket is a disabled or a static unit not running, not starting it.
      Setting up git (1:2.39.5-0+deb12u2) ...
      Setting up ssh (1:9.2p1-2+deb12u5) ...
      Setting up libcamera-ipa:armhf (0.4.0+rpt20250213-1) ...
      Setting up libcamera0.4:armhf (0.4.0+rpt20250213-1) ...
      Setting up rpicam-apps-lite (1.6.0-2) ...
      Setting up libcamera-apps-lite (1.6.0-2) ...
      Processing triggers for libc-bin (2.36-9+rpt2+deb12u9) ...
      Processing triggers for man-db (2.11.2-2) ...
      Reading package lists... Done
      Building dependency tree... Done
      Reading state information... Done
      The following packages will be REMOVED:
        libcamera0.3
      0 upgraded, 0 newly installed, 1 to remove and 15 not upgraded.
      After this operation, 2,108 kB disk space will be freed.
      (Reading database ... 67162 files and directories currently installed.)
      Removing libcamera0.3:armhf (0.3.2+rpt20241119-1) ...
      Processing triggers for libc-bin (2.36-9+rpt2+deb12u9) ...
      ```
      
1. Install the build tools necessary for the firmware build:

    - Copy and run this commandline:
        
      ```
      sudo apt install -y gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib libnewlib-arm-none-eabi
      ```
      _Note1: *This will take a while - go make a cup of tea/coffee/beverage of your choice.*_\
      _Note2: Versions/exact output will be different!  Don't worry about it._
    - Output will be similar to this:    

        ```
        pi-star@wpsd:~ $ sudo apt install -y gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib libnewlib-arm-none-eabi
        Reading package lists... Done
        Building dependency tree... Done
        Reading state information... Done
        Note, selecting 'gdb-multiarch' instead of 'gdb-arm-none-eabi'
        The following additional packages will be installed:
          binutils-arm-none-eabi libnewlib-dev libstdc++-arm-none-eabi-dev
        Suggested packages:
          libnewlib-doc
        The following NEW packages will be installed:
          binutils-arm-none-eabi gcc-arm-none-eabi gdb-multiarch libnewlib-arm-none-eabi libnewlib-dev libstdc++-arm-none-eabi-dev
          libstdc++-arm-none-eabi-newlib
        0 upgraded, 7 newly installed, 0 to remove and 15 not upgraded.
        Need to get 408 MB of archives.
        After this operation, 2,469 MB of additional disk space will be used.
        Get:5 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libnewlib-arm-none-eabi all 3.3.0-1.3+deb12u1 [45.4 MB]
        Get:1 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf binutils-arm-none-eabi armhf 2.39-8+rpi1+18 [2,173 kB]
        Get:4 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libnewlib-dev all 3.3.0-1.3+deb12u1 [257 kB]
        Get:2 http://www.mirrorservice.org/sites/archive.raspbian.org/raspbian bookworm/main armhf gcc-arm-none-eabi armhf 15:12.2.rel1-1 [42.4 MB]
        Get:3 http://www.mirrorservice.org/sites/archive.raspbian.org/raspbian bookworm/main armhf gdb-multiarch armhf 13.1-3 [3,553 kB]
        Get:6 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libstdc++-arm-none-eabi-dev all 15:12.2.rel1-1+23 [1,217 kB]
        Get:7 http://raspbian.raspberrypi.com/raspbian bookworm/main armhf libstdc++-arm-none-eabi-newlib all 15:12.2.rel1-1+23 [313 MB]
        Fetched 408 MB in 3min 18s (2,060 kB/s)
        Selecting previously unselected package binutils-arm-none-eabi.
        (Reading database ... 67155 files and directories currently installed.)
        Preparing to unpack .../0-binutils-arm-none-eabi_2.39-8+rpi1+18_armhf.deb ...
        Unpacking binutils-arm-none-eabi (2.39-8+rpi1+18) ...
        Selecting previously unselected package gcc-arm-none-eabi.
        Preparing to unpack .../1-gcc-arm-none-eabi_15%3a12.2.rel1-1_armhf.deb ...
        Unpacking gcc-arm-none-eabi (15:12.2.rel1-1) ...
        Selecting previously unselected package gdb-multiarch.
        Preparing to unpack .../2-gdb-multiarch_13.1-3_armhf.deb ...
        Unpacking gdb-multiarch (13.1-3) ...
        Selecting previously unselected package libnewlib-dev.
        Preparing to unpack .../3-libnewlib-dev_3.3.0-1.3+deb12u1_all.deb ...
        Unpacking libnewlib-dev (3.3.0-1.3+deb12u1) ...
        Selecting previously unselected package libnewlib-arm-none-eabi.
        Preparing to unpack .../4-libnewlib-arm-none-eabi_3.3.0-1.3+deb12u1_all.deb ...
        Unpacking libnewlib-arm-none-eabi (3.3.0-1.3+deb12u1) ...
        Selecting previously unselected package libstdc++-arm-none-eabi-dev.
        Preparing to unpack .../5-libstdc++-arm-none-eabi-dev_15%3a12.2.rel1-1+23_all.deb ...
        Unpacking libstdc++-arm-none-eabi-dev (15:12.2.rel1-1+23) ...
        Selecting previously unselected package libstdc++-arm-none-eabi-newlib.
        Preparing to unpack .../6-libstdc++-arm-none-eabi-newlib_15%3a12.2.rel1-1+23_all.deb ...
        Unpacking libstdc++-arm-none-eabi-newlib (15:12.2.rel1-1+23) ...
        Setting up binutils-arm-none-eabi (2.39-8+rpi1+18) ...
        Setting up gcc-arm-none-eabi (15:12.2.rel1-1) ...
        Setting up libnewlib-dev (3.3.0-1.3+deb12u1) ...
        Setting up gdb-multiarch (13.1-3) ...
        Setting up libnewlib-arm-none-eabi (3.3.0-1.3+deb12u1) ...
        Setting up libstdc++-arm-none-eabi-dev (15:12.2.rel1-1+23) ...
        Setting up libstdc++-arm-none-eabi-newlib (15:12.2.rel1-1+23) ...
        Processing triggers for man-db (2.11.2-2) ...
        Processing triggers for libc-bin (2.36-9+rpt2+deb12u9) ...
        ```
1. We now need to download the firmware source.

    - Copy and run this commandline to get a copy of the firmware source repository ready to build
    
      ```
      cd ~
      mkdir git
      cd git
      ls
      git clone https://github.com/richiejarvis/MMDVM_HS_Hat.git
      cd ~/git/MMDVM_HS_Hat
      git submodule init
      git submodule update
      ```
        
    - You will see output similar to this:
        ```
        pi-star@wpsd:~ $ cd ~
        pi-star@wpsd:~ $ mkdir git
        pi-star@wpsd:~ $ cd git
        pi-star@wpsd:~/git $ git clone https://github.com/richiejarvis/MMDVM_HS_Hat.git
        Cloning into 'MMDVM_HS_Hat'...
        remote: Enumerating objects: 783, done.
        remote: Counting objects: 100% (39/39), done.
        remote: Compressing objects: 100% (28/28), done.
        remote: Total 783 (delta 14), reused 28 (delta 11), pack-reused 744 (from 1)
        Receiving objects: 100% (783/783), 42.29 MiB | 1.94 MiB/s, done.
        Resolving deltas: 100% (464/464), done.
        Updating files: 100% (66/66), done.
        pi-star@wpsd:~ $ cd git/MMDVM_HS_Hat/
        ```
1. Prepare the repository for Building
    - Copy and run this commandline:
      ```
      git submodule init; git submodule update
      ```
    - The output should look similar to this:
      ```
      pi-star@wpsd:~/git/MMDVM_HS_Hat $ git submodule init; git submodule update
      Submodule 'libraries/pkl' (https://github.com/esden/pretty-kicad-libs.git) registered for path 'libraries/pkl'
      Cloning into '/home/pi-star/git/MMDVM_HS_Hat/libraries/pkl'...
      Submodule path 'libraries/pkl': checked out '37a5a862b61527ad2c535c8b9a14c9720e6f6afb'
      ```
1. 