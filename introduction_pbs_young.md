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

    #!/bin/bash
    # This is a sample PBS job script
    
    #PBS -N MyFirstJobJob Name: A descriptive name for your job, making it easy to identify with `qstat`.       # Job name
    #PBS -l walltime=00:05:00Wall-Clock Time: The maximum real-world time (HH:MM:SS) your job is allowed to run. The scheduler will terminate the job if it exceeds this limit. # Wall-clock time limit
    #PBS -l nodes=1:ppn=1Resource Request: Specifies the number of nodes (computers) and processors-per-node (CPU cores) your job needs. `nodes=1:ppn=1` is one core on one machine.     # Request 1 node, 1 processor
    #PBS -q standardQueue Name: Submits the job to a specific queue. Clusters have different queues for different job types (e.g., short, long, gpu).          # Specify a queue
    #PBS -j oeJoin Output/Error: Combines the standard output (stdout) and standard error (stderr) streams into a single file for convenience.                # Join standard output and error
    #PBS -o my_job_output.logOutput File: Specifies the name for the combined output and error file. If omitted, a default name is used. # Specify output file name
    
    # Change to the directory where you submitted the job
    cd $PBS_O_WORKDIR
    
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
