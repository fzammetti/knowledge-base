# Android
----------

## List all apps

    adb shell pm list packages

## List only enabled apps

    adb shell pm list packages -e

## List disabled apps

    adb shell pm list packages -d

## List apps with their associated APKs

    adb shell pm list packages -f

## List only system apps

    adb shell pm list packages -s

## List only third-party apps

    adb shell pm list packages -3

## Disable an app

    adb shell pm disable-user --user 0 <package_name>

## Enable an app

    adb shell pm enable <package_name>

## Download mode

Hold volume down + home + power

(note: this is not true for all phones!)

## Recovery mode

Hold volume up + home + power

(note: this is not true for all phones!)

## Procedure For Flashing Modem Only

This is for old Samsung phones only!

1.  Start ODIN.  Select PIT file and modem file in PHONE section of ODIN. Ensure only Reboot is selected.
2. Shut off phone, power on while holding down 1 on keyboard to enter download mode.
3. Click Start in ODIN.  Wait until it says PASS in green in the box up top.
4. Phone will restart.  Check baseband version in phone info to confirm.

## Procedure For Flashing An Update On A Nexus 7 2013 (Probably 2012 Too)

This can be done without wiping - see description below.

To install an OTA:

1. Download the update zip from Google's servers
2. Reboot your Nexus 7 and hold the Volume Down button while it's booting up. Once you see the fastboot menu and the word Start, press Volume Up a few times until you see Recovery and then press the Power button to enter recovery.
3. You will see an Android with a red exclamation point. Now press Volume Up+Power together, and you should see a menu appear.
4. Select the 2nd option called apply update from adb.
5. Connect the Nexus 7 to your computer with a USB cable.
6. type in: **adb sideload xxxxx.zip**
7. Reboot when it's done

ALTERNATIVELY!  Download the full factory image from here: https://developers.google.com/android/nexus/images

Then, extract it all.

Then, copy all files from **android\android-sdk\platform-tools** into the extract directory.

Then, open **flash-all.bat** and look for the line:

    fastboot -w update XXX.zip

**XXX.zip** will be whatever this version of the image is.  Remove the **-w** option and save the file. The **-w** option means wipe, so removing that will install the image without wiping anything.

Then, power down and hold volume down while pressing power.  This will get you to the fastboot screen.  Just leave it there, plug it in and then go execute the batch file.

Note that this WILL remove root, so need to re-root afterwards (easiest to use Nexus Root Toolkit).

## Procedure For Moving Any App To External SD Card

1. From command prompt: **adb shell pm get-install location** (should say **0[auto]**)
2. To change it: **adb shell pm set-install-location 2** (then do the **get-install** location command again, should say **2[external]** now
3. Move apps, then set back to 0

## Procedure to make SD card writable by third-party apps on Android 4.4+

1. Copy ROOT **/system/etc/permissions/platform.xml** to **platform.xml_original**
2. Open original file in an editor
3. Find **\<permission>** with **name="android.permission.WRITE_EXTERNAL_STORAGE"**
4. Add a new **\<group>** element with **gid="media_rw"**

   Should look like this when done:

       <permission name="android.peermission.WRITE_EXTERNAL_STORAGE">
         <group gid="sdcard_r" />
         <group gid="sdcard_rw" />
         <group gid="media_rw" />  <<---------- THIS IS WHAT IS ADDED
       </permission>

5. Reboot and you're done

## Procedure to flash just one partition on a Galaxy S5:

If tiy want to flash a single stock partition such as boot, recovery or even system without flashing the complete firmware package, this is a quick explanation of how to use the Unified Android Toolkit to do it.

### PART 1: Downloading the stock firmware image
  1. THIS CAN BE SKIPPED IF YOU ALREADY HAVE A .TAR.MD5 FILE
  2. Go to **samsung-updates.com** then click on the triangle next to "Select Device" (top left side) and scroll down to your device such as SM-G900T (for the Galaxy S5).
  3. A list of available stock firmware images will be displayed. Find the build that you need and click on the "Download" button on the right side. IMPORTANT: Since you only want to flash a single partition make sure that the build you download is the same as the build you have on your device now so that it is compatible with the rest of your partitions. If the same build isn't available or you can't access that information and don't know you want it to AT LEAST be the same version (i.e. 4.4.2).
  4. The next screen will give you the option of where to download the image from. In my opinion Rapidgator is a bit faster (for the free download) but will still take a while so you might want to go away and come back when its done.
  5. After it's finished downloading you will have a large ZIP file. Extract the contents using Winzip or 7-Zip and you should now have another large **.tar.md5** file. This is the file you would load in to Odin (as a "PDA"/"AP") if you want to flash the complete package but since you just want a single image you need to rename the file to **.tar** (delete the **.md5** from the end) and then extract just the image you need such as the **system.img.ext4** file.

### PART 2: Making the flashable TAR file
  1. So now you should have the image file you want to flash but you need to get it in to a **.tar** format that can be flashed by Odin. Copy the **.img** file to the **INPUT** folder in the Toolkit (v1.2.7 or later) and start the Toolkit. Type option '99' to load up the Basic Toolkit. In the Main Menu of the Basic Toolkit select option '8' (Create an Odin flashable tar). In the next menu choose option 2 to make a single tar file from multiple images (it doesn't matter that you only have 1 image).
  2. Make sure that ONLY the image you need to flash is displayed in the list (**.img** or **.img.ext4**) and type **yes** to start. If the file is a system image then it will take a while to convert but eventually it should say completed and you will have a **.tar** file in the OUTPUT folder. This is your flashable file.

### PART 3: Flashing the TAR file
  1. Once you are back in the Main Menu of the Basic Toolkit select the "Device Reboot Options" section and "Reboot to Download Mode and Start Odin". If you can not boot your device in to Android with ADB debug enabled (needed for this option) then just unplug your USB cable, hold Volume Down, Home and Power buttons until you get a warning screen and press Volume Down to get in to Download Mode. Then you can run **Odin3_v3.09.exe** from Toolkit folder. Now connect your USB cable and the box under **ID:COM** should be blue.
  2. Make sure "Auto Reboot" and "F. Reset Time" is ticked.
  3. Click on "PDA"/"AP" and browse to the Toolkit, OUTPUT folder. Click on the TAR file (it will start with CREATED-TAR) and then click on Open. When you are ready hit the "Start" button and the flash process should start. This may take a while depending on the image you are flashing. A progress bar will be displayed on the device and on Odin to let you know how it's going.
  4. After the flash finishes your device will reboot and if everything went fine the new image will be flashed and your device should boot without any problems.

## Flash an update without wiping phone (Galaxy S5):

* You need a stock tar.md5 file.
* Run Odin.
* Add the firmware file to PDA.  Make sure re-partition is NOT ticked!
* Reboot phone in Download Mode (press and hold Home + Power + Volume Down).
* Click the start button, sit back and wait a few minutes.
