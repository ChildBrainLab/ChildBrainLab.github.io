Moochie is the storage component of the lab's computational resources managed through CSG. Access will vary depending on your location:

# Off Campus
Make sure you are connected to the VPN before proceeding! Once you are follow the steps below...

# On Campus
On a lab PC (5th or 6th floor of 4444), once you are logged in with your NIL account:

1. Open File Explorer
2. Right click "This PC" on the sidebar
3. Click "Map network drive"
4. For "Folder" enter: "\\10.20.145.33\moochie"
5. Make sure "reconnect at sign-in" is checked
6. Click "Finish"
7. Login using the the username 'neuroimage\nil-username' and your nil password

Moochie will now display as a mounted network location on your lab PC. You will have to repeat this setup process on any new PCs you use.

## On Mac
1. Open Finder
2. Press command + K or click Go -> Connect to server on menu bar
3. Enter "smb:\\10.20.145.33\moochie" and click "Connect"
4. For "Name" enter your NIL username
5. For "Password" enter your NIL password
6. Click "Connect"

To disconnect, click the eject button next to the server address on the Finder sidebar.

## On Linux
```
sudo sshfs -o allow_other NILUSER@dynosparky.neuroimage.wustl.edu:/data/perlman/moochie /path/to/mount/to
```
