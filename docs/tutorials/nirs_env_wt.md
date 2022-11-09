The LCBD's fNIRS processing is performed on DynoSparky in a Jupyter Notebook (IPython) environment. If you haven't yet connected to DynoSparky, see [the DynoSparky tutorial](../../setting_up/DynoSparky.md) for steps on starting an SSH connection.

This environment was selected for a few reasons:
- the computations happen on DynoSparky's resources, meaning your local PC or laptop won't be bogged down
- longer computations can run in the background on the Jupyter server for hours, or even days, without dropping
- the implementation of a [virtual environment](https://docs.python.org/3/library/venv.html) means that every user can instantly acquire all of the dependency packages that fNIRS analysis relies on, such as:
  - [numpy](https://numpy.org/)
  - [pandas](https://pandas.pydata.org/)
  - [MNE](https://mne.tools/stable/index.html)
  - [MNE-NIRS](https://mne.tools/mne-nirs/stable/index.html)
  - [PyCWT](https://pycwt.readthedocs.io/en/latest/)
  - and more
- moreover, the exact versions of these dependency packages are maintained across users ensuring consistent results

The process for starting and connecting to a server is a bit lengthy, but is streamlined by the use of some helpful bash scripts. Note that you must be either connected to the VPN or on WUSTL internat. The full proceedings are as follows:
1. Load the virtual environment, giving access to Jupyter and all of the dependencies
2. Launch a Jupyter Notebook server (from DynoSparky) on a specific port
3. Open an SSH tunnel to that port of DynoSparky on your local machine
4. Visit the tunnel in a web page to direct Jupyter from your local machine

To simplify this process, a single script, [startJupyterServer.sh](https://github.com/ChildBrainLab/LCBDtools/tree/main/scripts/NIRS/startJupyterServer.sh) takes care of steps 1 and 2, and prints exact directions for steps 3 and 4.

To execute the script, navigate to `/data/perlman/moochie/github/LCBDtools` on DynoSparky using the following command:

    cd /data/perlman/moochie/github/LCBDtools
    
and subsequently run:

    bash scripts/NIRS/startJupyterServer.sh <port number>
    
where the port number is an integer between 9000 and 9999. Ideally, each lab member should consistently use the same port number. Most numbers should be available, but it might be sensible to use something memorable to you, for example:
- 9090
- 9000
- 9999
- 9001

If run successfully, the output should resemble the following:

```
dynosparky:/data/perlman/moochie/github/LCBDtools% bash scripts/NIRS/startJupyterServer.sh 9000

Currently running servers: http://localhost:9000/ :: /data/perlman/moochie/github/LCBDtools
Tmux session started on port 9000.

Connect on your local machine by running the following command (while on VPN or WUSTL internet):
ssh -N -f -L 8888:localhost:9000 schneiderc@dynosparky.neuroimage.wustl.edu

and then open a web browser and go to the address, 'localhost:8888'

If you are connecting from a new computer, you will need to copy the access token.
Copy the string of the https://localhost...... address following 'token=' .
This is what you will paste in the 'password' entry when connecting.
```

The command for connecting on your local machine should be tailored with the port number you used, as well as your NIL username. This command is what you'll enter from your local machine to open the SSH tunnel to the Jupyter server on DynoSparky:
    
    ssh -N -f -L 8888:localhost:<port number> <NIL username>@dynosparky.neuroimage.wustl.edu
    
replacing the two arguments with your port number and NIL username respectively.

Once that is complete, you can open a web browser (any should be fine) and navigate to the URL, localhost:8888, where you should find the Jupyter web page. 

If this is your first time connecting from that computer, you may need an access token. If so, you can find the token in the output of the startJupyterServer.sh script, in the URL, following the phrase, "token=". Copy this string of characters and paste it into the 'password' field.

Then you should be good to go!
