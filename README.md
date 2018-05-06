# Raspberry Pi Setup Instructions
**Following methods are verified in windows**<br/>
*Complexities of the methods are in ascending order.* 

##### Method 1 (Easiest): Require Internet Connection, Putty, Text Editor and Bonjour Application
1. [Download Raspberry Pi OS Image](https://www.raspberrypi.org/downloads/raspbian/)
2. Extract the image file
3. Burn image file to SD card with [Etcher](https://etcher.io/) or [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
4. Once done, navigate to `boot` folder of the SD card
5. Create an empty file with Notepad or similar text editor and name it `ssh`. **The file MUST NOT contain file extension i.e. file type.** *The file can also be found here.*
3. Mount SD Card on Pi
4. Open [putty](https://www.putty.org/)
   a. Connection Type: SSH
   b. Host Name: raspberrypi.local
5. login as: `pi`
6. password: `raspberry`
7.  At Terminal:
    a. Type `sudo raspi-config`
    b. Choose `5 Interfacing Options`
    c. Choose `P3 VNC`
8. [Install VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)
9. At Address Bar enter: `raspberrypi.local`

##### Method 2: Headless Setup: Require Internet Connection, Putty, Text Editor and Ethernet Cable
1. Steps 1-3 are same from Method 1  
4. Connect Raspberry Pi to router via ethernet cable to obtain Pi's IP Address from Router  
5. Follow Steps 4-8 from Method 1 where Host Name is Pi's IP Address  
10. At Address Bar enter Pi's IP Address  

##### Method 3: Headless Setup: Putty, Text Editor and Ethernet Cable
1. Steps 1-2 are same from Method 1  
3. Enter WiFi Details:  
    a. Create a file on the `boot` directory called `wpa_supplicant.conf`  
    b. Inside the file enter the following details:  
    ```
    network={
        ssid="YOUR_NETWORK_NAME"
        psk="YOUR_PASSWORD"
        key_mgmt=WPA-PSK
    }
    ```
4. Setup Static IP Address:  
    a. On the `boot` directory open `cmdline.txt`
    b. Add `ip = x.x.x.x` after `rootwait`
5. Mount SD Card on Pi  
6. Follow Steps 4-8 from Method 1 where Host Name is Pi's IP Address  

##### Setting Up Static IP Address
1. DHCPCD Method:  
    a. At the Terminal enter: `sudo nano /etc/dhcpcd.conf`  
    b. Edit file: At the bottom of the file add following lines where x.x.x.x denotes IP address and router and domain_name_servers are same:  
    ```
    interface eth0
    static ip_address=x.x.x.x/24
    static routers=x.x.x.x
    static domain_name_servers=x.x.x.x

    interface wlan0
    static ip_address=x.x.x.x/24
    static routers=x.x.x.x
    static domain_name_servers=x.x.x.x
    ```
##### Swapping serial port
By default port serial port is disabled and replaced by default Bluetooth module. 
Therefore, ttyAMA0 is assigned to Bluetooth while ttyS0 is assigned to serial port.  
[Raspberry Pi UARTS Documentation](https://www.raspberrypi.org/documentation/configuration/uart.md)  
[UART Setup Walkthrough](https://spellfoundry.com/2016/05/29/configuring-gpio-serial-port-raspbian-jessie-including-pi-3/)  
1. Edit `/boot/config.txt` by adding `enable_uart=1` at the bottom  
2. At the terminal enter `ls -l /dev` to find out serial port aliases  
3. To stop and disable `/dev/ttyS0`, at Terminal enter  
    ```
    sudo systemctl stop serial-getty@ttyS0.service
    sudo systemctl disable serial-getty@ttyS0.service
    ```
4. Edit `/boot/cmdline.txt` file by removing `console=serial0, 115200`  
5. Edit `/boot/config.txt` by adding `pi3-disable-bt` and `dtoverlay=pi3-miniuart-bt` to disable bluetooth  
6. Reboot  
7. At terminal enter `ls -l /dev` to check whether serial0 is assigned to ttyAMA0
8. Alternatively enter `sudo dmesg` to look for UART


##### Install Teamviewer
Installer can be downloaded from Teamviewer website
1. `sudo wget https://download.teamviewer.com/download/linux/teamviewer-host_armhf.deb`  
2. Navigate to download folder and enter at the terminal `sudo dpkg -i teamviewer-host_armhf.deb`

##### Install NodeJS
1. Create a new directory from terminal: `sudo mkdir <directoryName>`
2. Go to the new directory
2. From browser go to [NodeJS](https://nodejs.org/en/download/)  
3. Hover cursor over the distribution version to get the link which is shown at the bottom-right corner of the browser
    a. For RPi3 get ARMv7  
    b. For RPi Zero get ARMv6  
4. At terminal enter distribution version link `sudo wget <distributionVersionLink>`. For example, to install version 8.9.4 for RPi Zero, at terminal type the following commands:  
    ```
    sudo wget https://nodejs.org/v8.9.4/node-v8.9.4-linux-armv6l.tar.xz
    sudo tar -xvf node-v8.9.4-linux-armv6l.tar.xz
    cd node-v8.9.4-linux-armv6l
    sudo cp -R * /usr/local/
    ```
##### Darkflow
1. Open Anaconda Prompt  
2. Navigate to `Anaconda3/Darkflow`  
3. `python flow --model cfg/yolo.cfg --load bin/yolo.weights`
