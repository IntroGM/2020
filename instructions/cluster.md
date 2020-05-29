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

### sculpin

- Host: `sculpin.seismo.helsinki.fi`
- To change your password you can log in to sculpin and type `passwd`.

### geo-hpcc

- Host: `geo-hpfe.seismo.helsinki.fi` (geo-hpfe is a frontend virtual machine)
- To change your password you can log in to geo-hpfe and type `yppasswd`.

## Connecting from Windows

### Command line access

1. Open **PuTTY**
2. *Host name*: `sculpin.seismo.helsinki.fi`
3. Click *Open*
4. Type in your username and password
5. Type the command `ssh USERNAME@geo-hpfe.seismo.helsinki.fi`, where `USERNAME` is the your username

### File transfers/access

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

1. `ssh USERNAME@sculpin.seismo.helsinki.fi`
2. `ssh USERNAME@geo-hpfe.seismo.helsinki.fi`

Note, `USERNAME` should be replaced with your username.

### File transfers/access

1. `scp -oProxyCommand="ssh -W %h:%p USERNAME@sculpin.seismo.helsinki.fi" USERNAME@geo-hpfe.seismo.helsinki.fi:/globalscratch/USERNAME/douar/modelname/OUT/*.vtk .`

## Navigating the filesystem

- `less FILENAME`: Show the contents of a text file
    - Scroll up and down in file with arrow keys or page-up, page-down
    - Exit by pressing `q`
- `ls DIRECTORY`: List the contents of a directory
- `cd DIRECTORY`: Change into directory `DIRECTORY`
    - *Note*: Typing `cd` with no directory name given will change to your home directory
- `cp FILENAME DESTINATION`: Copy a file from `FILENAME` to `DESTINATION`
    - *Note*: To copy a directory you must include the `-r` flag. For example, `cp -r DIRECTORY DESTINATION`

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

### Editing and running a new job

To run a new simulation with DOUAR you should do the following:
```bash
cd
cd douar/inputs
cp OLDMODEL.txt NEWMODEL.txt
nano NEWMODEL.txt
```
*(edit parameters and save)*
```bash
~/bin/submitdouar.sh -i ~/douar/inputs/NEWMODEL.txt -n 32
```
The number of processor cores to use (given after the `-n` flag) should be no more than 64 to ensure other jobs are not queued waiting for available resources.

### Monitoring job status/cancelling a job

- `squeue`: Show a list of models running on the geo-hpcc cluster
- `scancel JOB_ID`: Cancel a job running on the cluster

## Postprocessing job output

To create the `VTK` files for visualization in **ParaView**, you can do the following:

```bash
cd /globalscratch/USERNAME/douar/NEWMODEL_yymmddhhmmss/OUT
~/bin/process_outbin.sh -n NUM
```
where `NUM` is the time increment between output VTK files (i.e., output written every `NUM`-th time step). For example, `~/bin/process_outbin.sh -n 5` would produce output for every 5th DOUAR time step.

*(optional)* Create a compressed file containing the output files

```bash
tar czvf FILENAME.tar.gz *.vtk
```

*(tranfer* `.vtk` *files using WinSCP or scp - see above)*

*(optional)* Extract files from compressed archive

```bash
tar xzvf FILENAME.tar.gz
```
or extract using your computer's operating system tools (i.e., right-click on file in file browser and extract).

*(Visualize data in ParaView)*
