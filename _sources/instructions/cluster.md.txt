# Using the geo-hpcc cluster

## Suggested software

### Windows

- [PuTTY](https://www.putty.org/) (for connecting and using geo-hpcc)
- [WinSCP](https://winscp.net/eng/index.php) (for copying files from geo-hpcc)
- [ParaView](https://www.paraview.org/download/) (for data visualization)

### Mac/Linux

- [ParaView](https://www.paraview.org/download/) (for data visualization)

## User accounts

Accounts have been created for you on geo-hpcc and sculpin, the gateway maching you need to connect through if you are trying to access the cluster from outside the Institute of Seismology. Some basic information about the machines, including how to change your password, can be found below.

### geo-hpcc

- Host: geo-hpfe.seismo.helsinki.fi (geo-hpfe is a frontend virtual machine)
- To change your password you can log in to geo-hpfe and type `yppasswd`.

### sculpin

- Host: sculpin.seismo.helsinki.fi
- To change your password you can log in to sculpin and type `passwd`.

## Connecting from Windows

### Command line access

1. Open PuTTY
2. *Host name*: `sculpin.seismo.helsinki.fi`
3. Click *Open*
4. Type in your username and password
5. Type the command `ssh username@geo-hpfe.seismo.helsinki.fi`, where `username` is the your username

### File access

1. Open WinSCP
2. Click *New Site*, fill in *Host name*: `geo-hpfe.seismo.helsinki.fi`, *User name* and *Password*.
3. Click *Advanced...*
4. Choose *Connection*, *Tunnel*, and check *Connect through SSH tunnel*
5. Fill in values: *Host name*: `sculpin.seismo.helsinki.fi`, *User name* and *Password*
6. Click *OK*
7. Click *Save*
8. Click *Login*

The WinSCP session will be saved, so that next time you can just double-click the hostname on the left, and the connection will be opened.

## Connecting from Mac/Linux

### Command line access

1. `ssh username@sculpin.seismo.helsinki.fi`
2. `ssh username@geo-hpfe.seismo.helsinki.fi`

Note, the usernames need to be replaced with the your username.

### File access

1. `scp -oProxyCommand="ssh -W %h:%p username@sculpin.seismo.helsinki.fi" username@geo-hpfe.seismo.helsinki.fi:/globalscratch/username/douar/modelname/OUT/*.vtk .`

## Running DOUAR models

1. Modify the model *input file*, e.g., `nano ~/douar/inputs/rift.txt`
2. *Submit* the job: `~/bin/submitdouar.sh -i ~/douar/inputs/rift.txt -n 16`
3. *Monitor* the job status: `squeue`
4. *Post-process* the output:

   - `cd /globalscratch/username/douar/rift_20180514100500/OUT`
   - `~/bin/process_outbin.sh`
   - Note that the directory name of the output is formed from the model name (`rift`) and the date and time of the job submission

5. Copy *VTK files* to your local machine (see instructions above) to be opened in ParaView

You can *cancel* your job with `scancel jobid` where jobid is the numerical ID of the job, and can be found using `squeue`