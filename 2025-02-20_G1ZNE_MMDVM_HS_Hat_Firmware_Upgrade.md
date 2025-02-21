# Updating your MMDVM Firmware

## Richie Jarvis - G1ZNE - 2025-02-20

1. We need to use the commandline to carry out this process.  We need the devices IP address.
    - Find your WPSD machine's IP address from the http://wpsd.local/

        ![alt-text](./WPSD_IP_Address.jpg)   

1. Login to your WPSD machine via SSH

    - Download and Install PuTTY from here: https://www.putty.org/
    - Enter the IP address where shown in the picture and click "Open" as marked
    
        ![alt-text](./PuTTY_Login.jpg)
    - Click "Accept" the first time you connect to accept the host key and store it for future connections
       
1. Update your Raspbian OS

    - It is worth keeping your Raspbian (the OS that WPSD uses) updated regularly.  You can re-run the commandline given periodically to do so.
    
    - It will take a while.  Go make a coffee/tea/beverage of your choice.  It will connect to the internet and download everything needed, then install it.  When it returns to the commandline prompt `pi-star@wpsd:~ $` it is done.
    
      <span style="color:red">
      **Very IMPORTANT: Ignore the message "The following packages have been kept back:"**
      
      **If you attempt to fix it, you may well break your WSPD installation.**     
      </span>
    
    - Copy and run this commandline - the output is long and boring...      
        
        `sudo apt update ; sudo apt upgrade -y ; sudo apt autoremove -y`
   

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
      
      - My MMDVM currently has v1.5.2 loaded - in this example I am updating to v1.6.1
    
        `MMDVM_HS_Hat-v1.5.2 20201108 14.7456MHz ADF7021`

1. Make sure your MMDVM settings listed in the output from the `wpsd-detectmodem` command above match the settings in the WPSD Configuration screen here: http://wpsd.local/admin/configure.php
    
    - Output from `wpsd-detectmodem` 
      
      `Port: /dev/ttyAMA0 (GPIO) Baud: 115200`
    
    - Make sure the WPSD Webconfig screen reflects this information in these fields:
    
        ![alt-text](./WPSD_MMDVM_Config_Screen.jpg)   

1. Check the modem type against the list here: https://manual.wpsd.radio/maintenance_items/#how-to-update-modem-firmware
    - From the `wpsd-detectmodem` the type is listed as `MMDVM_HS_Hat-v1.5.2 20201108 14.7456MHz ADF7021`
    - This means my MMDVM is classed as `MMDVM_HS_Hat (14.7456MHz TCXO) GPIO` for the purposes of the upgrade, so I use the parameter `hs_hat` after the upgrade command.
         
    - Copy and run this commandline if you have the same MMDVM board as myself.  Substitute the correct text if different on the WPSD website:

      `sudo wpsd-modemupgrade hs_hat`

    - The output you will see will be similar to this.  It will take a couple of minutes to complete the update.

        ```
            _      _____  _______
           | | /| / / _ \/ __/ _ \
           | |/ |/ / ___/\ \/ // /
           |__/|__/_/  /___/____/
        Modem Firmware Update Utility

            [i] Latest firmware version: 1.6.1

        Press any key to flash 'hs_hat' firmware version 1.6.1 to this modem, or 'q' to abort...

        • Checking 'hs_hat' firmware version 1.6.1...
            [✓] Complete.

        • Preparing to flash 'hs_hat' modem with firmware version (1.6.1)...
            [✓] Complete! Modem firmware flash successful!
                Modem reinitialized.

        [i] Note: You will need to refresh your dashboard to reflect the updated firmware version.
        ```

1.  To complete the update, reboot the device.  

      - Either from the WPSD dashboard, or from the commandline by running this command:

        ```
        sudo reboot
        ```
    
      - You will see the following output.  
    
        ```
        pi-star@wpsd:~ $ sudo reboot

        Broadcast message from root@wpsd on pts/1 (Fri 2025-02-21 16:06:56 GMT):

        The system will reboot now!
        ```
      - Wait a couple of minutes and you should be able to login to http://wpsd.local and see the Dashboard.  The modem version displayed should now be the latest available.
      
        ![alt-text](./WPSD_Dashboard_Upgraded.jpg)   
      
  
**Congratulations!  Your modem should now be updated and working once the device has powered up again**