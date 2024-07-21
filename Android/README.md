# Android

---

* [List all apps](#9fe4e549-21f1-4031-8b42-f06da693f28d)
* [List only enabled apps](#a8da0152-849c-4e44-887a-23d18c170042)
* [List disabled apps](#a1fa31bf-89ff-4ba8-b12d-4eadc0d2330f)
* [List apps with their associated APKs](#aaaf46f9-7198-44aa-b686-ec570acb3fc0)
* [List only system apps](#f1bedaf1-a23c-4768-84e7-395f5538418c)
* [List only third-party apps](#a3b68e7a-3f29-4d56-9fdf-58c788204d05)
* [Disable an app](#ea89ef2c-1879-4dc8-a634-83fb266a3778)
* [Enable an app](#867d12b3-de8f-45a9-af6d-92e208c9f3cc)
* [Download mode](#590f2310-4b90-44ca-ad86-8c5c049d7e6f)
* [Recovery mode](#6beba2b5-cde8-406a-846c-0da657583154)
* [Procedure For Flashing Modem Only](#f1b7ae96-bbe0-44aa-b552-a12ea69e625a)
* [Procedure For Flashing An Update On A Nexus 7 2013 (Probably 2012 Too)](#ebde2f05-dc72-446f-adf3-302682f15355)
* [Procedure For Moving Any App To External SD Card](#dbba5d32-a96f-4e85-b883-555f753412f9)
* [Procedure to make SD card writable by third-party apps on Android 4.4+](#f813ccb9-62d7-460e-bb8a-241b989a80ac)
* [Procedure to flash just one partition on a Galaxy S5](#19edd8c5-c983-4bfb-80d6-235060c220d4)
* [Flash an update without wiping phone (Galaxy S5)](#6378f5c6-e1ea-401a-b293-4701057cb880)
* [Get around an SDK version block to install an app](#45d88660-9570-4de4-bd2d-d377cd828e4c)

---




<div id="9fe4e549-21f1-4031-8b42-f06da693f28d">

## List all apps

</div>

    adb shell pm list packages




<div id="a8da0152-849c-4e44-887a-23d18c170042">

## List only enabled apps

</div>

    adb shell pm list packages -e




<div id="a1fa31bf-89ff-4ba8-b12d-4eadc0d2330f">

## List disabled apps

</div>

    adb shell pm list packages -d




<div id="aaaf46f9-7198-44aa-b686-ec570acb3fc0">

## List apps with their associated APKs

</div>

    adb shell pm list packages -f




<div id="f1bedaf1-a23c-4768-84e7-395f5538418c">

## List only system apps

</div>

    adb shell pm list packages -s




<div id="a3b68e7a-3f29-4d56-9fdf-58c788204d05">

## List only third-party apps

</div>

    adb shell pm list packages -3




<div id="ea89ef2c-1879-4dc8-a634-83fb266a3778">

## Disable an app

</div>

    adb shell pm disable-user --user 0 <package_name>




<div id="867d12b3-de8f-45a9-af6d-92e208c9f3cc">

## Enable an app

</div>

    adb shell pm enable <package_name>




<div id="590f2310-4b90-44ca-ad86-8c5c049d7e6f">

## Download mode

</div>

Hold volume down + home + power

(note: this is not true for all phones!)




<div id="6beba2b5-cde8-406a-846c-0da657583154">

## Recovery mode

</div>

Hold volume up + home + power

(note: this is not true for all phones!)

Alternate method:
1. Start with the phone ON.
2. Hold Volume Down + Power, choose Power Off when the shutdown menu appears.
3. Wait a few seconds after the screen goes blank to ensure it's off.
4. Hold Volume Up and Power until the Android logo appears, then release.  Recovery mode should appear in a few seconds.

Specifically for Fold 4 recovery mode:
  1. Start with the phone OFF.
  2. Press and hold the side key (power) and the volume up button.
  3. Release the side key once the phone turns on but keep volume up button pressed until menu appears.

Specifically for Fold 4 download mode:
  1. Start with the phone OFF.
  2. Press and hold the side key (power) and BOTH volume buttons.
  3. Release the side key once the phone turns on but keep BOTH volume buttons pressed until menu appears.




<div id="f1b7ae96-bbe0-44aa-b552-a12ea69e625a">

## Procedure For Flashing Modem Only

</div>

This is for old Samsung phones only!

1.  Start ODIN.  Select PIT file and modem file in PHONE section of ODIN. Ensure only Reboot is selected.
2. Shut off phone, power on while holding down 1 on keyboard to enter download mode.
3. Click Start in ODIN.  Wait until it says PASS in green in the box up top.
4. Phone will restart.  Check baseband version in phone info to confirm.




<div id="ebde2f05-dc72-446f-adf3-302682f15355">

## Procedure For Flashing An Update On A Nexus 7 2013 (Probably 2012 Too)

</div>

This can be done without wiping - see description below.

To install an OTA:

1. Download the update zip from Google's servers
2. Reboot your Nexus 7 and hold the Volume Down button while it's booting up. Once you see the fastboot menu and the word Start, press Volume Up a few times until you see Recovery and then press the Power button to enter recovery.
3. You will see an Android with a red exclamation point. Now press Volume Up+Power together, and you should see a menu appear.
4. Select the 2nd option called apply update from adb.
5. Connect the Nexus 7 to your computer with a USB cable.
6. type in: **adb sideload <filename>.zip**
7. Reboot when it's done

ALTERNATIVELY!  Download the full factory image from here: https://developers.google.com/android/nexus/images

Then, extract it all.

Then, copy all files from **android\android-sdk\platform-tools** into the extract directory.

Then, open **flash-all.bat** and look for the line:

    fastboot -w update <filename>.zip

**<filename>.zip** will be whatever this version of the image is.  Remove the **-w** option and save the file. The **-w** option means wipe, so removing that will install the image without wiping anything.

Then, power down and hold volume down while pressing power.  This will get you to the fastboot screen.  Just leave it there, plug it in and then go execute the batch file.

Note that this WILL remove root, so need to re-root afterwards (easiest to use Nexus Root Toolkit).




<div id="dbba5d32-a96f-4e85-b883-555f753412f9">

## Procedure For Moving Any App To External SD Card

</div>

1. From command prompt: **adb shell pm get-install location** (should say **0[auto]**)
2. To change it: **adb shell pm set-install-location 2** (then do the **get-install** location command again, should say **2[external]** now
3. Move apps, then set back to 0




<div id="f813ccb9-62d7-460e-bb8a-241b989a80ac">

## Procedure to make SD card writable by third-party apps on Android 4.4+

</div>

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




<div id="19edd8c5-c983-4bfb-80d6-235060c220d4">

## Procedure to flash just one partition on a Galaxy S5

</div>

If you want to flash a single stock partition such as boot, recovery or even system without flashing the complete firmware package, this is a quick explanation of how to use the Unified Android Toolkit to do it.

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




<div id="6378f5c6-e1ea-401a-b293-4701057cb880">

## Flash an update without wiping phone (Galaxy S5)

</div>

* You need a stock tar.md5 file.
* Run Odin.
* Add the firmware file to PDA.  Make sure re-partition is NOT ticked!
* Reboot phone in Download Mode (press and hold Home + Power + Volume Down).
* Click the start button, sit back and wait a few minutes.




<div id="45d88660-9570-4de4-bd2d-d377cd828e4c">

## Get around an SDK version block to install an app

</div>

On an Android device you can connect to ADB and sideload an application with:

    adb install name_of_package.apk

This may result in this error:

    INSTALL_FAILED_DEPRECATED_SDK_VERSION: App package must target at least SDK version 23, but found 22

To overcome this error and install the application anyway, use:

    adb install --bypass-low-target-sdk-block name_of_package.apk
