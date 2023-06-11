# Apple

---

* [Dealing with those annoying .DS_Store files](#ed6a37ad-3071-4e47-b69e-16089b50fead)

---




<div id="ed6a37ad-3071-4e47-b69e-16089b50fead">

## Dealing with those annoying .DS_Store files

</div>

To configure a Mac OS X user account so that **.DS_Store** files are not created when interacting with a remote file server using Finder (note that this will affect the user's interactions with SMB/CIFS, AFP, NFS, and WebDAV servers):

1. Open Terminal

2. Execute:

        defaults write com.apple.desktopservices DSDontWriteNetworkStores true

3. Either restart the computer or log out and back in to the user account

4. If you want to prevent **.DS_Store** file creation for other users on the same computer, log in to each user account and perform the steps above, or distribute a copy of the newly modified **com.apple.desktopservices.plist** file to the **~/Library/Preferences** folder of other user accounts
