Interactive Guide to HPC Commands
=================================

Your essential toolkit for navigating High-Performance Computing clusters.

Introducing **`qsub`** **`qstat`** **`qdel`** commands in Young and best practices!

What is a Job Scheduler?
------------------------

Imagine a library with only a few study rooms, but hundreds of students needing to use them. A librarian would manage who gets a room, for how long, and when. In an HPC cluster, the "librarian" is the \*\*Job Scheduler\*\*.

It's a system that manages a queue of jobs, allocating resources like CPUs and memory to ensure fair and efficient use of the cluster among all users. The commands **`qsub`**, **`qstat`**, and **`qdel`** are your primary tools 
for interacting with this "librarian."

### The Job Lifecycle

A job goes through several states from submission to completion. Click on each step below to learn more.

#### 1\. Submit

```bash
qsub job.sh
```

#### 2\. Queued

**`qstat`** shows 'Q'

#### 3\. Running

**`qstat`** shows 'R'

#### 4\. Finished

Job completes or is deleted with **`qdel`**

`qsub`: Submitting Your Job
-----------------------------

The **`qsub`** (Queue Submit) command is used to send your job script to the scheduler. This script contains resource requests and the commands to execute.

### Interactive Job Script (job.sh)

Hover over the highlighted directives in the script below to understand what each one does.

    #!/bin/bash -l # Specifies the script should be run with Bash as a login shell.

    # PBS Directives:
    #$ -l h_rt=48:00:00 # Sets the hard runtime limit for the job to 48 hours (maximum allowed in Young).
    #$ -l mem=4.5G       # Requests 4.5 Gigabytes of memory for the job.
    #$ -pe mpi 40       # Requests 40 cores using the MPI parallel environment.
    #$ -N First_job      # Names the job "First_job".
    #$ -A KCL_YOUR_GROUP # Specifies the accounting project for resource usage.
    #$ -ac allow=Y      # Allows the job to run on specific nodes/partitions.
    #$ -P Gold      # Assigns the job to the "GoldLong" queue.
    #$ -cwd             # Executes the job from the current working directory.
    
    echo "Hello from my first HPC job!"
    echo "Current date and time: $(date)"
    sleep 60
    echo "Job finished successfully!"
    

`qstat` Checking Job Status
------------------------------

**`qstat`** is your window into the scheduler, showing the status of your jobs and others on the cluster. The most important column is \`State\`.

### Understanding \`qstat\` Output


|Job ID            | Username     | Queue    | Jobname      | State       |  
|------------------| -----------  | -------- | -------------| -------     |
|12345.scheduler   | your_user    | standard | MyFirstJob   | R (Running) |
|12346.scheduler   | another_user | long     | DataAnalysis | Q (Queued)  |


`qdel`: Deleting a Job
------------------------

Use **`qdel`** to cancel a job that is either running or waiting in the queue. You might do this if you realize there's a mistake in your script or if you no longer need the results.

```bash
qdel <Job\_ID>
```

For example, to delete the job **`12345.scheduler`**, you would run:

```bash
qdel 12345
```

`lquota`: Checking your account's disk space:
---------------------------------------------------

Use **`lquota`** to check the status of your quota. The result will be something like:

|Storage | Used       |Quota        |% Used |  Path
|--------|------------|-------------|-------|---------
|home    | 43.85 GiB  |  100.00 GiB | 43%   | /home/mmmXXXX
|scratch | 80.56 GiB  |  250.00 GiB | 32%   | /scratch/mmmXXXX

indicating your current disk space usage. 

Best Practices & Tips
---------------------

Follow these tips to use the HPC cluster efficiently and effectively.

### 1\. Start Small

Request minimal resources for initial tests. This gets your job scheduled faster and saves resources if it fails.

### 2\. Test Locally

Whenever possible, test your scripts on the login node with small datasets to catch simple errors before submission.

### 3\. Meaningful Job Names

Use **`#PBS -N`** with a descriptive name. **`Analysis\_Phase1`** is better than **`MyJob`**.

### 5\. Check Output Files

After a job finishes, always check your log file for errors and results. This is your primary debugging tool.

### 6\. Be Realistic with Walltime

Request enough time, but don't grossly overestimate. A job asking for 100 hours might wait longer in the queue than one asking for 2 hours.
