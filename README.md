# Voron-video-splash-screen
a tutorial to install the video in the splash screen of a klipper printer with raspberry such as voron, vzbot etc..

![](/gif/blue.gif)

thanks to VzBoT and NordFPV for the starting guide.

We have 2 video: 1.mp4 and 2.mp4. 1.mp4 is the real video that we see in the boot sequence, the 2.mp4 it's a black video that serves to block klipperscreen which otherwise when started interrupts the playback of 1.mp4

ATTENTION!!:
for RPI 5 the file config.txt and cmdline.txt are in the folder /boot/ and not /etc/


What do you need?
- SSH terminal like MobaXterm
- a raspberry PI already configured and running klipper screen
- a little patience..

1)Installing VLC on Raspberry Pi
To install VLC on our Raspberry Pi, we first ensure that the system is up to date before starting the installation.
Open SSH program and run these commands:

```
sudo apt update
```
```
sudo apt upgrade
```
Then, to install VLC, we use the following command:

```
sudo apt install vlc
```

2)Remove the color test/rainbow screen
```
sudo nano /etc/config.txt
```
add at the end of the file 
```
disable_splash=1
```

3)Copying video file

copy 1.mp4 and 2.mp4 into /home/pi/ with the folder explorer of MobaXterm

4)Add script for video display
```
sudo nano /etc/systemd/system/splash.service
```
paste:

```                                                                                      
[Unit]
Description=Splash screen
DefaultDependencies=no
After=local-fs.target

[Service]
User=pi
ExecStart=/bin/bash -c "DISPLAY=:0 /usr/bin/cvlc -f --quiet --no-video-title --play-and-exit /home/pi/1.mp4" &
StandardInput=tty
StandardOutput=tty

[Install]
WantedBy=sysinit.target
```

then:
```
sudo nano /etc/rc.local
```
and add this line before the exit 0:
```
su - pi -c "sleep 2 | DISPLAY=:0 /usr/bin/cvlc -f --quiet --no-video-title --play-and-exit /home/pi/2.mp4"
```
5)Activate service
```
sudo systemctl enable splash
```
6)OPTIONAL
to remove the raspberry logo, the initial checklist and the cursor at the top before the video boot:
```
sudo nano /boot/cmdline.txt
```
add this command at the end of the line after a space:
```
 logo.nologo consoleblank=0 loglevel=1 quiet vt.global_cursor_default=0
```
 - logo.nologo - Remove the Raspberry logo.
 - consoleblank=0 loglevel=1 quiet - Disable the commands and various bits of the kernel
 - vt.global_cursor_default=0- Removes the blinking cursor before the image loads.

7)Restart
```
sudo reboot
```



that's all folks
