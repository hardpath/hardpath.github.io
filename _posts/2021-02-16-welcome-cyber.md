---
title: "Basic Configuration of a Raspberry Pi"
date: 2021-02-16 12:34:50 +0000
categories: cyber
tn_ref: TN2155001
tn_rev: "01"
---
## 1 INTRODUCTION

This article describes the steps to set up a Raspberry Pi with a basic configuration which can then be used for different purposes.
This procedure will detail the steps for:

- [Create a Bootable Media](#3-create-a-bootable-media)
- [Basic Configuration](#4-basic-configuration)
- [Network Access](#5-network-access)
- [Miscellaneous](#6-miscellaneous)

Throughout this article, the following convention will be used:

Commands:

$ command

File contents:

    4 This is line 4 of the file
    5 This is line 5

The line number might not match the actual line number in the file.

## 2 REQUIREMENTS
The following will be required for this procedure:

- [Raspberry Pi OS Lite](https://www.raspberrypi.org/software/operating-systems/) (Raspberry Pi OS Lite)
- [Rufus](https://rufus.ie/) (portable)
- microSD Card (nimimum: 4 GB; recommended: 16 GB)
- Monitor with HDMI connection
- USB keyboard
- Computer running Windows and with a microSD card slot.

## 3 CREATE A BOOTABLE MEDIA

It is assumed that the Rufus software was already downloaded, and the Operating System image file was extracted from the downloaded zip file.

Insert the microSD card in the computer’s slot.

Start Rufus by double-clicking on the executable.

Ensure that the Device Combobox has the microSD card. 

![rufus](/assets/images/posts/2021-02-16-welcome-cyber/img0301.png)

Press SELECT and then browse for the img file of the Operating System.

Press START. A warning message will be shown. Press OK to proceed.

Once finished, the Progress Bar will show READY.

Press CLOSE.

On Windows Explorer, right-click on the drive associated with the microSD Card and select Eject.

Remove the microSD card from the slot. The microSD card is ready to be used on the Raspberry Pi.

## 4 BASIC CONFIGURATION

This section describes the steps for different configurations.

### 4.1 Root Password

By default, the root has no password assigned. For security reasons, it is suggested to leave it this way.

However, the root’s password can be changed using the following command:

    $ sudo passwd

### 4.2 New User

Not allowing root to log in will require someone to find the user's username with superuser rights. For this to be effective, a new user should be created (the ‘pi’ user is broadly known).

The following commands should be used to create a new user. For convenience, the username will be ‘username’ which should be replaced by the actual username.

    $ sudo useradd -m username -c “f_name l_name”
    $ sudo passwd username

If a user already exists with superuser rights, then the following commands can be skipped. The following commands will add the new user created above to the super users, a.k.a. sudoers.

    $ sudo usermod -aG sudo username

It is now required to delete the default user (‘pi’) and its home directory.

    $ sudo userdel pi
    $ rm -fr /home/pi

Details of the newly created user can be checked by reading the contents of the passwd file.

    $ cat /etc/passwd

### 4.3 Hostname

The hostname can be configured in two ways.

One option is to use the raspi-config (Raspberry PI Configuration Tool).

    $ sudo rasp-config

After, select 1 System Options and them S4 Hostname. Once the new hostname is entered, and after selection Finish to exit the configuration tool, it will be prompted to reboot, which should be accepted.

The other option is to edit the /etc/hostname file directly.

    $ sudo vi /etc/hostname

The current hostname can be verified using the following command:

    $ hostname

Using the options mentioned above will only change the /etc/hostname file. However, the hostname is also used in the file /etc/dhcpcd.conf which should be updated accordingly. This is done by editing the line 8 of the file /etc/dhcpcd.conf.

    7 # Inform the DHCP server of our hostname for DDNS.
    8 myhostname

Further down in the file, the hostname must be added to the interface eth0 section. This is so that the hostname could be sent when the DHCP request is issued. After edited, the file should look like this:

    44 interface eth0
    45 hostname myhostname

## 5 NETWORK ACCESS

This section covers different network configurations for both wired and wireless connections. For each, both static IP address and dynamic IP address (DHCP) scenarios will be considered.

The configurations outlined below will involve editing the configuration file ``/etc/dhcpcd.conf``. For that reason, it is convenient to keep a safe copy of it. For that, use the following command:

    $ sudo cp /etc/dhcpcd.conf /etc/dhcpcd.conf.sav

To apply changes made to the configuration file, reboot  using the following command:

    $ sudo reboot

### 5.1 Wired

#### 5.1.1 Dynamic IP Address

By default, the IP address is dynamically assigned by the DHCP server. Nothing needs changing the /etc/dhcpcd.conf file.

If a static address is in place, as described in the next section, to enable dynamic addressing, just comment those lines in the configuration file.

#### 5.1.2	Static IP Address

The following lines must exist (uncommented) in the configuration file to define a static IP address

    46 static ip_address=192.168.0.100/24
    47 static routers=192.168.0.1
    48 static domain_name_servers=192.168.0.1

In the excerpt above, the IP address is defined in the first line alongside the netmask 255.255.255.0 .
The gateway is defined in the second line. The name server in the last line.

#### 5.2 Wireless

To connect to a wireless network, run raspi-config.  Select 1 System Options and then S1 Wireless LAN. The first time, it will be asked to select the country.

After, enter the SSID and, when prompted, the password.

Exit raspi-config with rebooting. After rebooting, wireless access to the network should be active.

To turn Wi-Fi off and on, use the following commands respectively:

    $ sudo ifconfig wlan0 down
    $ sudo ifconfig wlan0 up

To remove the SSID from the know networks, edit the file /etc/wpa_supplicant/wpa_supplicant.conf and remove the lines shown below.

    5 network={
    6     ssid="SSID"
    7     psk="password"
    8 }

After rebooting, the wlan0 interface will be available but not connected.

## 6 MISCELLANEOUS

### 6.1	Update Linux Distro

To update the Linux, run the following commands

    $ sudo apt update
    $ sudo apt -y upgrade
    $ sudo apt autoremove

### 6.2 SSH Access

SSH is disabled by default.

To enable an SSH connection, the SSH Server has to be enabled. Before enabling the SSH sever ensure that no default users and passwords are in place, and existing users have strong passwords.

The simplest way to enable the SSH Server is to use raspi-config as follows:

    $ sudo rasp-config

After, select 3 Interface Options and then P2 SSH. Then select Yes.

The SSH daemon configuration is held in the file /etc/ssh/sshd_config. In this file, for example, we can disable the display of the last login timestamp by setting the following in the file:

    94 PrintLastLog no

### 6.3 Change Welcome Message

A welcome message (example shown below)is shown to a user after logging in via a remote SSH session. This message is managed by the Message Of The Day daemon. 

    1: Linux hostname 5.10.11-v7+ #1399 SMP Thu Jan 28 12:06:05 GMT 2021 armv7l
    2: 
    3: The programs included with the Debian GNU/Linux system are free software;
    4: the exact distribution terms for each program are described in the
    5: individual files in /usr/share/doc/*/copyright.
    6: 
    6: Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    8: permitted by applicable law.
    9: Last login: Fri Feb 12 14:57:35 2021 from 192.168.4.99

The motd daemon utilises to the file /etc/motd and the scripts in /etc/update-motd.d.

The message shown above is comprised of three entries:

- Line 1: Output generated by a script in /etc/update-motd.d
- Lines 2 to 8: Contents of the file /etc/motd
- Line 9: This line is not related to the Welcome Message, and it is generated by the SSH daemon (see Section SSH Access)

The scripts in /etc/update-motd.d are execute in alphabetical order.

### 6.4 Splash Screen

The package rpd-plym-splash is required to configure a custom splash screen. To install the package, execute the following command

    $ sudo apt install rpd-plym-splash

Then run raspi-config.  Select 1 System Options and then S7 Slash Screen. It will be prompted if you like to show the splash screen at boot. Select Yes.

With no further configuration, the default Raspberry welcome splash screen shown below will be shown.

The image has 1024 x 768 pixels at 72 ppi.
