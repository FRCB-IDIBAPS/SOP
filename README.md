[How to acces the resources](#how-to-acces-the-resources)

[Folder structure](#folder-structure)

[Version Control](#version-control)

[How to upload data](#how-to-upload-data)

[Data maintainment](#data-maintainment)

[How to execute jobs](#how-to-execute-jobs)

[Interactivelly:](#interactively)

[Batch Submmiting:](#batch-submitting)

[Rstudio in Nord4 server](#rstudio-in-nord4-server)

# How to acces the resources
==========================

Cluster resources can be reached in different ways, but all require
having a valid username ID and a password to get access to them. The
connection will always be made through one of the available login nodes
n4login1, n4login2 and n4login3.

-   **Console** :

    -   Open a terminal in your PC and type:

```bash
ssh <user_name>@<login_node>.bsc.es
```

Replace \<username\> with your nord4 ID and \<login\_node\> with either n4login1, n4login2 or n4login3. The prompt will ask you to type
in the password and you will see something like this:

![Alt text](./images/image1.png)

In this case we have logged in via n4login2. If we type pwd we will
see, we are inside our home (**/home/rcli/\<user\_name\>**) directory.
Using cd command you can move inside the system.

-   **Nautilus**

    -   On a Unix system, open a Nautilus window and search the "+ Other
        Locations" button on the left hand bar.

        ![Alt text](./images/image2.png)

    -   Click on it and you will see a bar on the right bottom of the
        page

        ![Alt text](./images/image3.png)

Type 
```bash
sftp://<user_name>@<login_node>.bsc.es/
```
and click "Connect".

Your home (**/gpfs/home/\<institution\>/\<user\_name\>**) will open and then you can
navigate through all the directories.

-   **Visual Studio Code**

    -   In Visual Studio Code you can establish connection through ssh
        aswell.

    -   Click the following icon on the bottom-left corner of the
        screen:

        ![Alt text](./images/image4.png)

    -   It will open the following dialogue box on top of the screen:

        ![Alt text](./images/image5.png)

    -   Here you can choose either "Connect to Host" or "Connect Current Window to Host". Then clink on "+ Add New SSH Host"

        ![Alt text](./images/image6.png)


    -   and type: 
    
    ```bash
    ssh <user_name>@<login_node>.bsc.es
    ```

    -   If you repeat the process, you will find a button called "Configure SSH Hosts". Then clik on "/home/user/.ssh/config"

        ![Alt text](./images/image7.png)

    -   The config file will open for edit. By default, you will have something like this:

        ![Alt text](./images/image8.png)

    -   Replace \<Name\> with your nickname and \<user\_name\> with your nord4 ID and save the file. You can also choose one of thEvery time you want to establish connection to the server with VSC you can just click on your name after "Connect to Host" or "Connect Current Window to Host".

# Folder structure
================

Nord4 uses a General Parallel File System (GPFS) where its mounted:

![Alt text](./images/image9.png)

For most users, the most important folders will be:

**/gpsf/home**

**/gpfs/projects**

**/gpfs/apps**

**/gpfs/scratch**

These directories are available from all the Nord4 nodes.

-   **/gpfs/home**:

    -   **/gpfs/home** contains all the user's home directories inside the
        corresponding group folder. For example:
        **/gpfs/home/rcli/idib057763/**.

    -   This is the current directory once the user logs in.

    -   Each home is private to its user and has a default quota of
        10GB, so it's not intended to store data.

    -   A backup is performed daily for **/gpfs/home**.

    -   It's indicated to store personal data and own-developed scripts.

-   **/gpfs/projects**:

    -   **/gpfs/projects** is a specific directory for each group. For
        example, all the users in the group idib57 will have a common
        space in **/gpfs/projects/idib57**.

    -   This is where you should run your jobs.

    -   The data in **/gpfs/projects** is shared with all the users in the
        group.

    -   The quota of **/gpfs/projects/\<groupID\>** can be consulted running
        *bsc\_quota* in the terminal.

    -   IMPORTANT!! **/gpfs/projects** is not intended for long-term
        storage. See data maintainment part.

    -   **/gpfs/projects** is backuped periodically.

-   **/gpfs/apps/MN4**:

    -   **/gpfs/apps** contains all the software, applications and libraries
        already installed in the system and available to all users. For
        example, if you want to run an R script with the R version 4.3.0
        you should do:

        ```bash
        module load gcc pcre2 R/4.3.0 #load the desired modules and their dependencies
        ```

        ```bash
        Rscript <script_name>.R #run the script
        ```

    -   For example, if we get the directory tree of the R folder with 1
        level in depth, we can see all the R versions available. Each
        one with its specific folder:

        ![Alt text](./images/image10.png)

    -   And the tree of the R/4.3.0 with 4 levels in depth:

        ![Alt text](./images/image11.png)

    -   In **/gpfs/apps/MN4/R/4.3.0/GCC/lib64/R/library/** you can find all    the installed packages for this R version. Just to have a glimpse:

        ![Alt text](./images/image12.png)


    -   The users are not allowed to modify any content of **/gpfs/apps** MN4.
        If you need some library or package that is not found in
        /gpfs/apps/MN4 you should ask <support@bsc.es> to do it for you.
        Please, first make sure that it is actually not installed.

    -   Another important folder for bioinformatics is
        **/gpfs/apps/SL/BIODATA**. This folder is intended to store biological and clinical databases as well as reference files such as genome or transcriptome versions for the most common model organisms. It was originally created in StarLife (SL) cluster but is accessible to all NORD4 users. In this manner it is not necessary to upload these files to your **/gpfs/projects/\<group\_ID\>** if it is already in **/gpfs/apps/SL/BIODATA** and redundant copies inside the system are avoided.


-   **/gpfs/scratch**:

    -   Each user has a directory in
        **/gpfs/scratch/\<group\_ID\>/\<user\_ID\>**.

    -   This directory is only intended to store temporary files during
        job execution and doesn't have any backup. However, this space in neither intended for long term storage. So is responsability of each group users to delete all the unnecessary files periodically.

# Version Control

There\'s no outgoing internet connection from the cluster, which
prevents the use of external repositories directly from our machines. To circumvent that, you can use the sshfs command in your local machine.
Doing that, you can mount a desired directory from our GPFS system in
your local machine.

First, by mounting a desired remote directory onto a local directory of
your laptop, you can work locally on your existing code in the cluster
and making sure all the changes are replicated in the corresponding file within the GPFS system.

Second, you can track all the changes in your code by synchronizing your local working directory with a cloud repository (using git).

![Alt text](./images/image13.png)

Steps:

1.  Create an empty directory in your local machine that will be used as
    a mount point.

2.  Run sshfs to mirror the directory of interest in the HPC with your
    newly created directory.

```bash
sshfs -o workaround=rename [username]@n4login1.bsc.es:/path/to/remote/directory /path/to/local/empty/directory
```

3.  (If not done already) Clone an existing repository (GitHub,
    GitLab) into your local mounted directory. This will be immediately
    reflected in your remote directory.

4.  Check that your repository is up to date, and which branch you are
    operating from by running **git status** from your working terminal.

# How to upload data

To copy files from/to the Cluster you can use direct scp, sftp or rsync with the aid of transfer nodes. 

We recommend using rsync for uploading large datasets since it allows
resume in case of unstable connection. The typical command through the
transfer node would be:

-   **\# From local machine to remote path** (execute in local):

    ```bash
    rsync -av --progress --partial --append local/path/files [user_name]@transfer1.bsc.es:/path/to/remote/directory
    ```

-   **\# From remote path to local** (execute in server):

    ```bash
    rsync -av --progress --partial --append [user_name]@transfer1.bsc.es:/path/to/remote/files /your/local/destination/
    ```

The transfer progress will be shown in the terminal as the files are
being copied. Also, if the connection is being interrupted the transfer
can be resumed because of the "\--partial" flag.

# Data maintainment 

A typical workflow when using an HPC environment involves three main
steps: loading raw data onto the server (1), performing the analysis
(2), and retrieving the results and downloading them to a shared space
accessible to project collaborators (3).

But what happens as we start working on more projects? The amount of
data---ranging from gigabytes to terabytes---stored in our HPC space can
grow rapidly. That's why a plan to manage our data and storage space in
a responsible and safe manner is needed.

As mentioned in the folder structure section, /gpfs/projects/ is not
intended for long-term data storage. Therefore, once a project is
completed, data should be transferred to more appropriate storage
solutions. Regular cleanup sessions should also be conducted to free up
space for active projects.

Once a project is completed, it is good practice to deposit raw data in
a public repository (e.g., EGA, ENA, GEO) to make it accessible to the
scientific community. Likewise, all code written during the analysis
should be pushed to a version-controlled repository (e.g., GitHub,
GitLab) to ensure reproducibility. Temporary and intermediate files
should be removed once the project has concluded.

Quota space on nord4 can be consulted via the command **bsc\_quota**.

In the following image are shown the different block spaces for group
idib57.

![Alt text](./images/image14.png)

Each user has a fixed quota of 80 GB in their home directory, which is
accessible only by that specific user---hence the *USR* type. In
contrast, the quotas for /gpfs/projects/GROUP and /gpfs/scratch/GROUP
are shared among all members of the same project, corresponding to the
*GRP* type.

The different fields in the quota output are explained below:

-   **Usage**: The amount of disk space currently in use.

-   **Quota**: The soft limit of the fileset. Exceeding
    this limit by less than 5% will not immediately block writes, but if
    the limit remains exceeded beyond the grace period (typically 7
    days), the fileset will be blocked.

-   **Limit**: The hard limit of the fileset. Surpassing
    this limit will immediately block any further data writing.

-   **In doubt**: Disk usage that is not yet accounted
    for. This may result from ongoing operations, exceeded quotas, or
    write errors.

-   **Grace**: The time remaining before the fileset is
    blocked due to exceeding the soft limit. The default grace period is
    7 days.

-   **Files**: The total number of files within the
    fileset.

-   **In doubt** (Files): Same as above, but refers to
    unaccounted file counts rather than disk space.

# How to execute jobs

## **Interactively:**

In NORD4 you can run interactive jobs directly in the command line in 5
different nodes: login1, login2, login3, login4 and login 5. As you can
see, this means jobs can be run directly upon log-in to the cluster
through three of the available login nodes: n4login1, n4login2 or
n4login3. If you prefer you can change to login4 or login5 by just
running **ssh login4** or **ssh login5**. However, these nodes are
intended for editing, compiling, preparation and submission of batch
executions so running time is limited to 10 minutes only.

If your job needs more than 10 minutes to run it needs to be done
through interactive allocation in the interactive partition or in a
compute node (main partition).

-   In an interactive partition:

    ```bash
    salloc -p interactive -A <UNIXGROUP> -q <queue>.
    ```

-   In a compute node:

    ```bash
    salloc -n1 -c2 -A <UNIXGROUP> -q <queue> #Example to request 1 task and 2 CPUs per task.
    ```

If you are not sure about what queues are available you can run the
command **bsc\_queues**.

![Alt text](./images/image15.png)

In the image above you can see there are 3 different queues available
for this user, their description and limitations.

If you start an interactive session in the interactive partition, you
will be re-directed to some of the interactive nodes (login1, login2,...
login5). Whereas in the main partition you will be re-directed to a
compute node.

In the following example we started two interactive jobs:

![Alt text](./images/image16.png)

The one with the JOBID 32597260 is running in the interactive partition
and node login4 was allocated. The job with JOBID 32597257 has been
launched in the main partition, but the user tried to allocate the job
in the interactive queue and therefore no node has been allocated since
this relation is not allowed.

![Alt text](./images/image17.png)

To run interactive sessions in the main partition queue parameter has to
be set to cli or debug like in the job 32597261. Notice it is already
running while 32597257 is still in pending (PD) state (ST).

Once you want to finish the interactive session just type **exit**.

## **Batch Submitting:**

NORD4 cluster uses Slurm (Simple Linux Utility for Resource Management)
as its job scheduler. Slurm organizes and allocates computing resources
in HPC clusters, ensuring fair and efficient usage.

-   Manages resources (nodes, tasks, CPUs, memory, time, etc.) across a
    computing cluster.

-   Schedules user-submitted jobs, determining when, where, and how they
    run based on available resources and priorities.

-   Handles job queues, prioritization, dependencies, and job arrays.

-   Enables efficient sharing of compute resources among multiple users
    and workloads.

A job is the basic unit of execution in Slurm. It is defined by a job
script (a text file containing a set of Slurm directives and shell
commands). The directives specify the job's resource requirements and are written as special comments in the script. These must follow a specific
syntax to correctly inform the system of the job\'s needs.

Jobs are submitted using the sbatch command-line tool. Sbatch does:

-   Accepts a job script containing Slurm directives and shell commands.

-   Submits the script to Slurm, which queues and schedules it for
    execution on available compute nodes.

-   Returns a job ID, which can be used to monitor or manage the job.

The basic sbatch syntax is:

```bash
    #SBATCH --directive=<value>
```

In most bioinformatic processes a single node will be enough for jobs to run. So, we recommend always setting \#SBATCH \--node=1 and only modify two main directives: cpus-per-task and time.

```bash
   #SBATCH --cpus-per-task=<value>

   #SBATCH --time=<hh:mm:ss>
```

A regular sbatch script would have the following pattern:

```bash
#!/bin/bash

#SBATCH --job-name=<job_name>           # Job name

#SBATCH --output=<job_name>.out         # Output file

#SBATCH --error=<job_name>.err          # Error file

#SBATCH --nodes=1                       # Number of nodes

#SBATCH --cpus-per-task=<value>         # Number of CPUs per task

#SBATCH --time=<hh:mm:ss>               # Time limit

#SBATCH --account=<UNIX_GROUP>          # UNIX GROUP

#SBATCH --qos=<queue>                   # queue where to run the job

module load <necessary modules>

<rest of the code>
```

Bear in mind that in NORD4 each CPU has approximately 2GB of RAM so
necessary CPUs can be calculated as (GB needed) / 2.

As an example, here's a sbatch script to call an Rscript called
my\_r\_script.R to run in a single NORD4 node with approximately 8GB of
RAM for 1.5 hours as maximum:

```bash
#!/bin/bash

#SBATCH --job-name=R_job            # Job name

#SBATCH --output=R_job.out          # Output file

#SBATCH --error=R_job.err           # Error file

#SBATCH --nodes=1                   # Number of nodes

#SBATCH --cpus-per-task=4           # Number of CPUs per task

#SBATCH --time=01:30:00             # Time limit (hh:mm:ss)

#SBATCH --account=idib57            # UNIX GROUP

#SBATCH --qos=cli                   # queue where job will run

module load gcc pcre2 R/4.3.0       # Load R module and dependencies.

Rscript path/to/my_R_script.R       # Execute the Rscript.
```

This sbatch script can be either saved as a .sh or .sbatch (for example:
R_job.sbatch) and the correct form to run it is:

```bash
sbatch R_job.sbatch
```

To see the status of the job just type **squeue** and it will appear
together with the JOBID. To cancel a job, whether it is running or not,
just type **scancel \<JOBID\>**.

NORD4 has 72 nodes with 96GB/node but if your job requires more RAM
there is the possibility to specify a high memory node (384GB7node i.e.
8GB per core). Add the following in the \#SBATCH directives:

```bash
#SBATCH --constraint=highmem
```

Requesting these nodes means the job will last longer in the queue (before start running).

# Rstudio in Nord4 server
=======================


**Private connection to compute node**

Create and save the following script as a bash (.sh extension) script.
For example: "Rstudio\_server.sh".

```bash
#!/bin/bash**

#SBATCH --job-name=rstudio_server

#SBATCH --output=rstudio_%j.out

#SBATCH --error=rstudio_%j.err

#SBATCH --nodes=1

#SBATCH --ntasks=1

#SBATCH --cpus-per-task=48

#SBATCH --account=idib57

#SBATCH --time=01:00:00

#SBATCH --exclusive

#SBATCH --qos=cli


# Load all required modules for you application here:

module load gcc/7.2.0 pcre2/10.39 boost/1.69.0 R/4.1.2 openssl/1.1.1o
RSTUDIO/1.3-rserver

# For RStudio server (default port: 8787)

rserver --www-address=127.0.0.1
```

Once you have it ready, open a terminal and run the script doing:

```bash
sbatch path/to/Rstudio_server.sh
```

This script is thought to reserve a whole compute node for your user.
Run squeue to see when it starts running and which node is reserved for
you.

```bash
squeue
```

![Alt text](./images/image18.png)

In the case above we got s01r2b39 reserved for us.

To complete the compute node reservation now we have to generate a unix
socket. This socket has to be private for us so a good practice would be
to first change to a private directory, for example, our /gpfs/home.
Then, run the following command with the compute node reserved:

```bash
ssh -L ./socket.sock:localhost:8787 <node_ID> # change the compute node
```

![Alt text](./images/image19.png)

Now a new file named socket.sock has been created in your private home
directory. If it's not the first time you do this remember to first
delete the socket.sock doing **rm socket.sock** prior to create it
again.

**Connection from local PC to login node**

First open a new terminal. It won't work in the same terminal above.
Don't connect into the cluster. You want to establish this connection
from your local machine to the socket.

Run the following command changing with your actual Starlife ID.

```bash
ssh -L 8787:/path/to/private/directory/socket.sock <user_name>@n4login1.bsc.es # change Starlife ID
```

![Alt text](./images/image20.png)

Now the port has been created. Open a web browser (Firefox or Chrome,
for example) and type:

**http://localhost:8787**

The Rstudio window will open.

Bear in mind that both connections have to be established with the same
login node. In the case above we launched the script when logged with
n4login1. Then we had to establish the connection from our local machine
to the socket with the same node. Otherwise, it wouldn't have worked.