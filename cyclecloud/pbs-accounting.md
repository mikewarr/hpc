# Azure CycleCloud - PBS Pro Accounting

There is a facility to query jobs which is installed by default with CycleCloud, however there will be a need to manipulate the data to obtain the required information. 

## Accounting Log File
Firstly, there’s an Accounting file created per day, which can be found on the master node in “/var/spool/pbs/server_priv/accounting”, you’ll need to be root to access this and to export information:

More about the log file can be found here on Page 615 - [PBS Pro Admin Guide](https://www.altair.com/pdfs/pbsworks/PBSAdminGuide2020.1.pdf)

## Extracting Accounting Information
To extract information from the file, using one of the native tools, you’ll need to be root to ensure all information is available otherwise you’ll see a subset of data. 

“tracejob” is a tool that will extract information for a single job, if you need to collect information across many different jobs then that will need a bespoke script or application to do that. The output from tracejob shows a lot of information about how a job is run, which is essentially output from the accounting log file mixed with other log information. More information on “tracejob” can be found here: Page 245 - [PBS Reference Guide](https://www.altair.com/pdfs/pbsworks/PBSReferenceGuide2020.1.pdf)


> [mikewarr@ip-0A020205 demo]$ sudo su
> [root@ip-0A020205 demo]# tracejob 6
> 
> Job: 6.ip-0A020205
> 
> 01/05/2021 13:09:04  S    enqueuing into workq, state 2 hop 1
> 01/05/2021 13:09:04  S    Job Queued at request of mikewarr@ip-0a020205, owner = mikewarr@ip-0a020205, job name = pitest, queue = workq
> 01/05/2021 13:09:04  A    queue=workq
> 01/05/2021 13:09:16  S    Job Modified at request of root@ip-0a020205
> 01/05/2021 13:09:16  S    Holds so released at request of root@ip-0a020205
> 01/05/2021 13:16:46  L    Considering job to run
> 01/05/2021 13:16:46  L    Insufficient amount of resource: ncpus (R: 1 A: 0 T: 0)
> 01/05/2021 13:16:46  S    Job Modified at request of Scheduler@ip-0a020205
> 01/05/2021 13:16:46  L    Job will never run with the resources currently configured in the complex
> 01/05/2021 13:17:01  L    Considering job to run
> 01/05/2021 13:17:01  S    Job Modified at request of Scheduler@ip-0a020205
> 01/05/2021 13:17:01  S    Job Run at request of Scheduler@ip-0a020205 on exec_vnode (ip-0A020206:ncpus=1)
> 01/05/2021 13:17:01  L    Job run
>
> 01/05/2021 13:17:01  A    user=mikewarr group=mikewarr account="MicrosoftProject" project=_pbs_project_default jobname=pitest queue=workq ctime=1609852144 qtime=1609852144 etime=1609852156 start=1609852621 exec_host=ip-0A020206/0 exec_vnode=(ip-0A020206:ncpus=1) Resource_List.ncpus=1 Resource_List.nodect=1  Resource_List.place=pack:group=group_id Resource_List.select=1:ncpus=1:ungrouped=false Resource_List.ungrouped=false resource_assigned.ncpus=1
> 
> 01/05/2021 13:17:12  S    Obit received momhop:1 serverhop:1 state:4 substate:42
>
> 01/05/2021 13:17:13  S    Exit_status=0 resources_used.cpupercent=100 resources_used.cput=00:00:11 resources_used.mem=9848kb resources_used.ncpus=1 resources_used.vmem=356456kb resources_used.walltime=00:00:11
> 
> 01/05/2021 13:17:13  S    dequeuing from workq, state 5
>
> 01/05/2021 13:17:13  A    user=mikewarr group=mikewarr account="MicrosoftProject" project=_pbs_project_default jobname=pitest queue=workq ctime=1609852144 qtime=1609852144 etime=1609852156 start=1609852621 exec_host=ip-0A020206/0 exec_vnode=(ip-0A020206:ncpus=1) Resource_List.ncpus=1 Resource_List.nodect=1 Resource_List.place=pack:group=group_id Resource_List.select=1:ncpus=1:ungrouped=false Resource_List.ungrouped=false session=5265 end=1609852633 Exit_status=0 resources_used.cpupercent=100 resources_used.cput=00:00:11 resources_used.mem=9848kb resources_used.ncpus=1 resources_used.vmem=356456kb resources_used.walltime=00:00:11 run_count=1


We can get the same information direct from the accounting log as well:

> 01/05/2021 13:17:13;E;6.ip-0A020205;user=mikewarr group=mikewarr account="MicrosoftProject" project=_pbs_project_default jobname=pitest queue=workq ctime=1609852144 qtime=1609852144 etime=1609852156 start=1609852621 exec_host=ip-0A020206/0 exec_vnode=(ip-0A020206:ncpus=1) Resource_List.ncpus=1 Resource_List.nodect=1 Resource_List.place=pack:group=group_id Resource_List.select=1:ncpus=1:ungrouped=false Resource_List.ungrouped=false session=5265 end=1609852633 Exit_status=0 resources_used.cpupercent=100 resources_used.cput=00:00:11 resources_used.mem=9848kb resources_used.ncpus=1 resources_used.vmem=356456kb resources_used.walltime=00:00:11 run_count=1

I’ve highlighted the interesting points in the output, one of which is called “account”. This is a parameter that can be used during the job submission to provide a freeform filter, that can be used as follows:

> qsub -A MicrosoftProject pi.sh

The Accounting string is referenced here in the documentation: Page 39 - [PBS User Guide](https://www.altair.com/pdfs/pbsworks/PBSUserGuide2020.1.pdf)

Full PBS Pro documentation can be found here: https://www.altair.com/pbs-works-documentation/

