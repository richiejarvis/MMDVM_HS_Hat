# Updating your MMDVM Firmware

## Richie Jarvis - G1ZNE - 2025-02-20
       
1. Update your Raspbian OS

    - It is worth keeping your Raspbian (the OS that WPSD uses) updated regularly.  You can re-run the commandline given periodically to do so.
    
    - It will take a while.  Go make a coffee/tea/beverage of your choice.  It will connect to the internet and download everything needed, then install it.  When it returns to the commandline prompt `pi-star@wpsd:~ $` it is done.
    
      <span style="color:red">
      **Very IMPORTANT: Ignore the message "The following packages have been kept back:"**
      
      **If you attempt to fix it, you may well break your WSPD installation.**     
      </span>
    
    - Copy and run this commandline - the output is long and boring...      
        
        ```
        sudo apt update ; sudo apt upgrade -y ; sudo apt autoremove -y
        ```    

1. Check what MMDVM Hat you are using and that the serial port and baudrate are correct.

  - Copy and run this commandline:

    `sudo wpsd-detectmodem`

  - The output you will see will be similar to this:

    ```
    pi-star@wpsd:~ $ sudo wpsd-detectmodem
    Detected MMDVM_HS Port: /dev/ttyAMA0 (GPIO) Baud: 115200 Protocol: Unknown
    Modem Data: MMDVM_HS_Hat-v1.5.2 20201108 14.7456MHz ADF7021 FW by CA6JAU GitID #89daa2000600056590000094E545047
    ```
  - Here are the important pieces of info you will need to upgrade your MMDVM Hat firmware
    - Detected HAT Type:
    
      `Detected MMDVM_HS`
    
    
    - The Hardware type
    
      `ADF7021`
      
    - My MMDVM currently has v1.5.2 loaded - in this example I am updating to v1.6.2
    
      `MMDVM_HS_Hat-v1.5.2`

2. Make sure your MMDVM settings listed in the output from the `wpsd-detectmodem` command above match the settings in the WPSD Configuration screen here: http://wpsd.local/admin/configure.php
    
    - Output from `wpsd-detectmodem` 
      
      `Port: /dev/ttyAMA0 (GPIO) Baud: 115200`
    
    - Make sure the WPSD Webconfig screen reflects this information in these fields:
    
        ![alt-text](./WPSD_MMDVM_Config_Screen.jpg)     
       
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
