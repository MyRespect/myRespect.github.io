---
layout: post
title:  "GPU Server Wiki"
categories: Deep_Learning
tags: Server
--- 

* content
{:toc}

This is a quick guide for using Deep Learning Stack Server in OTML-SEIT labs.





# **Server Introduction**

The basic configurations are: TensorEX TS4-178672310 - 2x Gold 5218R, 16GBx16 DDR4, 2x 2TB NVMe SSD, 8x 8TB SATA HDD, 8x RTX A6000, Ubuntu 20.04 w/ EMLI Container Deep Learning Stack.

## **Before Log in**
The server is maintained by DECS and OTML-SEIT labs, so make sure you can access the EGR network in MSU before logging into the server.

- Active your EGR Account before connecting to the server [here](https://www.egr.msu.edu/decs/myaccount/?page=activate).
- For Windows users, you can find Engineering VPN [here](https://www.egr.msu.edu/decs/how-to/new-engineering-vpn). For more information about remote access and VPN setup, please visit [DECS](https://www.egr.msu.edu/decs/).

An alternative option is to use jump server if you are outside of MSU network:
```
ssh {netid}@scully.egr.msu.edu
```

## **Log in**
Use ssh to log in as below:
```
ssh {netid}@otml-seit.egr.msu.edu
```
- **/home/{netid}/**: Upon logging in you’ll have your usual EGR home directory at /home/{netid}, we do not recommend you do storage in the home folder. Instead, we have two other options.
- **/localscratch/ and /localscratch2/**: Each is individual and non-redundant, but very durable; this way there is roughly 7TB worth of fast storage available for working from. Both of these are where other groups have their users make themselves a folder for their own access (named with their netid for ease of identification)
- **/storage/**: It is slower than the NVMe, but has about 42 TB total usable; it is set up to allow the failure of 2 of the 8 drives without data loss. It is highly recommended that running your data from scratch, and then once results are done and ready to be archived/whatever, you can just copy/move them to /storage.

## **Usage**
Just as you drive on the road, the basic principle is not to affect the use of others.

### **Anaconda**
Do not install anaconda in your home folder since it will be large like 3.5G+. Instead, following [conda install](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html) to install anaconda. Here is recommended:
```
/localscratch/{netid}/anaconda3
```

### **Virtualenv**
Virtualenv is a tool to create isolated Python environments. Follow [virtualenv install](https://virtualenv.pypa.io/en/latest/installation.html) to install virtualenv. In most cases, you will not need the sudo permission under virtualenv.

### **Pytorch and Tensorflow**

Follow [pytorch install](https://pytorch.org/get-started/locally/) and [tensorflow install](https://www.tensorflow.org/install) here to install these two softwares.

- The default GPU for OTML Lab (Prof. Liu)  is GPU '0', '1', '2', '3'.

- The default GPU for SEIT Lab (Prof. Yan)  is GPU '4', '5', '6', '7'.

**Be sure not to use multiple GPUs at the same time if one is enough!** If the other lab would like to use their default GPUs, you need to kill the jobs ASAP. Here are some ways to specify the number of GPUs you can use:
```
export CUDA_VISIBLE_DEVICES=4 # Command example to use GPU '4'

os.environ["CUDA_VISIBLE_DEVICES"] = "4" # Command example to use GPU '4'

config = tf.ConfigProto()
config.gpu_options.allow_growth = True # allocate gpu according to its need

config.gpu_options.per_process_gpu_memory_fraction = 0.4 # specifiy GPU memory fraction
```

### **Tmux and Screen**
Tmux is a terminal multiplexer: it enables a number of terminals to be created, accessed, and controlled from a single screen. Tmux may be detached from a screen and continue running in the background, then later reattached. It is recommended to choose Tmux. Here is some example usages:
```
tmux new -s <session-name>
tmux detach
tmux attach -t <session-name>
tmux kill-session -t <session-name>
tmux switch -t <session-name>
```

An alternative choice is to use screen. You can start a screen session and then open any number of windows (virtual terminals) inside that session. Processes running in Screen will continue to run when their window is not visible even if you get disconnected. Here is some example usages:
```
sudo apt install screen
screen -S yourname # create a named session， 
screen -r yourname # reattach to your session
screen -ls # list all sessions
```

### **MISC**
Here are some other commands you may find helpful.

#### Checking GPU usage
Use the below command for a breakdown of the GPU usage:
```
nvidia-smi
watch -n 1 nvidia-smi
```

#### Transferring files to the server
Here are some recommendations for transferring files: 
```
scp, rz/sz, FileZilla
```

#### Git
Here are some git commands in case you may use git on the server:
```
git status  # check git status
git add -A	# Add all new and changed files to the staging area
git commit -m "[commit message]"	# Commit changes
git push -u origin [branch name]	# Push changes to the remote repository (and remember the branch)
git diff file # Preview changes before merging
git checkout # Switch to a branch and discard the changes
git reset HEAD file # Discard changes in the staging area
```
