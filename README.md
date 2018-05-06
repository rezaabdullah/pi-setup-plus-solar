# Raspberry Pi Headless Setup Instructions
**Following methods are verified in Windows 10**<br/>
*Complexities of the methods are in ascending order.*

#### **Method 1**
**Requirements:**
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++
* [Bonjour Application](https://support.apple.com/downloads/bonjour)<br/>


**Steps**<br/>
1. [Download Raspberry Pi OS Image](https://www.raspberrypi.org/downloads/raspbian/)
2. Extract the image file
3. Burn image file to SD card with [Etcher](https://etcher.io/) or [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
4. Once done, navigate to `boot` folder of the SD card
5. Create an empty file with Notepad or similar text editor and name it `ssh` **(NO file extension i.e. file type)** *ssh file can also be downloaded from here.*
6. Mount SD Card on Pi
7. Connect ethernet cable to pi and laptop
8. Turn on the pi
9. Open [putty](https://www.putty.org/)<br/>
   a. Connection Type: SSH<br/>
   b. Host Name: raspberrypi.local
10. Login as (User Name): `pi`
11. Password: `raspberry`
12. At Terminal:<br/>
   a. Type `sudo raspi-config`<br/>
   b. Choose `5 Interfacing Options`<br/>
   c. Choose `P3 VNC`<br/>
   d. Use arrow key to enable it
13. [Install VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)
14. At Address Bar enter: `raspberrypi.local`. Use info from *10 & 11* when prompted to enter username and password.

#### **Method 2**
**Requirements:**
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++


1. Follow *1-5* from Method 1
6. Find and open `cmdline.txt` with text editor. The file can be found inside `boot` folder
7. Edit the file. Add `ip=192.168.1.200::192.168.1.1:255.255.255.0:rpi:eth0:off` at the end of the existing content of the file.<br/>
   *The file can also be found here. Replace the original file with the file from this repository.*
8. Disconnect computer from the internet
9. Connect ethernet cable to Pi and computer
10. Turn on the Pi
11. Open `Network & Internet Settings` from Control Panel
12. Click on the network name next to `Connections:`
13. Under *Activity* select *Properties*
14. Select *Internet Protocol Version 4 (TCP/IPv4)
15. Select *Properties*
16. Select *Use the following IP address*<br/>
    a. IP address: 192.168.1.201<br/>
    b. Subnet mask: 255.255.255.0<br/>
    c. Default gateway: 192.168.1.1
17. Connect Pi to the router via ethernet cable
18. Turn on the pi
19. Obtain Pi's IP address from the router
20. Follow *9-14* from Method 1

#### **Method 3**
**Requirements:**
* Internet connection
* Router
* Putty
* Ethernet Cable
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++
* [Bonjour Application](https://support.apple.com/downloads/bonjour)<br/>


1. Follow *1-6* from Method 1
7. Connect Pi to the router via ethernet cable
8. Turn on the pi
9. Obtain Pi's IP address from the router
10. Follow *9-14* from Method 1

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
