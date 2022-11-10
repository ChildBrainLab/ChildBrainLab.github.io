In addition to the computational resources managed by NIL, we have access to resources managed by the Mallinckrodt Institute of Radiology's [Center for High Performance Computing](https://www.mir.wustl.edu/research/core-resources/center-for-high-performance-computing/). Keep in mind - this is a computing system fully distinct from the NIL resources, including DynoSparky and Moochie, and is structurally an entirely different animal. Whereas DynoSparky is a self-contained server we explicitly use, CHPC is a [computer cluster](https://en.wikipedia.org/wiki/Computer_cluster), meaning hundreds of computers are linked together to be used in parallel by many users running many different processes at once. Jobs are also not instantaneous, but rather submitted to a job queue, [SLURM](https://slurm.schedmd.com/documentation.html), and run in large batches concurrently, parellized so that, for example, each subject's processing has its own dedicated CPU. This is typically how large neuroimaging analysis jobs are  run in reasonable amounts of time. This level of parallelization is not possible on DynoSparky, though it is itself a powerful computer - HPC is like having a hundred DynoSparky's for a couple hours.

The MIR's HPC system is dedicated to in-vivo imaging, and currently we are allowed access through the Psychiatry Department. To request an account, you'll need to have completed your HIPAA training through [Learn@Work](http://www.learnatwork.wustl.edu/), after which you can submit an application [here](https://sites.wustl.edu/chpc/for-users/account-request/). Once accepted, familiarize yourself with the [Rules and Guidelines](https://sites.wustl.edu/chpc/for-users/rules-and-guidelines/) for accessing CHPC - there are many more nuances to computing through this system, and you should not jump in without being highly familiar with Unix systems, the SLURM job queue, and awareness of the rescrictions and relevant quotas.

For any lingering questions regarding CHPC, you can visit the [FAQ page](https://sites.wustl.edu/chpc/for-users/frequently-asked-questions-faq/) or email the director, [Xing Huang](x.huang@wustl.edu), who has been extremely helpful and kind in assisting the lab with this resource.

# Connecting to CHPC
Once your account request has been accepted, you can access CHPC. To access via a 'login node' via SSH, you will use the following syntax:


    ssh -Y <wustl_key>@login3.chpc.wustl.edu


and enter your WUSTL key password to complete authentication.

Note: login3 is the alias for the "3rd gen" of login nodes used by CHPC. These will connect you "round-robin" to the two login nodes, login3-01 and login3-02. Use of login2 or login1 are deprecated and will lead you to encounter problems.

To view your disk quotas on CHPC, enter the following command:


    check_user_quota


To view the job queue, you can enter `squeue` or `squeue -u wustlID` to view only the jobs you've submitted.
