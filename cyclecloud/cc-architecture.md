# Azure CycleCloud Architecture Considerations

## Azure CycleCloud Deployment

-   Define which version of CycleCloud will be deployed:
    -   [Azure CycleCloud 8.1 - Current Release](https://docs.microsoft.com/en-us/azure/cyclecloud/release-notes?view=cyclecloud-8)
    -   [Azure CycleCloud 7.9 - Previous Release](https://docs.microsoft.com/en-us/azure/cyclecloud/release-notes-previous?view=cyclecloud-8)
-   Define which Subscription, vNet, Subnet and Resource Group the CycleCloud server will be deployed to
    -   [Prepare your Azure Subscription](https://docs.microsoft.com/en-in/azure/cyclecloud/how-to/configuration?view=cyclecloud-8)
-	Define which Resource Group will host any clusters or if CycleCloud can create them on an adhoc basis (default setting) 
-	Create a storage account for locker access
-	Determine if SSH keys, AD or LDAP will be used for authentication
-	Define if CycleCloud will use a Service Principal or a Managed Identity (recommended with a single subscription) [Choosing between a Service Principal and a Managed Indentity](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/service-principals?view=cyclecloud-7#choosing-between-a-service-principal-and-a-managed-identity)
-	Confirm which SKU will be used for CycleCloud: System Requirements: [CycleCloud System Requirements](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/install-manual?view=cyclecloud-8#system-requirements)
-   Will the environment be deployed in a locked down network? If so, take into account the following requirements: [Operating in a locked down network](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/running-in-locked-down-network?view=cyclecloud-8)
-	Deploy the CycleCloud server

## Configuration of Azure CycleCloud
-	Login to the CycleCloud server, create a site and an admin account for CycleCloud: [CycleCloud setup](https://docs.microsoft.com/en-us/azure/cyclecloud/qs-install-marketplace?view=cyclecloud-8#log-into-the-cyclecloud-application-server)
-	Create the locker which points to the storage account created [CycleCloud Lockers](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/storage-blobs?view=cyclecloud-8#lockers)

## Cluster Configuration for Azure CycleCloud
-   Define user access to the clusters [Cluster User Management](https://docs.microsoft.com/en-in/azure/cyclecloud/how-to/user-access?view=cyclecloud-8)	
-   Determine which scheduler will be used
-   Determine which SKU will be required for the scheduler/master node
-	Determine what SKUs will be required for the Compute/Execute nodes, this will be entirely dependent on the Application being run 
-	Will clusters be deployed using a template or manually? 
    -   Cluster templates will need to be defined and uploaded to the locker: [Cluster Template Reference](https://docs.microsoft.com/en-us/azure/cyclecloud/cluster-references/cluster-template-reference?view=cyclecloud-8)
    -   Manual creation: [Create a New Cluster](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/create-cluster?view=cyclecloud-8)
-	Will any scripts need to be run on the master or execute nodes once deployed: 
    -   [Cluster-Init](https://docs.microsoft.com/en-us/azure/cyclecloud/cluster-references/cluster-init-reference?view=cyclecloud-7)
    -   [Cloud-Init](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/cloud-init?view=cyclecloud-8)

## Applications
-	What dependencies do the applications have i.e. libraries etc., how will these be made available
-	How long does an application take to setup, this might determine how an application is made available to the execute nodes
-	Determine where applications will be executed from, this will be dependent on install times and performance requirements:
    -   Through a custom image: 
        -   [Custom Images in a CycleCloud Cluster](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/create-custom-image?view=cyclecloud-8)
        -   [Create a Customer Linux Image](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-custom-images)
    -   Using a marketplace image
    -   From an NFS share, blob storage, Azure NetApp Files
-   Is there a specific SKU which will need to be used for the Applications to run on? i.e. will MPI be a requirement as that would necessitate a different family of machines like the H series:
    -   [Azure VM sizes - HPC](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-hpc)
    -   [HB/HC Cluster Best Practices](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/hb-hc-best-practices?view=cyclecloud-8)
-   What will be the optimum number of cores per job for each application?
-   Ensure subscription quotas are in place to fulfil the core requirements for the applications

## Data
-	Confirm where the input data resides and how it will be made available in Azure in a timely manner
-	Confirm where in Azure the input data will reside, this will be a variable in the performance of the applications i.e. locally on the execute nodes, from an NFS share, using blob storage, using Azure NetApp Files
-	Confirm if there is any post-processing needed on the output data
-	Confirm where the output data will reside once processing is complete
    - Does it need to be copied elsewhere?
    - What archive/backup requirements are there? 

## Job Submission
-	How will users submit jobs?
-	Will they have a script to run on the master node or will there be a frontend to help with data upload and job submission?

## Cluster Deployment
-	Deploy the desired configuration as per the requirements i.e. manually / template based
-	Confirm the applications run as expected

## Backup and Disaster Recovery
-	Will templates be used for cluster creation? This will make the recreation of a CycleCloud server a lot quicker and consistent across deployments
-	What requirements for DR are there? i.e. what would happen to the business if an Azure region wasn’t available as expected
-	Are there any SLA’s defined for the applications used by the internal business?
-	Could another region be used as a standby? 

