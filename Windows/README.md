# Windows
----------

## General Windows repair procedure:

    sfc /scannow
    DISM /Online /Cleanup-Image /CheckHealth
    DISM /Online /Cleanup-Image /ScanHealth
    DISM /Online /Cleanup-Image /RestoreHealth
    sfc /scannow

## To reset networking:

    netsh winsock reset
    netsh int tcp reset
    netsh int ip reset

## To reset the video driver:

    Win+Ctrl+Shift+B

## If Windows Store downloads are getting stuck, execute this from an admin command prompt:

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

## If Windows Store won’t launch, especially when trying to do wsreset, try this:

    icacls "C:\Program Files\WindowsApps" /reset /t /c /q

  ...then run **wsreset** again.

## List all installed hotfixes:

    wmic qfe

## Fix tab key not working at Command Prompt:

    reg add "hkcu\software\microsoft\command processor" /v CompletionChar /d 9 /t REG_DWORD /f
    reg add "hkcu\software\microsoft\command processor" /v PathCompletionChar /d 9 /t REG_DWORD /f

  Close and reopen Command Prompt for the change to take effect.
