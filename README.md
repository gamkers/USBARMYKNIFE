Project Link:

https://github.com/i-am-shodan/USBArmyKnife

Plug in your device, holding down any boot button that may exist

Visit https://esp.huhn.me/

Click Connect, select your ESP32 device and select Connect again

Add the files and offsets seen in the image below. You need to pay special attention to the offsets for each file. Delete any other entries. image

Click Program

In the Output window you should see lots of text scrolling, the device flashing then the following image

![alt text](image.png "Title Text")



Project Instalation:

Preparing your SD card

The USB Army Knife may not run correctly with large SD cards or those with newer filesystems. We recommend using one with at most one 32GB FAT32 partition for maximum compatibility. Smaller capacities can also be used. This article on partitioning an SD card can help with the process of doing this on Windows.

Note on first run, if an SD card cannot be found with a supported filesystem the device will offer to format it for you. If you use this option the filesystem created on the SD card may not work under Windows. As such it is advised to create a suitable SD card off device.

Preparing your script file

Be aware that your script file should have Windows style (CRLF) line endings. If your script is terminating on empty lines convert your script using unix2dos.

Installing firmware

Flashing with Visual Studio Code and PlatformIO

Install Visual Studio Code

Install PlatformIO Visual Studio Code extension

Clone the repository:

git clone https://github.com/i-am-shodan/USBArmyKnife.git
Now you've cloned the repo you need to pull down the submodules. Run this command in the directory you just cloned to. If you don't do this you will get errors related to ESP32Maurauder

git submodule update --init 
Open VS Code and then Open the USB Army Knife directory

Click the PlatformIO icon (Alien icon)

(Remove the dongle if it was inserted) Press and hold the hardware button, insert the device, wait 1s and release the button.

You should now seen a new COM port/serial device attached to your machine
In the menu expand the device you want to flash.

For the T-Dongle S3 you should expand 'LILYGO-T-Dongle-S3'
For any generic ESP32-S2 you should expand 'Generic-ESP32-S2'
Other devices we’ve added support for will appear in the list. You’ll need to only expand the device you have.
It may take a few seconds to populate the build menu after you've selected your device
Press 'Upload'

Only if your device does NOT have an SD card.

Edit the files for the flash filesystem, these are stored in the 'data' directory.
Expand the Platform folder in the build menu from the previous step.
Click 'Upload Filesystem Image'.
When the upload has finished successfully, remove the dongle and insert the micro SD card if you have one

Updating the codebase to the latest version

If you want to update an existing install you need to:

Use git pull to grab the latest changes to this repository
Run git submodule update --recursive to make sure all the submodules are up to date
Click 'Full Clean' in the PlatformIO build menu. At this point all your code and dependencies will be up to date and you can continue with the build steps above.
Flashing with a web browser

You can use the web browser method to flash ESP32 based devices. This has been tested on the Lilygo Dongle but on other devices YMMV. If you are unable to flash your device this way please use the VS Code method.

This method is only suitable for devices WITH an SD card slot. This is because this flashing method does not allow you to create or manage a flash based file system. Proceeding with an unsupported device will cause the device to likely crash on start, unless you have previously created this file system

Download the latest code for your device

Visit this page
Select the first/top most entry (this is the latest build)
In the Artifacts section click download next to the model of device you have (only ESP32 based devices are currently supported)
Extract the downloaded files
Download boot_app0.bin. Make sure you press the 'Download raw file' button.

Plug in your device, holding down any boot button that may exist

Visit https://esp.huhn.me/

Click Connect, select your ESP32 device and select Connect again

Add the files and offsets seen in the image below. You need to pay special attention to the offsets for each file. Delete any other entries. image

Click Program

In the Output window you should see lots of text scrolling, the device flashing then the following image

Unplug and re-insert the device without any buttons being held down

Blowing eFuses to increase USB compatibility (optional)

When an ESP32-S3 is plugged in, the bootloader initially sets the device as a USB Serial adapter with a Espressif VID/PID. The USB Army Knife then boots and reconfigures USB. This process might cause issue with some USB stacks, you'll know this includes you in a USB keyboard doesn't appear when the device is plugged in. People have reported that blowing the USB_PHY_SEL eFuse increase USB compatibility but disabling the boot loader from becoming a USB device.

To blow your USB_PHY_SEL eFuse you'll need to use espefuse.py tool, it cannot be done online. The following instructions will work if you have a Visual Studio code environment set up.

In Visual Studio Code click the PIO (Alien icon) in the left hand bar. Click 'New Terminal' in the quick access menu. In the terminal type making sure to use your device's COM port.

pip install cryptography
pip install ecdsa
pip install bitstring
pip install reedsolo
pio pkg exec --package "platformio/tool-esptoolpy" -- espefuse.py --port COM3 summary
You should see a summary of your current eFuse state. Now run:

pio pkg exec --package "platformio/tool-esptoolpy" -- espefuse.py --port COM3 burn_efuse USB_PHY_SEL 1
