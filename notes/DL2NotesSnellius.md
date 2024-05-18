## Create a key
Create the key pair:
```bash
ssh-keygen -f ~/.ssh/dl2Key
```
Get the public key:
```bash
cat ~/.ssh/dl2Key.pub
```
## Login at the portal
Open browser and visit https://portal.cua.surf.nl/login/?next=/home/ , then login your scur account. 
Add the public key to https:/portal.cua.surf.nl/user/keys.

## Connect to Snellius server

### Connect from the terminal (WSL):
```bash
ssh -i ~/.ssh/dl2Key scur0411@snellius.surf.nl
```
### Connect with VSCode:
Copy your keys to `C:\Users\<User>\.ssh`. Open VSCode. From the blue/green button at the bottom left choose "Connect to Host". Then choose "Add new ssh host" and enter:
```bash
ssh -vvv -i C:\Users\<User>\.ssh\dl2Key scur0411@snellius.surf.nl
```

## Clone a private Github repo at Snellius:

Only one person needs to do that.

### Create a key in Snellius:
```bash
ssh-keygen -f ~/.ssh/gitKey
```

### Add the host and key to the config file:
```bash
Host github.com
HostName github.com
IdentityFile ~/.ssh/gitKey
```

Then, open the private Github repo settings and add the generated public key as a deploy key. Now you can access the private repo from Snellius.

## Submitting jobs

### Submitting the job:
```bash
sbatch my_job.sh
```

### Check what jobs are in the queue:
```bash
squeue
```

### Check what is going on with a job:
```bash
scontrol show job [jobid]
```

### Cancel a job:
```bash
scancel [jobid]
```

### Run an interactive session:
Run the following and wait at the queue until your job starts:
```bash
srun  --partition=gpu --gres=gpu:1 --cpus-per-task=9 --gpus=1 --job-name=Interactive --ntasks=1 --time=01:00:00 --mem=32000M --pty /bin/bash
```

***Note:*** When your time ends, Snellius will wait for 30 seconds and kick you out. You might loose results. To check how much time your session has been running you can use `squeue`.

## Creating an environment
Execute the following. You don't need to be in an interactive session.
```bash
conda create --name instdiff1 python=3.8 -y
conda activate instdiff1
pip install -r requirements.txt
```

***NOTE:*** You can access that environment from jobs and interactive sessions, however instead of writing `python` you need to use it as `/home/scur0411/.conda/envs/instdiff1/bin/python`. For now we haven't found a fix for this.

## Exit Snellius server
```bash
exit
```

## Useful links: 
https://servicedesk.surf.nl/wiki/display/WIKI/Example+job+scripts#Examplejobscripts-Snellius-MIG
https://servicedesk.surf.nl/wiki/display/WIKI/Software+policy+Snellius
https://servicedesk.surf.nl/wiki/display/WIKI/Software+development
https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial1/Lisa_Cluster.html
https://servicedesk.surf.nl/wiki/display/WIKI/Loading+modules
https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial1/Lisa_Cluster.html#Remote-development-with-VSCode-or-PyCharm
https://servicedesk.surf.nl/wiki/display/WIKI/Interactive+jobs
