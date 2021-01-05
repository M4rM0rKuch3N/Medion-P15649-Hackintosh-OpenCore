# Medion-P15649-Hackintosh-OpenCore

**Specs of this laptop:**
* Intel Core i5-10210U Comet Lake
* Intel UHD Graphics 630
* 8 GB DDR4 RAM (I upgraded mine to 16GB RAM)
* I2C Touchpad (FTCS1000)
* Intel AX201 Wifi/Bluetooth
* Realtek ALC233 Audio Controller

**Working**
* Display
* Integrated Graphics
* Keyboard
* Trackpad
* Wifi
* Bluetooth
* Battery Readout
* Webcam
* Microphone
* Audio
* iMessage/Facetime/Handoff
* Audio combojack headphone port
* Sleep/wake
* Keyboard backlight

**Not working (yet)**
* Brightness / Volume Hotkeys

**Instructions**

1. Follow dortania guide https://dortania.github.io/OpenCore-Install-Guide/ until "Adding the Base OpenCore Files" where you take my EFI instead. 
2. Boot into MacOS Installer from USB, delete your drive to install MacOS and let the installer do its work. (You can follow the dortania guide under the point "Installation Process" for that.
3. Setup MacOS after finishing the installation
4. Download MountEFI: https://github.com/corpnewt/MountEFI or OpenCore Configurator: https://mackie100projects.altervista.org/opencore-configurator/ (I'd personally use OpenCore Configurator because it will help us edit the config.plist easier and will take the job to mount your EFI partition)
5. With MountEFI or OpenCore configurator open, mount your Install USB's EFI partition and copy its EFI folder to your desktop.
6. Eject the USB's EFI and mount your MacOS drive's EFI now
7. Copy the EFI folder on the desktop to the EFI partition you mounted from MacOS.
8. Now you should have everything functioning as it should.

**Fixing Headphone combojack**

I fixed my combojack using this tutorial: https://elitemacx86.com/threads/audio-distortion-when-using-headphones-on-laptops-clover-opencore.185/

1. Download CodecCommander from this link: https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/
2. Download the Jack Fix: https://elitemacx86.com/attachments/jack-fix-zip.698/
3. Boot into your recovery partition (you can select that atthe opencore picker)
4. Open the terminal and write "csrutil authenticated-root disable" and press enter
5. Write "csrutil disable" to gethe SIP disabled and click enter
6. Boot back into MacOS
7. Open terminal and write "diskutil list"
8. This will give you a list of your drives and partitions, look for your MacOS drive, for me it was "disk1s5"
9. Open Finder and go to its settings, go to side-bar and tick the option with your username and the house icon left to it. Then go back to finder and into this directory. 
10. In terminal write mkdir ~/nonroot and hit enter, this will make our folder where we will temporarly mount our root directory.
11. Write "sudo mount -o nobrowse -t apfs /dev/"YOUR DISK NAME" ~/nonroot", replace it with your disk name, in my example disk1s5 and hit enter
12. You will see, that nonroot was replaced with our MacOS partition in finder, open it.
13. To copy our files we need to enable seeing hidden files, do this by writing "defaults write com.apple.Finder AppleShowAllFiles true" in terminal and hitting enter, then "killall Finder" and enter again.
14. You should now see all the hidden folders and files in our temporary root directory.
15. Go to usr folder, then to bin and copy and paste the two files, hda-verb from our downloaded CodecCommander and jack fix into that directory.
16. Open settings, go to users&groups and to login items and add the jack fix file using the plus icon in there.
17. Lastly, write in terminal "sudo bless --folder ~/nonroot/System/Library/CoreServices --bootefi --create-snapshot" and hit enter
18. Reboot and check if combojack is working without distortion or clackling sound.
