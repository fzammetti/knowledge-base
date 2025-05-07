# Windows

**NOTE: While it's true that not all do, it is easiest to assume that these all need to be done from elevated command prompts
or PowerShell prompts.**

---

* [General Windows repair procedure](#13d15f09-8de9-4e70-831d-d9d68a2604d4)
* [To reset networking](#6f6d43c3-b77c-478f-a1e2-bd418d5d2f12)
* [To reset the video driver](#71dbea84-2c1b-4d8b-a931-ac71176ff658)
* [If Windows Store downloads are getting stuck](#8c89241a-745c-46f2-a9ab-a3a71e37e295)
* [If Windows Store won’t launch, especially when trying to do wsreset](#88be2746-d665-4a3d-9c2a-b92dc489b814)
* [List all installed hotfixes](#12e8d4ea-2e35-4f79-a06b-da467481549a)
* [Fix tab key not working at Command Prompt](#b885d4f4-5bc1-4739-9509-40069f76ca11)
* [List and uninstall app](#313890a4-d814-4367-b021-12d31afc914e)
* [Show full details about all logged in users](#fbb8966b-581b-481e-81ee-4f09808870e9)
* [Reclaim ownership of any file or filder](#f60266d5-adec-4ba9-827d-4a10c6514556)
* [View all installed drivers on system](#d0c22faa-bd25-4a17-8369-3df45bea92d1)
* [List and kill tasks from command line](#8c58a11e-2fed-43e1-8dd0-d93479c5a7b5)
* [Start/stop a service from command line](#8018aa38-3b21-4c24-a6d0-9ae906445d1d)
* [Configure startup type of a service from command line](#9086dea7-9502-4029-bbea-e661d530c7c9)
* [See domain user information](#4009b548-40d0-4031-a1bf-cca8e747aa11)
* [The equivalent of touch for windows](#2b3b0a70-3e82-41e6-a148-b0b6fd54d558)
* [Download TLS cert from a server](#7e039598-7b10-4cef-a333-64ab9a1719b5)
* [Delete a driver from the command line](#481092c6-9b43-47ce-b2de-809fc34237ae)
* [Manually edit service start (for when startup type is greyed out in UI)](#ada5e549-524b-40e3-be40-af730221c27a)
* [Clear all Windows event logs](#6aecab68-b733-430f-92e5-1230fe0031ff)
* [Important Registry locations](#658259b5-57cc-4192-b6b5-7469b000d759)
* [To turn hibernation on or off](#76c0c982-f595-467c-87d8-cfa70642d1b9)
* [Disable the Windows Welcome Experience](#7bfeb95d-6447-4768-bc3f-8cfdb6615472)
* [Reboot into BIOS/UEFI](#06a70476-c3f4-439a-ac84-725d98207863)
* [Query drive SMART status (with other info)](#e9133478-5f92-4354-ba7a-c9c194a23554)
* [Problem dragging and dropping taskbar icons)](#434c33fa-eb3a-47ee-a526-0a3e51f282de)

---




<div id="13d15f09-8de9-4e70-831d-d9d68a2604d4">

## General Windows repair procedure

</div>

    sfc /scannow
    DISM /Online /Cleanup-Image /CheckHealth
    DISM /Online /Cleanup-Image /ScanHealth
    DISM /Online /Cleanup-Image /RestoreHealth
    sfc /scannow




<div id="6f6d43c3-b77c-478f-a1e2-bd418d5d2f12">

## To reset networking

</div>

    netsh winsock reset
    netsh int tcp reset
    netsh int ip reset




<div id="71dbea84-2c1b-4d8b-a931-ac71176ff658">

## To reset the video driver

</div>

    Win+Ctrl+Shift+B




<div id="8c89241a-745c-46f2-a9ab-a3a71e37e295">

## If Windows Store downloads are getting stuck

</div>

    dism /online /cleanup-image /startcomponentcleanup

  ...can also try:

    wsreset

  ...that takes a while, seems to do nothing, then opens store when complete.

  As a last resort, try this:

    Dism /online /Cleanup-Image /StartComponentCleanup
    sfc /scannow
    Dism /Online /Cleanup-Image /RestoreHealth
    sfc /scannow
    wsreset
    <reboot>




<div id="88be2746-d665-4a3d-9c2a-b92dc489b814">

## If Windows Store won’t launch, especially when trying to do wsreset

</div>

    icacls "C:\Program Files\WindowsApps" /reset /t /c /q

  ...then run **wsreset** again.




<div id="12e8d4ea-2e35-4f79-a06b-da467481549a">

## List all installed hotfixes

</div>

    wmic qfe




<div id="b885d4f4-5bc1-4739-9509-40069f76ca11">

## Fix tab key not working at Command Prompt

</div>

    reg add "hkcu\software\microsoft\command processor" /v CompletionChar /d 9 /t REG_DWORD /f
    reg add "hkcu\software\microsoft\command processor" /v PathCompletionChar /d 9 /t REG_DWORD /f

  Close and reopen Command Prompt for the change to take effect.




<div id="313890a4-d814-4367-b021-12d31afc914e">

## List and uninstall app

</div>

  * Type: **wmic**\<enter>
  * Type: **get product name**\<enter>
  * Find app to uninstall and type: **product where name="\<name_of_program>"** call uninstlal
  * Look for **ReturnValue = 0** at the end




<div id="fbb8966b-581b-481e-81ee-4f09808870e9">

## Show full details about all logged in users

</div>

    whoami /all




<div id="f60266d5-adec-4ba9-827d-4a10c6514556">

## Reclaim ownership of any file or filder

</div>

    takeown /f \<file_or_folder>

  Note: Add /r to the end to take ownership of the full contents of a folder recursively




<div id="d0c22faa-bd25-4a17-8369-3df45bea92d1">

## View all installed drivers on system

</div>

    driverquery




<div id="8c58a11e-2fed-43e1-8dd0-d93479c5a7b5">

## List and kill tasks from command line

</div>

    tasklist
    taskkill /im \<image_name>




<div id="8018aa38-3b21-4c24-a6d0-9ae906445d1d">

## Start/stop a service from command line

</div>

    sc start|stop “<service-name>”




<div id="9086dea7-9502-4029-bbea-e661d530c7c9">

## Configure startup type of a service from command line

</div>

    sc config “<service-name>” start=<auto|boot}demand|disabled}system>

Note: demand means manual




<div id="4009b548-40d0-4031-a1bf-cca8e747aa11">

## See domain user information

</div>

    net user xedd3mx /domain

Note: you can do /do instead of /domain too




<div id="2b3b0a70-3e82-41e6-a148-b0b6fd54d558">

## The equivalent of touch for windows

</div>

    copy /b <filename.ext> +,,




<div id="7e039598-7b10-4cef-a333-64ab9a1719b5">

## Download TLS cert from a server

</div>

    openssl s_client -connect <url>:<port> -starttls <service_type>

This requires OpenSSL be installed (which means you can do this from Linux just as well).  As an example, to download the cert from the BNY corporate SMTP server, the <url> would be smtpa.bnymellon.net, the <port> would be 587, and the <service_type> would be smtp.




<div id="481092c6-9b43-47ce-b2de-809fc34237ae">

## Delete a driver from the command line

</div>

This first came about when I tried to enable Core Isolation Memory Integrity on my server and Windows wouldn't let me due to some old Western Digital drivers used for an external hard drive.  This drivers were not compatible with this protection, but because they were no longer used, there was no obvious way to remove them.  So, to solve that problem:

* Open an administrative command prompt

* Execute the command:

        pnputil /enum-devices /connected

    Go through the output and make sure the driver you want to remove isn't listed for any device.

* After confirming, execute the command:

        pnputil /delete-driver <driver_published_name> /uninstall /force /reboot




<div id="ada5e549-524b-40e3-be40-af730221c27a">

## Manually edit service start (for when startup type is greyed out in UI)

</div>

Expand **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services** key in RegEdit, look for service name (as shown in
properties dialog for the service).  Edit the **Start** key value as follows:
* 2 = Automatic
* 3 = Manual
* 4 = Disabled




<div id="6aecab68-b733-430f-92e5-1230fe0031ff">

## Clear all Windows event logs

</div>

    for /F "tokens=*" %1 in ('wevtutil.exe el') DO wevtutil.exe cl "%1"




<div id="658259b5-57cc-4192-b6b5-7469b000d759">

## Important Registry locations

</div>

* **Services**: Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services (start: 0=Boot, 1=System, 2=Automatic, 3=Manual, 4=Disabled)
* **Explorer shell overlays**: Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers




<div id="76c0c982-f595-467c-87d8-cfa70642d1b9">

## To turn hibernation on or off (delets hiberfil.sys file in root of C):

</div>

    powercfg /hibernate on
    powercfg /hibernate off




<div id="7bfeb95d-6447-4768-bc3f-8cfdb6615472">

## Disable the Windows Welcome Experience

The Windows Welcome Experience shows up after major updates.  It can be disabled by disabling it in notification
settings:

* Settings->System->Notifications->Additional Settings
* Uncheck 'Show me the Windows welcome experience after updates and occasionally when I sign in to highlight what’s new and suggested'
* Uncheck 'Suggest ways to get the most out of Windows and finish setting up the device'
* Uncheck 'Offer suggestions on how I can set up my device''




<div id="06a70476-c3f4-439a-ac84-725d98207863">

## Reboot into BIOS/UEFI

Execute this command from a command prompt to restart the machine into BIOS/UEFI:

    shutdown /r /fw /t 0




<div id="e9133478-5f92-4354-ba7a-c9c194a23554">

## Query drive SMART status (with other info)

Execute this command from a command prompt:

    wmic diskdrive get status,model,serialnumber




<div id="434c33fa-eb3a-47ee-a526-0a3e51f282de">

## Problem dragging and dropping taskbar icons

On some Windows installations (but not all, and seemingly NOT depending on version), you can freely drag-and-drop taskbar icons around, for running apps, when labels are turned off.  But, sometimes, this doesn't work (it DOES if labels are turned on).  You can, however, do it via keyboard shortcuts:

    1. Press Windows+T to place focus on the task bar
    2. Press Arrow Left or Arrow Right to select the icon you want to move
    3. Use Alt+Shift+Arrow Left or Alt+Shift+Arrow Right to move it spot by spot

Annoying for sure, but at least it's a working solution to what should NOT be a problem in the first place (great job Microsoft! /s)
