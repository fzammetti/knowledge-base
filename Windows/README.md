# Windows
----------

## General Windows repair procedure

    sfc /scannow
    DISM /Online /Cleanup-Image /CheckHealth
    DISM /Online /Cleanup-Image /ScanHealth
    DISM /Online /Cleanup-Image /RestoreHealth
    sfc /scannow

## To reset networking

    netsh winsock reset
    netsh int tcp reset
    netsh int ip reset

## To reset the video driver

    Win+Ctrl+Shift+B

## If Windows Store downloads are getting stuck, execute this from an admin command prompt

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

## If Windows Store won’t launch, especially when trying to do wsreset, try this

    icacls "C:\Program Files\WindowsApps" /reset /t /c /q

  ...then run **wsreset** again.

## List all installed hotfixes

    wmic qfe

## Fix tab key not working at Command Prompt

    reg add "hkcu\software\microsoft\command processor" /v CompletionChar /d 9 /t REG_DWORD /f
    reg add "hkcu\software\microsoft\command processor" /v PathCompletionChar /d 9 /t REG_DWORD /f

  Close and reopen Command Prompt for the change to take effect.

## List and uninstall app (from administrator command prompt)

  * Type: **wmic**\<enter>
  * Type: **get product name**\<enter>
  * Find app to uninstall and type: **product where name="\<name_of_program>"** call uninstlal
  * Look for **ReturnValue = 0** at the end

## Show full details about all logged in users (from administrator command prompt)

    whoami /all

## Reclaim ownership of any file or filder (from administrator command prompt)

    takeown /f \<file_or_folder>

  Note: Add /r to the end to take ownership of the full contents of a folder recursively

## View all installed drivers on system (from administrator command prompt)

    driverquery

## List and kill tasks from command

    tasklist
    taskkill /im \<image_name>

## Start/stop a service from command line

    sc start|stop “<service-name>”

Note: this must be done from BNYM elevated command prompt

## Configure startup type of a service from command line

    sc config “<service-name>” start=<auto|boot}demand|disabled}system>

Note: demand means manual

## See domain user information

    net user xedd3mx /domain

Note: you can do /do instead of /domain too

## The equivalent of touch for windows

    copy /b <filename.ext> +,,

## Download TLS cert from a server

    openssl s_client -connect <url>:<port> -starttls <service_type>

This requires OpenSSL be installed (which means you can do this from Linux just as well).  As an example, to download the cert from the BNY corporate SMTP server, the <url> would be smtpa.bnymellon.net, the <port> would be 587, and the <service_type> would be smtp.
