### Create a key
Create the key pair:
```
ssh-keygen -f ~/.ssh/dl2Key
```
Get the public key:
```
cat ~/.ssh/dl2Key.pub
```
### Login at the portal
Open browser and visit https://portal.cua.surf.nl/login/?next=/home/ , then login your scur account. 
Add the public key to https:/portal.cua.surf.nl/user/keys.
### Connect to Snellius server
#### Connect from the terminal (WSL):
```
ssh -i ~/.ssh/dl2Key scur0411@snellius.surf.nl
```
#### Connect with VSCode:
Copy your keys to `C:\Users\<User>\.ssh`. Open VSCode. From the blue/green button at the bottom left choose "Connect to Host". Then choose "Add new ssh host" and enter:
```
ssh -vvv -i C:\Users\<User>\.ssh\dl2Key scur0411@snellius.surf.nl
```
### Create an environment
```
TODO
```
## Submitting jobs

Submitting the job:
```
sbatch my_job.sh
```
Cancel the job:
```
scancel [jobid]
```
Check what is going on with a job:
```
scontrol show job [jobid]
```
Interactive session:
```
TODO
```
### Exit Snellius server
```
exit
```
### Useful links: 
https://servicedesk.surf.nl/wiki/display/WIKI/Example+job+scripts#Examplejobscripts-Snellius-MIG
https://servicedesk.surf.nl/wiki/display/WIKI/Software+policy+Snellius
https://servicedesk.surf.nl/wiki/display/WIKI/Software+development
https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial1/Lisa_Cluster.html
https://servicedesk.surf.nl/wiki/display/WIKI/Loading+modules
https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial1/Lisa_Cluster.html#Remote-development-with-VSCode-or-PyCharm
https://servicedesk.surf.nl/wiki/display/WIKI/Interactive+jobs

