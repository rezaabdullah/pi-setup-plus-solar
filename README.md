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
4. Once done, navigate to ***boot*** folder of the SD card
5. Create an empty file with Notepad or similar text editor and name it ***ssh* (NO file extension i.e. file type)** *ssh file can also be downloaded from here.*
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
* Text Editor i.e. Notepad/Atom/VS Code/Notepad++<br/>


**Steps**<br/>
1. Follow *1-5* from Method 1
6. Find and open ***cmdline.txt*** with text editor. The file can be found inside ***boot*** folder
7. Edit the file. Add `ip=192.168.1.200::192.168.1.1:255.255.255.0:rpi:eth0:off` at the end of the existing content of the file.<br/>
   *The file can also be found here. Replace the original file with the file from this repository.*
8. Disconnect computer from the internet
9. Connect ethernet cable to Pi and computer
10. Turn on the Pi
11. Open ***Network & Internet Settings*** from Control Panel
12. Click on the network name next to ***Connections:***<br/>
![Netowrk and Sharing Center](images/Connections.png)
13. Under ***Activity* select *Properties***<br/>
![Activity](images/Activity.png)
14. Select ***Internet Protocol Version 4 (TCP/IPv4)***
15. Select ***Properties***<br/>
![TCP/IPv4](images/IPv4.png)
16. Select ***Use the following IP address***<br/>
    a. IP address: `192.168.1.201`<br/>
    b. Subnet mask: `255.255.255.0`<br/>
    c. Default gateway: `192.168.1.1`
    ![TCP/IPv4](images/IPAddress.png)
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

**Steps**<br/>
1. Follow *1-6* from Method 1
7. Connect Pi to the router via ethernet cable
8. Turn on the pi
9. Obtain Pi's IP address from the router
10. Follow *9-14* from Method 1