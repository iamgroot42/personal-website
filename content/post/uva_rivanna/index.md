---
title: Running scripts on Rivanna at UVA
subtitle: "A tutorial on how to run scripts on [Rivanna](https://www.rc.virginia.edu/userinfo/rivanna/overview/) (SLURM in general) cluster at UVA, along with some tricks."

# Summary for listings and search engines
summary: "A tutorial on how to run scripts on Rivanna (SLURM in general) cluster at UVA"

# Link this post with a project
projects: []

# Date published
date: "2022-02-03T00:00:00Z"

# Date updated
lastmod: "2022-02-03T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
 caption: 'Image credit: [**uvarc/rc-website (GitHub)**](https://github.com/uvarc/rc-website/blob/master/static/images/carousel/network-server-lights-copy.png)'
 focal_point: ""
 placement: 2
 preview_only: false

authors:
- admin

tags:
- slurm
- rivanna
- guide
---

# Overview

Our department has a nice collection of 16 servers, each with 4 GPUs. While this may sound like a lot, the servers are shared across the department and thus fill out pretty fast. We do have a SLURM cluster too, but it's not as fast (or big enough to run GPU-based jobs via SLURM) as the servers. I recently looked into [Rivanna](https://www.rc.virginia.edu/userinfo/rivanna/overview/) after my [advisor](https://www.cs.virginia.edu/~evans/) suggested it. I did write some sbatch scripts during my internship at [Oracle Research Labs](https://labs.oracle.com/pls/apex/labs/r/labs/intro), so that definitely came in handy while trying to write wrapper scripts for the Rivanna cluster.

# Settings things up

The structure for these environments here is pretty similar to what we have for the CS servers. You can load up specific modules using `module load`. Since sbatch files get passed onto in a new bash environment, it's always a good idea to have all your `module load` and other related commands (like `conda activate`) in your `.bashrc` file so that you don't have to worry about adding all of them to every sbatch file.

The only downside here is that storage is not shared with the other CS servers: you must either commit to using only Rivanna or only CS servers, or make sure you regularly sync your generated code (which is straightforward, thanks to Git) and data (not so trivial).
<div class="alert alert-danger" role="alert">
  Also, the Rivanna cluster has a cronjob of sorts that <b>deletes files</b> that aren't accessed for more than <b>90 days</b>.
</div>

# Writing SBATCH scripts

Okay, enough talk! Let's start with a basic wrapper script- let's call it `test.sbatch`. If you are already familiar with these options and/or just want to use the defaults and get started, feel free to use the template [here](https://gist.github.com/iamgroot42/2a29141b5cb241aa82aa80809b420437).

```bash
#!/bin/bash

#SBATCH --ntasks=1
#SBATCH -A uvasrg
#SBATCH --mem=32G
#SBATCH -p gpu
#SBATCH --gres=gpu:rtx2080:1
#SBATCH --cpus-per-task=10
#SBATCH --time=3-00:00:00
#SBATCH --output=logs/%x-%j.out
#SBATCH --error=logs/%x-%j.err

# Your command goes here
echo "Parameters were $PARAM1 and $PARAM2"
```

Wait, wait, wait! Aren't the `#SBATCH` commands going to get ignored (starts with `#`, so it's got to be a comment, duh?).
Well, not really- `#SBATCH` are treated differently by sbatch.

### `#SBATCH --ntasks=1`

This argument specifies the number of tasks you want to run with the given script. In most cases, this will be 1 unless you want parallel execution
and will utilize it explicitly via your script.

For instance, you could specify `--ntasks=2` to run two tasks in parallel:

```bash
 echo "Hello there" & 
 echo "General Kenobi" &
 wait
```

This can be particularly useful when you have some form of caching, utilize the same file across different scripts, or just like the idea of having all your experiments run on the same physical machine.

### `#SBATCH -A uvasrg`

This option specifies which "allocation" you want to use. As a user at UVA (or SLURM clusters in general), you may be part of one or more allocation groups, which all have their compute budgets. This option ensures that your scripts run on the allocation group you specify. If you happen to be in [UVASRG](https://uvasrg.github.io), this is the option for you!

If you're curious about the compute budget used up/left in your allocation, you can run:

```bash
allocations
```

Furthermore, if you're curious about other members in the same allocation group, you can run:

```bash
allocations -a <your_group_name>
```

### `#SBATCH --mem=32G`

This is pretty straightforward and lets you specify the total memory (in GBs) you want to allocate to your job. In most cases, 32 or 64 GBs is enough.
<div class="alert alert-info" role="alert">
  Rivanna has an upper limit of 32 GBs of memory per core, so you'll need to be careful with this.
</div>

### `#SBATCH -p gpu`

This option here specifies which partition you want your scripts to run on. For most users (at least with machine learning), this will be 'gpu'. You can have a look at all the available partitions [here](https://www.rc.virginia.edu/userinfo/rivanna/queues/).

### `#SBATCH --gres=gpu:rtx2080:1`

This option here (the `--gres` in general) allows you to specify configurations of the nodes you want to run your scripts on. In this case, the `rtx2080` is the name of the GPU card, and `1` is the number of cards you want to use. In terms of speed, you might want to prefer v100 over 2080.

One thing to note here: there may be times when machines with one type of GPU card are free while others are in use. It might be better to specify the other GPU in such cases instead of waiting for the busy GPU machines to clear up. But how exactly would you know which machines are free and which ones are not?

```bash
sinfo -o "%20N %10R %10e %25f %25G %t %C" -t IDLE,MIX
```

This provides information on the status of all machines, their GPU cards (and how many they have), free memory, and free/busy CPU cores. Pretty useful, huh!

### `#SBATCH --cpus-per-task=10`

This option lets you specify the number of CPUs you require per task in your script. From what I know, Rivanna has an upper limit of 10 per job for the GPU servers, but I'm not too sure about it.

### `#SBATCH --time=3-00:00:00`

This option here lets you specify an upper limit for your job. SLURM will terminate any jobs after this given time limit (set to a default value much lower than 3 days for Rivanna, from what I remember). We have an upper limit of 3 days, so this pretty much tells SLURM to run the job for as long as possible. If you want to run your job for longer, make sure you cache results so that the re-run can pick up from where the previous job left off.

### `#SBATCH --output=logs/%x-%j.out`

These two formats (this and `#SBATCH --error=logs/%x-%j.err`) specify the filenames for error and log files. The `%x` is the script's name, and `%j` is the job ID.

<div class="alert alert-secondary" role="alert">
  P.S. You might wanna make sure you have the directory `logs` where you submit your job.
</div>

## Submitting a job

There you go! Now that you know what each flag here means, let's get to submitting the job:

```bash
 sbatch --export=ALL,PARAM1='no',PARAM2=42 --job-name=try test.sbatch
```

Note how we have two additional parameters, `PARAM1` and `PARAM2`. This is a way for you to pass parameters to your job, which can then be used in your script.
Make sure you leave the `ALL` in there since this passes on your environment's predefined variables to the job.

<div class="alert alert-danger" role="alert">
  Another sidenote (that I found out the weird way) - make sure all your flags and options are specified <b>before</b> the filename of your script. If they aren't, `sbatch` will simply ignore them.
</div>

# Job Arrays

Sometimes you might want to run the same commands inside but with different parameters- maybe you want to run a grid search or generate results for multiple datasets. Instead of creating new sbatch scripts in this case, you can utilize job arrays. This is a way to run multiple jobs with the same configuration but with different parameters.

For instance, you could run a job array with the following command:

```bash
sbatch --job-name=try test.sbatch --array=0-6
```

Note the additional `--array=0-6` option. This specifies that you want to run 7 jobs, starting from 0 until 6 (all-inclusive). You could then modify the sbatch script to use different parameters based on the job index:

```bash
...
RATIOS=(0.2 0.3 0.4 0.5 0.6 0.7 0.8)
echo "Running for ratio ${RATIOS[$SLURM_ARRAY_TASK_ID]}"
...
```

# Tips and Tricks

## Start-Time Estimate

Submitted a job, but it's stuck in `(Priority)` or `(Resources)`? If you see it's way too much, you can always default back to some other servers (or change the configuration of your job so that it gets accepted). In cases like these, getting a rough estimate for the start time for your job can be helpful.

```bash
 squeue -u <your_username> --start
```

If you skip the `--start` above, you can view all your jobs and relevant information: how long they've been running for, their status, etc.

## Opening interactive sessions

Sometimes you may want to test your code out for smaller cases and/or debug, and going to and fro with log files may not be very convenient. You can open an interactive session with your job using the following command:

```bash
 ijob -A uvasrg -p gpu --time=0-00:30:00 --gres=gpu:rtx2080:1 --mem=8G 
```

This command here would open an interactive session (capped at 30 minutes, so that you don't accidentally leave it on and get charged for it) with your job, run on a machine with the RTX2080 GPU card and an allocation of 8GB memory.
