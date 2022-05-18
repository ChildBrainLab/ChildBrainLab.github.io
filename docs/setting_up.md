# Getting Set Up in the LCBD

## Setting Up LCBD Compute and Storage Resources
The lab uses two integrated servers, DynoSparky for processing and Moochie for storage. The server can only be accessed via the WUSTL network, so you must use the [Medical School VPN](https://cpb-us-w2.wpmucdn.com/sites.wustl.edu/dist/5/1185/files/2018/04/vpn-Windows-MED-new.pdf) to access DynoSparky from off campus.

Moochie is mapped to `/data/perlman/moochie` on DynoSparky.

DynoSparky runs the XFCE flavor of Ubuntu (a Linux OS distribution) known as Xubuntu. Any Ubuntu software will work just fine, but we also get the added benefit of XFCE's sleek interface, and graphical interfaces.

Lab members do not have access to DynoSparky by default. Each user must be individually added, so ask Amanda if you need to be added.

All of our computational resources (experiment and office PCs, DynoSparky, Moochie, VPN) are managed by the [Computer Support Group](https://research.wustl.edu/core-facilities/computer-support-group/), a sub-group of the [Neuroimaging Labs Research Center](https://www.mir.wustl.edu/research/research-centers/neuroimaging-labs-research-center-nil-rc/). CSG can be contacted via email at [nil-systems@npg.wustl.edu](nil-systems@npg.wustl.edu), but understand that they attend to nearly 70 different Linux systems, and are under extreme demand. Since the organization of NIL and CSG, as well as the resources they encompass can be a bit daunting, Timothy Brown was kind enough to produce [Using Neuro-Imaging Laboratory (NIL) Resources to Store, Preprocess, and Analyze fMRI Data](https://cpb-us-w2.wpmucdn.com/sites.wustl.edu/dist/e/952/files/2017/09/using_nil_resources_to_store_preprocess_and_analyze_fmri_data-1p8sv3c.pdf), which details the entire operation quite nicely - it's recommended that new lab members familiarize themselves with the resources discussed in this document, while also understanding that the exact usage of the computational resources may vary according to need within the LCBD.  

Once users are approved by CSG, there are a few options to access DynoSparky:

### Remote Desktop
Remote desktop takes advantage of Xubuntu to provide a graphical interface for navigating DynoSparky. This will be the most natural gateway for users who have not had experience navigating Linux systems via command prompt, or using SSH tunneling - however it is recommended that you spend time learning to use the terminal. Using remote desktop occupies much of the limited compute resources needlessly, and prevents our neuroimaging analyses from running quickly and in parallel. For some tasks and users, remote desktop is a sensible approach, and below are the instructions for using it in such situations.

#### For Mac Users
 1. Download [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop-8/id715768417?mt=12) from the AppStore (either version 9 or 10).
 2. Once installed, open the App and click "New" at the top to set up a new connection.
 3. For "PC Name", enter "dynosparky.neuroimage.wustl.edu". Under credentials, enter your NIL username (user camachoc is displayed below as an example). Note that this is NOT your WUSTL ID, this is your NIL login - they are separate credentials.

    ![Mac Remote Desktop Example](assets/setup_rd_mac.png)

 4. The other settings can be as you wish. The "Connection Name" is what the connection will be listed as in the App. You may choose to not leave your password entered, in which case you'll be prompted to enter it later. You may choose to check or uncheck the boxes at the bottom that determine how the window appears on your monitor.
 5. Close the Edit window. The new connection will now be listed in the main window of the app.

    ![Mac Remote Desktop Connection](assets/setup_rd_mac_connect.png)

 6. Double click the connection to start. The first time that you connect, the App will ask if you want to trust the server. Click yes.
 7. If you did NOT enter your password as part of the connection setup, you will see an error message. Click "Ok", then you will see the screen below. Log in with your NIL username and password.

    ![Mac Remote Desktop NIL Credentials](assets/setup_rd_mac_NIL.png)

 8. Once you are logged in, you should see your desktop, and can proceed to the DynoSparky first-time setup documentation.

#### For Windows Users:
 1. Search for the "Remote Desktop Connection" default Windows application, and open it.

    ![Windows Remote Desktop App](assets/setup_rd_windows.png)

 2. Once the window opens, click the "Show Options" down-arrow to expand the window. For Computer, enter `dynosparky.neuroimage.wustl.edu`. For User name, enter your NIL user name. Note that this is NOT the same as your WUSTL login.

    ![Windows Remote Desktop NIL Credentials](assets/setup_rd_windows_NIL.png)

 3. If you are using a personal computer or account, you can opt to check the "Allow me to save credentials" box to reduce how often you have to log in. You can also click "Save As" and save the Remote Desktop Connection to your desktop to make it quicker to connect in the future.
 4. Double-click your new connection or click "connect" from within the Remote Desktop Connection App. You will be prompted to log in with your NIL credentials. The first time you log in, you will get a message asking to allow connections with the remote computer, or if you can trust the certificate. For both, click "Yes" or "Allow". You can also check the "Don't ask me about this again on this computer" box to avoid having to give permission again later the next time you log in.
 5. Once you have logged in and provided permission to connect, you should see your desktop. If this is your first time connecting, you can proceed to the DynoSparky first-time setup documentation.

### Secure Shell (Terminal / Command Prompt)
Secure Shell ([SSH](https://en.wikipedia.org/wiki/Secure_Shell)) is a secure network protocol for operating services securely, even over unsecured networks. Even so, you must be connected to the [Medical School VPN](https://cpb-us-w2.wpmucdn.com/sites.wustl.edu/dist/5/1185/files/2018/04/vpn-Windows-MED-new.pdf) in order to connect.

On any operating system, you can connect to DynoSparky using SSH.

 1. Ensure you are connected to the VPN
 2. Open the Mac Terminal, Windows Command Prompt, or Linux Terminal

    ![SSH syntax](assets/setup_ssh.png)

 3. Type `ssh -Y NILUSER@dynosparky.neuroimage.wustl.edu` and hit ENTER. You should now be prompted to enter your password. Enter your password and hit enter (no asterisks will appear, like when you enter a password on a website). It may take a moment to connect.

    ![SSH success](assets/setup_ssh_success.png)

 4. If your OS is compatible with X11 forwarding, you may also use the `-X` flag to allow ssh to display graphical programs on your local computer. This is favored over using Remote Desktop, because the computational resources required to display the program are offloaded from DynoSparky, allowing the server to remain optimized in its role for multiple simultaneous connections. On Mac, you may need to first install [XQuarts](https://www.xquartz.org/). E.g.,

    ```
    ssh -X NILUSER@dynosparky.neuroimage.wustl.edu
    ```

## Setting Up for the First Time on DynoSparky
The first time you log in, you'll need to make some modifications. It will be useful first to review the Command Line Help and Tips section. A great deal can be accomplished through the use of even just a few Bash commands, and it *will* save you time and energy to have them at your disposal.

Each user on DynoSparky has a login-node space. This path is abbreviated for each user to `~`, and is equivalent to `/home/user/NILUSER.` This path is not necessarily your personal storage compartment (it is quite small), but *is* used for storing information about your DynoSparky configuration, and tools which may help you navigate the servers.

### C Shell Configuration File
`.cshrc` is a file that is executed each time you execute a new shell (i.e., each time you log in, open a new Remote Desktop session or Xterm window). [.cshrc Docs](https://www.doc.ic.ac.uk/csg-old/linux/Cshrc.html)

The "." at the start of the filename indicates that it is a *hidden* file, a little-known but vastly important convention that allows terminal-users to interact with files not typically displayed in the file explorer window.

LCBD members have compiled the most ubiquitous commands and shortcuts into a template .cshrc, which you can import to your own. The template cshrc file sets up the OS, as well as some PATH and environment variables which allow your user profile to recognize shared lab software packages, such as FSL and FreeSurfer. To merge it with your personal .cshrc (located in ~/.cshrc):

 1. Enter the following command:

    ```
    echo /data/perlman/moochie/user_data/cshrc.txt >> ~/.cshrc
    ```

This will append the contents of the template cshrc file to your personal configuration, `~/.cshrc`. NOTE: it is pertinent that you use `>>` and not `>` (two carats). `>>` appends to a file, whereas `>` writes the file out from scratch.

 2. After making changes to cshrc, they are not automatically loaded. Your csh configuratino file is only loaded once a new connection is made. Alternatively, you can force a refresh with:

    ```
    source ~/.cshrc
    ```

You may notice that this same `source` command is used to configure much of the LCBD environment. E.g., to load the LCBD virtual environment, you could enter:

    ```
    source /data/perlman/moochie/resources/server_access/MRI_env/bin/activate.csh
    ```

or alternatively, the equivalent "alias" which was injected from the cshrc template:

    ```
    sourceLCBD
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

## Setting Up with Wash U's Center for High Performance Computing
In addition to the computational resources managed by NIL, we have access to resources managed by the Mallinckrodt Institute of Radiology's [Center for High Performance Computing](https://www.mir.wustl.edu/research/core-resources/center-for-high-performance-computing/). Keep in mind - this is a computing system fully distinct from the NIL resources, including DynoSparky and Moochie, and is structurally an entirely different animal. Whereas DynoSparky is a self-contained server we explicitly use, CHPC is a [computer cluster](https://en.wikipedia.org/wiki/Computer_cluster), meaning hundreds of computers are linked together to be used in parallel by many users running many different processes at once. Jobs are also not instantaneous, but rather submitted to a job queue, [SLURM](https://slurm.schedmd.com/documentation.html), and run in large batches concurrently, parellized so that, for example, each subject's processing has its own dedicated CPU. This is typically how large neuroimaging analysis jobs are  run in reasonable amounts of time. This level of parallelization is not possible on DynoSparky, though it is itself a powerful computer - HPC is like having a hundred DynoSparky's for a couple hours. 

The MIR's HPC system is dedicated to in-vivo imaging, and currently we are allowed access through the Psychiatry Department. To request an account, you'll need to have completed your HIPAA training through [Learn@Work](http://www.learnatwork.wustl.edu/), after which you can submit an application [here](https://sites.wustl.edu/chpc/for-users/account-request/). Once accepted, familiarize yourself with the [Rules and Guidelines](https://sites.wustl.edu/chpc/for-users/rules-and-guidelines/) for accessing CHPC - there are many more nuances to computing through this system, and you should not jump in without being highly familiar with Unix systems, the SLURM job queue, and awareness of the rescrictions and relevant quotas.

For any lingering questions regarding CHPC, you can visit the [FAQ page](https://sites.wustl.edu/chpc/for-users/frequently-asked-questions-faq/) or email the director, [Malcolm Tobias](mtobias@wustl.edu), who has been extremely helpful and kind in assisting. 

### Connecting to CHPC
Once your account request has been accepted, you can access CHPC. To access via a 'login node' via SSH, you will use the following syntax:

    ```
    ssh -Y <wustl_key>@login3.chpc.wustl.edu
    ``` 

and enter your WUSTL key password to complete authentication. 

Note: login3 is the alias for the "3rd gen" of login nodes used by CHPC. These will connect you "round-robin" to the two login nodes, login3-01 and login3-02. Use of login2 or login1 are deprecated and will lead you to encounter problems. 

To view your disk quotas on CHPC, enter the following command:

    ```
    check_quota
    ```

To view the job queue, you can enter `squeue` or `squeue -u wustlID` to view only the jobs you've submitted.
