# Getting Set Up in the LCBD

## Setting Up Compute and Storage Resources
The lab uses two integrated servers, DynoSparky for processing and Moochie for storage. The server can only be accessed via the WUSTL network, so you must use the [Medical School VPN](https://cpb-us-w2.wpmucdn.com/sites.wustl.edu/dist/5/1185/files/2018/04/vpn-Windows-MED-new.pdf) to access DynoSparky from off campus.

Moochie is mapped to `/data/perlman/moochie` on DynoSparky.

DynoSparky runs the XFCE flavor of Ubuntu (a Linux OS distribution) known as Xubuntu. Any Ubuntu software will work just fine, but we also get the added benefit of XFCE's sleek interface, and graphical interfaces.

Lab members do not have access to DynoSparky by default. Each user must be individually added, so ask Jenna if you need to be added.

There are a few options for approved users to access the server:

### Remote Desktop
#### For Mac Users
1. Download [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop-8/id715768417?mt=12) from the AppStore (either version 9 or 10).
2. Once installed, open the App and click "New" at the top to set up a new connection.
3. For "PC Name", enter "dynosparky.neuroimage.wustl.edu". Under credentials, enter your NIL username (user camachoc is displayed below as an example). Note that this is NOT your WUSTL ID, this is your NIL login.

![Mac Remote Desktop Example](../assets/setup_rd_mac.png)

4. The other settings can be as you wish. The "Connection Name" is what the connection will be listed as in the App. You may choose to not leave your password entered, in which case you'll be prompted to enter it later. You may choose to check or uncheck the boxes at the bottom that determine how the window appears on your monitor.
5. Close the Edit window. The new connection will now be listed in the main window of the app.

![Mac Remote Desktop Connection](../assets/setup_rd_mac_connect.png)

6. Double click the connection to start. The first time that you connect, the App will ask if you want to trust the server. Click yes.
7. If you did NOT enter your password as part of the connection setup, you will see an error message. Click "Ok", then you will see the screen below. Log in with your NIL username and password.

![Mac Remote Desktop NIL Credentials](../assets/setup_rd_mac_NIL.png)

8. Once you are logged in, you should see your desktop, and can proceed to the DynoSparky first-time setup documentation.

#### For Windows Users:
1. Search for the "Remote Desktop Connection" default Windows application, and open it.

![Windows Remote Desktop App](../assets/setup_rd_windows.png)

2. Once the window opens, click the "Show Options" down-arrow to expand the window. For Computer, enter `dynosparky.neuroimage.wustl.edu`. For User name, enter your NIL user name. Note that this is NOT the same as your WUSTL login.
![Windows Remote Desktop NIL Credentials](../assets/setup_rd_windows_NIL.png)
3. If you are using a personal computer or account, you can opt to check the "Allow me to save credentials" box to reduce how often you have to log in. You can also click "Save As" and save the Remote Desktop Connection to your desktop to make it quicker to connect in the future.
4. Double-click your new connection or click "connect" from within the Remote Desktop Connection App. You will be prompted to log in with your NIL credentials. The first time you log in, you will get a message asking to allow connections with the remote computer, or if you can trust the certificate. For both, click "Yes" or "Allow". You can also check the "Don't ask me about this again on this computer" box to avoid having to give permission again later the next time you log in.
5. Once you have logged in and provided permission to connect, you should see your desktop. If this is your first time connecting, you can proceed to the DynoSparky first-time setup documentation.

### Secure Shell (Terminal / Command Prompt)
Secure Shell ([SSH](https://en.wikipedia.org/wiki/Secure_Shell)) is a secure network protocol for operating services securely, even over unsecured networks. Even so, you must be connected to the [Medical School VPN](https://cpb-us-w2.wpmucdn.com/sites.wustl.edu/dist/5/1185/files/2018/04/vpn-Windows-MED-new.pdf) in order to connect.

On any operating system, you can connect to DynoSparky using SSH.

1. Ensure you are connected to the VPN
2. Open the Mac Terminal, Windows Command Prompt, or Linux Terminal

![SSH syntax](../assets/setup_ssh.png)

3. Type `ssh -Y NILUSER@dynosparky.neuroimage.wustl.edu` and hit ENTER. You should now be prompted to enter your password. Enter your password and hit enter (no *'s will appear, like when you enter a password on a website). It may take a moment to connect.

![SSH success](../assets/setup_ssh_success.png)

4. If your OS is compatible with X11 forwarding, you may also use the `-X` flag to allow ssh to display graphical programs on your local computer. This is favored over using Remote Desktop, because the computational resources required to display the program are offloaded from DynoSparky, allowing the server to remain optimized in its role for multiple simultaneous connections. E.g.,
```
ssh -X NILUSER@dynosparky.neuroimage.wustl.edu
```

## Setting Up for the First Time on DynoSparky
The first time you log in, you'll need to make some modifications. It will be useful first to review the Command Line Help and Tips section. A great deal can be accomplished through the use of even just a few Bash commands, and it *will* save you time and energy to have them at your disposal.

Each user on DynoSparky has a login-node space. This path is abbreviated for each user to `~`, and is equivalent to `/home/user/NILUSER.` This path is not necessarily your personal storage compartment, but *is* used for storing information about your DynoSparky configuration, and tools which may help you navigate the servers.

### C Shell Configuration File
`.cshrc` is a file that is executed each time you execute a new shell (i.e., each time you log in, open a new Remote Desktop session or Xterm window). [.cshrc Docs](https://www.doc.ic.ac.uk/csg-old/linux/Cshrc.html)

The "." at the start of the filename indicates that it is a *hidden* file, a little-known but vastly important convention that allows terminal-users to interact with files not typically displayed in the file explorer window.

1. Enter the following command:
```
echo /data/perlman/moochie/user_data/cshrc.txt >> ~/.cshrc
```

This will append the contents of the template cshrc file to your personal configuration, `~/.cshrc`. NOTE: it is pertinent that you use `>>` and not `>` (two carats). `>>` appends to a file, whereas `>` writes the file out from scratch.

The template cshrc file sets up the OS, as well as some PATH and environment variables which allow your user profile to recognize shared lab software packages, such as FSL and FreeSurfer.

2. After making changes to cshrc, they are only loaded once a new connection is made. Alternatively, you can force a refresh with:
```
source ~/.cshrc
```

### MATLAB Setup
The lab uses a shared MATLAB installation.

1. To access easily, you can create a "soft link" or "symbolic link" to the program location.
```
ln -s /usr/local/pkg/MATLAB/R2019b/bin/matlab ./matlab
```

2. Add your new "bin" folder to your path, so it is automatically recognized as a source of available software.
```
echo "set path = ( $path /home/usr/$USER/bin )" >> ~/.cshrc
```

3. Add the lab's shared MATLAB toolboxes to your MATLAB startup file, so they are automatically recognized by MATLAB on boot.

***TODO***
```
echo "addpath(genpath('/'))" # comment
```
