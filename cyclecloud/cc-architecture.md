# CycleCloud Architecture Considerations

## CycleCloud Deployment

-	Confirm which Subscription, vNet, Subnet and Resource Group the CycleCloud server will be deployed to
-	Confirm which Resource Group will host any clusters or if CycleCloud can create them on an adhoc basis (default setting)
-	Create a storage account for locker access
-	Determine if SSH keys, AD or LDAP will be used for authentication
-	Determine if CycleCloud will use a Service Principal or a Managed Identity (recommended with a single subscription) Using a Service Principal - Azure CycleCloud | Microsoft Docs
-	Confirm which SKU will be used for CycleCloud: System Requirements: CycleCloud Manual Installation - Azure CycleCloud | Microsoft Docs
-	Deploy the CycleCloud server

## CONFIGURATION OF CYCLECLOUD
-	Login to the CycleCloud server, create a site and an admin account for CycleCloud: Quickstart - Install via Marketplace - Azure CycleCloud | Microsoft Docs
-	Create the locker which points to the storage account created

## CLUSTER CONFIGURATION
-	Determine which scheduler will be used
-   Determine which SKU will be required for the scheduler/master node
-	Determine what SKUs will be required for the Compute/Execute nodes 
-	Will clusters be deployed using a template or manually? 
o	Cluster templates will need to be defined and uploaded to the locker: Cluster Template Reference Overview - Azure CycleCloud | Microsoft Docs
o	Manual creation: Create a Cluster - Azure CycleCloud | Microsoft Docs
-	Will any scripts need to be run on the master or execute nodes once deployed: How to use Cloud-Init with CycleCloud - Azure CycleCloud | Microsoft Docs

## APPLICATIONS
-	What dependencies do the applications have i.e. libraries etc., how will these be made available
-	How long does an application take to setup, this might determine how an application is made available to the execute nodes
-	Determine where applications will be executed from, this will be dependent on install times and performance requirements:
        Through a custom image: Custom Images - Azure CycleCloud | Microsoft Docs
        Using a marketplace image
        From an NFS share, blob storage, Azure NetApp Files
-   Is there a specific SKU which will need to be used for the Applications to run on? i.e. will MPI be a requirement as that would necessitate a different family of machines like the H series:
        Azure VM sizes - HPC - Azure Virtual Machines | Microsoft Docs
        HB/HC Cluster Best Practices - Azure CycleCloud | Microsoft Docs
-   What will be the optimum number of cores per job for each application?
-   ACTION: Ensure subscription quotas are in place to fulfil the core requirements for the applications

## DATA
-	Confirm where the input data resides and how it will be made available in Azure in a timely manner
-	Confirm where in Azure the input data will reside, this will be a variable in the performance of the applications i.e. locally on the execute nodes, from an NFS share, using blob storage, using Azure NetApp Files
-	Confirm if there is any post-processing needed on the output data
-	Confirm where the output data will reside once processing is complete
o	Does it need to be copied elsewhere?
o	What archive/backup requirements are there? 

## JOB SUBMISSION
-	How will users submit jobs?
-	Will they have a script to run on the master node or will there be a frontend to help with data upload and job submission?

## CLUSTER DEPLOYMENT
-	Deploy the desired configuration as per the requirements i.e. manually / template based
-	Confirm the applications run as expected

## BACKUP / DR 
-	Will templates be used for cluster creation? This will make the recreation of a CycleCloud server a lot quicker and consistent across deployments
-	What requirements for DR are there? i.e. what would happen to the business if an Azure region wasn’t available as expected
-	Are there any SLA’s defined for the applications used by the internal business?
-	Could another region be used as a standby? 

