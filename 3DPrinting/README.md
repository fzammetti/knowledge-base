# 3D Printing

---

* [Restore Connectivity After Firmware Update](#781ea3c3-3083-474d-bc5f-a9339d3e0e1f)

---




<div id="781ea3c3-3083-474d-bc5f-a9339d3e0e1f">

## Restore Connectivity After Firmware Update

</div>

On my Ender-3 V3, I have Moonraker and Mainsail installed.  After performing a firmware update on the print, usually I can't connect (192.168.xxx.xxx:4409).  To resolve this, telnet into the printer, and use the helper_script.sh to remove and then re-install Moonraker.  The script can be found here: https://github.com/Guilouz/Creality-Helper-Script
