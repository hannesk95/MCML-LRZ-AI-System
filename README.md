# How to use MCML AI System
The following provides MCML affliates with a documentation about how to use the MCML AI System for our deep learning training purposes. Please find the detailed documentation in the MCML wiki [here](https://doku.lrz.de/lrz-ai-systems-11484278.html).

# (0) Getting Started

(1) Get in contact with one of the master users (Stefan Fischer, Johannes Kiechle) \
(2) Ask them to create a personal LRZ ID, which will be the login ID for the MCML AI cluster (similar to TUM ID, usually with a number as extension: ge37bud --> ge37bud2) \
(3) Once you know your **personal LRZ ID** and the **email address** which is linked to the LRZ ID, please reset your passwort and create a personal one [here](https://idmportal.lrz.de/pwreset). Select: **I know an e-mail address associated with my LRZ account** and follow the instructions. \
(4) After setting your personal passwort please login into the [LRZ IDM Portal](https://idmportal2.lrz.de/jidmp/) and accept the export control statement (otherwise you'll be not able to login into the LRZ AI Login Node from where you can start allocating GPU ressources). \
(5) Try to login into the LRZ AI Login Node using the command below.


In order to access the LRZ AI System please SSH into the login node using

```cmd
$ ssh login.ai.lrz.de -l your_user_ID
```
This login node is meant for preparing and submitting jobs to one or more of the available resources (see below), which are managed by the Slurm Workload Manager. \
Alternatively, if you favour "Klickibunti" you can login using a [web-based frontend](https://login.ai.lrz.de ).

# (1) Compute Hardware

### (1.1) Big Scale MCML Hardware (allocation time limit for individual jobs is 4 days)
Can only be used by MCML members. 
![image info](./assets/MCML_hardware.png)

```cmd
Important: Users need to specify the "mcml" quality of service (QoS) for their job allocation

$ salloc --partition=mcml-hgx-a100-80x4 --qos=mcml --ntasks=4 --gres=gpu:4 
$ salloc --partition=mcml-dgx-a100-40x8 --qos=mcml --ntasks=8 --gres=gpu:8  
```

---

### (1.2) Small Scale MCML Hardware (allocation time limit for individual jobs is 4 days)
Can only be used by MCML members.
![image info](./assets/SmallScale_MCML_hardware.png)

The first row indicates that there are five nodes whose GPUs are partitioned in three virtual GPU instances each: One with three slices out seven, and two with two slices each. The second row, indicates there are two nodes whose GPUs are partitioned in four virtual GPU instances each: one with three slices, one with two slices, and two with one slice. 

**Example:** If you want to allocate one instance with three slices (i.e., three seventh the capacity of a full A100) the following code block shows an example. 
```cmd
Important: Users need to specify the "mcml" quality of service (QoS) for their job allocation

$ salloc --partition=mcml-hgx-a100-80x48-mig --qos=mcml --gres=gpu:3g
```
The GPUs of the mcml-hgx-a100-80x4-mig partition can alternatively also be used in full by specifying the **--gres=gpu:[1-4]** option.

---

### (1.3) General Hardware (default time limit for individual jobs is one hour, maximum is 3 days)
Can also be used by non-MCML members.
![image info](./assets/General_hardware.png)

```cmd
$ salloc --partition=lrz-dgx-a100-80x8 --ntasks=8 --gres=gpu:8 --time=3-00:00:00
```

# (2) Data Storage

# (3) Submitting a Job

Some useful slurm commands: 
```console
Show all available queues
$ sinfo 

Show reservation of queus (if there is any)
$ squeue

Create allocation of ressources
$ salloc 
Example:
$ salloc --partition=mcml-hgx-a100-80x4 --qos=mcml --ntasks=4 --gres=gpu:4 

Cancel allocation of ressources
$ scancel <JobID>

Run job
$ srun
Example:
$ srun --pty enroot start --mount ./data:/mnt/data ubuntu.sqush bash

Submit batch job into SLURM pipeline
$ sbatch
Example:
$ sbatch script.sbatch
```