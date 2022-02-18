![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure HPC OnDemand Platform
</div>

<div class="MCWHeader2">
Hands-on lab guide
</div>

<div class="MCWHeader3">
January 2022
</div>

<!-- TOC -->

- [Azure HPC OnDemand Platform lab guide](#azure-hpc-on-demand-platform-lab-guide)
   - [Requirements](#requirements)
      - [Exercise 1: Prepare for implementing Azure HPC OnDemand Platform environment](#exercise-1-prepare-for-implementing-azure-hpc-ondemand-platform-environment)
      - [Task 1: Deploy Azure Bastion](#task-2-deploy-azure-bastion)
   - [Exercise 2: Implement Azure HPC OnDemand Platform infrastructure](#exercise-2-implement-azure-hpc-ondemand-platform-infrastructure)
      - [Task 1: Customize infrastructure components](#task-1-customize-infrastructure-components)
      - [Task 2: Deploy Azure HPC OnDemand Platform cloud infrastructure](#task-2-deploy-azure-hpc-ondemand-platform-cloud-infrastructure)
      - [Task 3: Build images](#task-3-build-images)
      - [Task 4: Review deployment results](#task-4-review-deployment-results)
      - [Task 5: Generate passwords for user and admin accounts](#task-5-generate-passwords-for-user-and-admin-accounts)
   - [Exercise 3: Install and configure Azure HPC OnDemand Platform software components](#exercise-3-install-and-configure-azure-hpc-ondemand-platform-software-components)
      - [Task 1: Install Azure HPC OnDemand Platform software components](#task-1-install-azure-hpc-ondemand-platform-software-components)
      - [Task 2: Review installation results](#task-2-review-installation-results)
   - [Exercise 4: Validate functionality of Azure HPC OnDemand Platform](#exercise-4-validate-functionality-of-azure-hpc-ondemand-platform)
      - [Task 1: Create jobs based on the default Azure HPC OnDemand Platform template](#task-1-create-jobs-based-on-the-default-azure-hpc-ondemand-platform-template)
      - [Task 2: Create jobs based on a non-default Azure HPC OnDemand Platform template](#task-2-create-jobs-based-on-a-non-default-azure-hpc-ondemand-platform-template)
   - [Exercise 5: Deprovision Azure HPC OnDemand Platform environment](#exercise-5-deprovision-azure-hpc-ondemand-platform-environment)
      - [Task 1: Terminate the cluster](#task-1-terminate-the-cluster)
      - [Task 2: Deprovision the Azure resources](#task-2-deprovision-the-azure-resources)

<!-- /TOC -->

# Azure HPC OnDemand Platform lab guide

## Requirements

- A Microsoft Azure subscription
- A work or school account with the Owner role in the Azure subscription
- A lab computer with:

   - Access to Microsoft Azure
   - A modern web browser (Microsoft Edge, Google Chrome, or Mozilla Firefox)


## Exercise 1: Prepare for implementing Azure HPC OnDemand Platform environment

Duration: 10 minutes

In this exercise, you will set up an Azure VM that will be used for deployment of the lab environment. 

### Task 1: Log in to Azure Portal and verify the pre-deployed resources

1. In the virtual machine (VM) on the left, click on the Azure Portal as shown below.

   ![Azure Portal](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-Azure-Network-Solutions/main/media/gs4.png)
   
2. When you click on Azure portal, edge browser welcome screen will come up, select Start without your data.

   ![Start without data](https://github.com/CloudLabsAI-Azure/azure-spring-cloud-training/blob/master/media/get_started.png)
   
3. On next window, click on Confirm and start browsing.

   ![](https://github.com/CloudLabsAI-Azure/azure-spring-cloud-training/blob/master/media/confirm.png)
   
4. Now, you will see two tabs in edge browser, close first tab named with Microsoft Edge.

5. On Sign in to Microsoft Azure tab you will see login screen, in that enter following email/username and then click on Next.

    * Email/Username: <inject key="AzureAdUserEmail"></inject>
   
     ![](https://github.com/CloudLabsAI-Azure/azure-spring-cloud-training/blob/master/media/azure-login-enter-email.png)
     
1. Now enter the following password and click on **Sign in**.

   * Password: <inject key="AzureAdUserPassword"></inject>
   
     ![](https://github.com/CloudLabsAI-Azure/azure-spring-cloud-training/blob/master/media/azure-login-enter-password.png)
     
     

### Task 2: Deploy Azure Bastion 

> **Note**: Azure Bastion allows for connection to Azure VMs without relying on public endpoints, providing protection against brute force exploits that target operating system level credentials.


1. In the **Azure portal**, open the **Azure Cloud Shell** by clicking on the cloud shell icon in the top menu bar. Alternatively, you can open cloud shell by navigating to https://shell.azure.com 

    ![](../main/media/cloudshell.png)

2. After launching the Azure Cloud Shell, select the **Bash** option.

     ![](../main/media/bash.png)
     
3. Now on You have no storage mounted dialog box click on **Show advanced settings. Select Create new under Storage account and provide values as below:
 
    - **Cloud Shell region**: **Central US**
    - **Storage account** : **stacc-<inject key="DeploymentID" />**
    - **File Share** : **blob**

      ![](../main/media/create%20storage.png)
  
4. Run the following commands to add a subnet named **AzureBastionSubnet** to the virtual network that you created in the previous task:

      ```bash
         RGNAME='Azure-hpc-DID'
         VNETNAME='azcli-vnet'
         az network vnet subnet create --resource-group $RGNAME --vnet-name $VNETNAME --name AzureBastionSubnet --address-prefixes 192.168.3.0/24
      ```

   > **Note :** Replace the DID with the value **<inject key="DeploymentID" />**


1. Close the Cloud Shell pane.

1. Type **Bastions (1)** in the search box of the Azure portal menu, and select **Bastions (2)**.

     ![](../main/media/bastions.png)
     
4. On the **Bastions** blade, select **+ Create Bastion**.

     ![](../main/media/create%20bastion.png)
     
6. On the **Basic** tab of the **Create a Bastion** blade, specify the following settings and select **Review + create**:

   |Setting|Value|
   |---|---|
   |Subscription|the name of the Azure subscription you are using in this lab|
   |Resource group|**Azure-hpc-<inject key="DeploymentID" />**|
   |Name|**azure-hpc-bastion**|
   |Region|the same Azure region to which you used for deployment in the previous task of this exercise|
   |Tier|**Basic**|
   |Virtual network|**azcli-vnet**|
   |Subnet|**AzureBastionSubnet (192.168.3.0/24)**|
   |Public IP address|**Create new**|
   |Public IP name|**azhop-cli-pip**|
   |Review + Create| Click on **Review + create**|
   
    ![](../main/media/re%2Bcr.png)
   
7. Review the configuration and click on **Create**.

   ![](../main/media/create.png)

   > **Note**: Wait for the deployment to complete before you proceed to the next exercise. The deployment might take about 5 minutes.


### Task 3: Connect to the VM

> **Note**: Ensure that your browser has the pop-up functionality enabled before you attempt to connect via Azure Bastion.

1. Type **Virtual machines (1)** in the search box of **Azure Portal** and select **Virtual machines (2)**.

     ![](../main/media/virtual%20machines.png)
     
1. On the **Virtual machines** page, select **azcli-vm0**.

    ![](../main/media/selectvm.png)
    
1. On the **azcli-vm0** blade, select **Connect** and click on **Bastion** from the drop-down menu.

   ![](../main/media/connect.png) 
   
1. On **Bastion** page, enter the below details:

    - **User name** : **azureadm (1)**
    - **Password** : **Password.1!! (2)**
    - Click on **Connect (3)**

     ![](../main/media/connectbastion.png)


## Exercise 2: Implement Azure HPC OnDemand Platform cloud infrastructure

Duration: 90 minutes

In this exercise, you will deploy the Azure HPC OnDemand Platform infrastructure.

### Task 1: Customize infrastructure components

An az-hop environment is defined by using a configuration file named **config.yml**, which needs to reside in the root of the repository. The simplest way to create is by cloning the template file **config.tpl.yml** and modifying the content of the copied file. In this task, you will review its content and, if needed, set the target Azure region for deploying the Azure HPC OnDemand Platform infrastructure.

1. On the lab computer, in the browser window displaying the Azure portal, within the SSH session to the Azure VM, run the following commands to copy **config.tpl.yml** to **config.yml**

   ```bash
   cp config.tpl.yml config.yml
   ```

1. Open the **config.yml** file using your preferred editor (such as Nano or vi)

   ```bash
   vi config.yml
   ```

1. Review the content of the **config.yml** file and note that it includes the following settings:

   - **location**: the name of the target Azure region
   - **resource_group**: the name of the target resource group (you do have the option of using an existing resource group by setting **use_existing_rg** to **true**)
   - **anf**: configuration properties of Azure NetApp Files resources
   - **admin_user**: the name of the admin user account (its random password is autogenerated and stored in the Azure Key vault provisioned as part of the deployment
   - **network**: configuration properties of the Azure virtual network into which the Azure resources hosting the infrastructure resources are deployed
   - **jumpbox**, **ad**, **ondemand**, **grafana**, **scheduler**, **cyclecloud**, **winviz**, and **lustre**: configuration properties of Azure VMs hosting the infrastructure components
   - **users**: user accounts autoprovisioned as part of the deployment
   - **queue_manager**: the name of the scheduler to be installed and configured (**openpbs** by default)
   - **authentication**: the authentication method (**basic** by default, but you have the option of using OpenID Connect instead)
   - **images**: the list of images that will be available for deployment of compute cluster nodes and their respective configuration
   - **queues**: the list of node arrays of CycleCloud and their respective configuration

1. If needed, change the name of the target Azure region set in the **config.yml** file to the one you are using in this lab, save the change, and close the file.

### Task 2: Deploy Azure HPC OnDemand Platform infrastructure

1. Within the SSH session to the Azure VM, run the following command to generate a Terraform deployment plan that includes the listing of all resources to be provisioned:

   ```bash
   ./build.sh -a plan
   ```

1. Review the generated list of resources and then run the following command to trigger the deployment of the Azure HPC OnDemand Platform infrastructure:

   ```bash
   ./build.sh -a apply
   ```

   > **Note**: Wait for the deployment to complete. This should take about 20 minutes. Once the deployment completes, you should see the message stating **Apply complete! Resources: 124 added, 0 changed, 0 destroyed.**


### Task 3: Build images

The az-hop solution provides pre-configured Packer configuration files that can be used to build custom images. The utility script **./packer/build_image.sh** performs the build process and stores the resulting images in the Shared Image Gallery included in the deployed infrastructure.

1. On the lab computer, in the browser window displaying the Azure portal, within the SSH session to the Azure VM, run the following commands to build a custom image based on the **azhop-centos79-v2-rdma-gpgpu.json** configuration file.

   > **Note**: The image creation process based on the **azhop-centos79-v2-rdma-gpgpu.json** configuration file relies on a **Standard_d8s_v3** SKU Azure VM.

   ```bash
   cd packer/
   ./build_image.sh -i azhop-centos79-v2-rdma-gpgpu.json
   ```

   > **Note**: Disregard any warning messages.

   > **Note**: Wait for the process to complete. This might take about 20 minutes.

   > **Note**: The image creation process based on the **centos-7.8-desktop-3d.json** configuration file relies on a **Standard_NV6** SKU Azure VM. If you managed to obtain sufficient number of quotas to provision a **Standard_NV6**-based Azure VM, you can proceed directly to the next step. Otherwise, open the file **centos-7.8-desktop-3d.json**, replace the entry **Standard_NV6** with **Standard_D8s_v3**, save the change, and close the file. While such step is likely to affect the functionality of compute resources based on this image, it is meant strictly as a workaround that facilitates the next step in the process of implementing the Azure HPC OnDemand Platform lab environment.

1. Within the SSH session to the Azure VM, run the following command to build a custom image based on the **centos-7.8-desktop-3d.json** configuration file.

   ```bash
   ./build_image.sh -i centos-7.8-desktop-3d.json
   ```

   > **Note**: Wait for the process to complete. This might take about 40 minutes.


### Task 4: Review deployment results

1. On the lab computer, in the browser window displaying the Azure portal, open another tab, navigate to the Azure portal, use the **Search resources, services, and docs** text box to search for **Azure compute galleries** and, in the list of results, select **Azure compute galleries**.
1. On the **Azure compute galleries** blade, select the entry which name starts with the prefix **azhop** and then, on the compute gallery blade, verify that the gallery includes two VM image definitions. 
1. In the Azure portal, use the **Search resources, services, and docs** text box to search for **Azure virtual machines** and, in the list of results, select **Azure virtual machines**.
1. On the **Azure virtual machines** blade, review the listing of the provisioned virtual machines.

   > **Note**: If needed, filter the listing of the virtual machines by setting the resource group criterion to **azhop**.

1. Close the newly opened browser tab displaying the **Azure virtual machines** blade in the Azure portal.

### Task 5: Generate passwords for user and admin accounts

1. On the lab computer, in the browser window displaying the Azure portal, within the SSH session to the Azure VM, run the following command to generate the password for the **clusteradmin** and **clusteruser** accounts defined in the **config.yml** configuration file:

   ```bash
   cd ~/az-hop/
   ./create_passwords.sh
   ```

1. Run the following command to display the newly generated password of the **clusteradmin** account:

   ```bash
   ./bin/get_secret clusteradmin
   ```

   > **Note**: Record the password. You will need it later in this lab.

   > **Note**: The **./bin/get_secret utility script** retrieves the password of the user you specify as its parameter from the Azure Key vault which is part of the Azure HPC OnDemand Platform infrastructure you deployed in the previous exercise.

## Exercise 3: Install and configure Azure HPC OnDemand Platform software components

Duration: 60 minutes

In this exercise, you will install and configure software components that form the Azure HPC OnDemand Platform solution.

> **Note**: The installation is performed by using Ansible playbooks and supports setup of individual components as well as setup of the entire solution. In either case, it is necessary to account for dependencies between components. The setup script **install.sh** in the root directory of the repository performs the installation in the intended order. The components include: **ad**, **linux**, **add_users**, **lustre**, **ccportal**, **cccluster** (this component requires that custom images are present in the compute gallery), **scheduler**, **ood**, **grafana**, **telegraf**, and **chrony**.

### Task 1: Install and configure Azure HPC OnDemand Platform software components

1. On the lab computer, in the browser window displaying the Azure portal, within the SSH session to the Azure VM, run the following command to invoke the installation of the Azure HPC OnDemand Platform software components:

   ```bash
   cd ~/az-hop/
   ./install.sh
   ```

   > **Note**: Wait for the process to complete. This might take about 30 minutes.

   > **Note**: In case of a transient failure, the install script can be rerun since most of the settings are idempotent. In addition, the script has a checkpointing mechanism that creates component-specific files with the **.ok** extension in the playbooks directory and check for their presence during subsequent runs. If you want to re-apply the configuration to an existing component, delete the corresponding **.ok** file and rerun the install script.

### Task 2: Review installation results

1. Once the installation completes, on the lab computer, in the browser window displaying the Azure portal, within the SSH session to the Azure VM, run the following command to display the URL of the Azure HPC On-Demand Platform portal:

   ```bash
   grep ondemand_fqdn playbooks/group_vars/all.yml
   ```

   > **Note**: Record this value. You will need it later in this lab.

1. From the lab computer, start a web browser, navigate to the URL of the Azure HPC On-Demand Platform portal you identified earlier in this task and, when prompted, sign in with the **clusteradmin** user account and its password you identified in the previous step.

   > **Note**: You will be presented with the **Azure HPC On-Demand Platform** dashboard. Review its interface, starting with the top level menu, which provides access to **Apps**, **Files**, **Jobs**, **Clusters**, **Interactive Apps**, **Monitoring**, and **My Interactive Sessions** menu items.

1. In the **Monitoring** menu, select **Azure CycleCloud** and, when presented with the page titled **App has not been initialized or does not exist**, select **Initialize App**. 

   > **Note**: This prompt is a reflection of the architecture of the OnDemand component, which the Azure HPC OnDemand Platform solution relies on to implement its portal. The shared front-end creates Per User NGINX (PUN) processes to provide connectivity to such components as **Azure CycleCloud**, **Grafana**, or **Robinhood Dashboard**. 

1. On the **Azure CycleCloud for Azure HPC On-Demand Platform** page, note that presents a configuration of a cluster named **pbs1**. 
1. On the **pbs1** page, select the **Arrays* tab and note that it contains six entries representing queue definitions you reviewed earlier in the **config.yml** file. 


## Exercise 4: Validate functionality of Azure HPC OnDemand Platform

Duration: 30 minutes

In this exercise, you will validate functionality of Azure HPC OnDemand Platform.

### Task 1: Create jobs based on the default Azure HPC OnDemand Platform template

1. On the lab computer, in the browser window displaying the Azure HPC On-Demand Platform portal, navigate to the Dashboard page, select **Jobs** menu title and, in the drop-down menu, select **Job Composer**. 
1. On the **Jobs** page, select **+ New job** and, in the drop-down menu, select **From Default Template**.

   > **Note**: This will automatically create a job named **(default) Sample Sequential Job** that targets the **execute** CycleCloud array. To identify the content of the job script, ensure that the newly created job is selected and review the **Script contents** pane.

1. Repeat the previous step twice to create two additional jobs based on the default template.

   > **Note**: The default job template contains a trivial script that runs `echo "Hello World"`.

1. Note that all three jobs are currently in the **Not Submitted** state. To submit them, select each one of them and select **Submit**. 

   > **Note**: The status of the jobs should change to **Queued**.

1. On the lab computer, in the browser window displaying the Azure HPC On-Demand Platform portal, select the **Azure HPC On-Demand Platform** header, select the **Monitoring** menu, and, in the drop-down list, select **Azure CycleCloud**.
1. On the **Azure CycleCloud for Azure HPC On-Demand Platform** portal, monitor the status of the cluster and note that the number of nodes increases to **3**, which initially are listed in the **acquiring** state.
1. On the **Nodes** tab, verify that **execute** appears in the **Template** column, the **Nodes** column contains the entry **3**, and the **Last status** column displays the **Creating VM** message.
1. On the **pbs1** page of the **Azure CycleCloud for Azure HPC On-Demand Platform** portal, select **Scalesets** tab and note a scaleset that hosts the cluster nodes with its size set to **3**.
1. Select the entry on the **Nodes** tab and review the details of the cluster nodes in the lower section of the page, including the name of each node, their status, the number of cores, and the placement group.
1. Navigate back to the **Azure HPC On-Demand Platform** portal, select the **Jobs** menu, and, in the drop-down menu, select **Active jobs**.
1. On the **Active jobs** page, verify that there are three active jobs listed in the **Queued** status.
1. Navigate back to the **Azure CycleCloud for Azure HPC On-Demand Platform** portal and monitor the progress of node provisioning.

   > **Note**: Wait until the status of nodes changes to **Ready**. This should take about 5 minutes.

1. Once the nodes are listed with the **Ready** status, switch back to the **Active jobs** page, refresh it, and note that the jobs are no longer listed there.

   > **Note**: If the jobs are still listed as queued, wait for a few minutes and refresh the page again. 

1. Navigate back to the **Azure CycleCloud for Azure HPC On-Demand Platform** portal and monitor the node status until it changes to terminating, resulting eventually in their deletion. 
1. On the **pbs1** page of the **Azure CycleCloud for Azure HPC On-Demand Platform** portal, select **Scalesets** tab and note that the scaleset hosting the cluster nodes persists but its size is set to **0**.

### Task 2: Create jobs based on a non-default Azure HPC OnDemand Platform template

1. On the lab computer, in the browser window, switch back to the **Azure HPC On-Demand Platform** portal, select the **Jobs** menu, and, in the drop-down menu, select **Job Composer**. 
1. On the **Jobs** page, select **Templates**.
1. On the **Templates** page, in the list of predefined templates, select the **Intel MPI PingPong** entry and then select **Copy Template**.

   > **Note**: The Message Passing Interface (MPI) ping-pong tests measure network latency and throughput between nodes on the cluster by sending packets of data back and forth between paired nodes repeatedly. The latency is the average of half of the time that it takes for a packet to make a round trip between a pair of nodes, in microseconds. The throughput is the average rate of data transfer between a pair of nodes, in MB/second. 

1. On the **New Template** page, specify the following settings and select **Save**:

   |Setting|Value|
   |---|---|
   |Name|**Intel MPI PingPong v2**|
   |Cluster|**AZHOP - Cluster**|
   |Name|**<p>A very basic template for running Intel MPI ping pong on hb120v2</p>**|

1. Back on the **Templates** page, select the newly created template and then select **View Files**.
1. On the page listing the files that are part of the template, in the **pingpong.sh** row, select the square icon containing the vertical ellipsis symbol and then, in the drop-down menu, select **Edit**.
1. On the page displaying the content of the **pingpong.sh** script, in the third line, replace `slot_type=hb120v3` with `slot_type=hb120v2` and select **Save**.
1. Navigate back to the **Jobs** page, select **+ New job**, in the drop-down menu, select **From Template**, on the **Templates** page, ensure that the **Intel MPI PingPong v2** entry is selected, and then select **Create New Job**.

   > **Note**: This will automatically create a job named **Intel MPI PingPong v2** that targets the **hb120v2** CycleCloud array. 

1. Repeat the previous step twice to create two additional jobs based on the **Intel MPI PingPong v2** template.
1. Note that, as before, all three jobs are currently in the **Not Submitted** state. To submit them, select each one of them and select **Submit**. 

   > **Note**: The status of the jobs should change to **Queued**.

1. Navigate to the **Azure CycleCloud for Azure HPC On-Demand Platform** portal and monitor the progress of node provisioning.

   > **Note**: Wait until the status of nodes changes to **Ready**. This should take about 5 minutes.

1. Once the nodes are listed with the **Ready** status, switch back to the **Active jobs** page, refresh it, and note that the jobs are no longer listed there.

   > **Note**: If the jobs are still listed as queued, wait for a few minutes and refresh the page again. 

1. Navigate back to the **Azure CycleCloud for Azure HPC On-Demand Platform** portal and monitor the node status until it changes to terminating, resulting eventually in their deletion. 
1. On the **pbs1** page of the **Azure CycleCloud for Azure HPC On-Demand Platform** portal, select **Scalesets** tab and note that the scaleset hosting the cluster nodes persists but its size is set to **0**.
1. To review the output of the job, switch back to the **Azure HPC On-Demand Platform** portal, select the **Jobs** menu, and, in the drop-down menu, select **Job Composer**. 
1. On the **Jobs** page, select any of the **Intel MPI PingPong v2** job entries and, in the **Folder Contents** section, select **PingPong.o3**. 

   > **Note**: This will automatically open another web browser tab displaying the output of the job.

## Exercise 5: Deprovision Azure HPC OnDemand Platform environment

Duration: 30 minutes

In this exercise, you will deprovision the Azure HPC OnDemand Platform lab environment.

### Task 1: Terminate the cluster

1. On the lab computer, in the browser window, navigate back to the **Azure CycleCloud for Azure HPC On-Demand Platform** portal.
1. On the **pbs1** page, select **Terminate** and, when prompted for confirmation, select **OK**.
1. Monitor the progress of terminating the cluster.

   > **Note**: Ensure that all nodes and scaleset are deleted before you proceed to the next step.

### Task 2: Deprovision the Azure resources

1. On the lab computer, switch to the browser window displaying the Azure portal and start a Bash session in **Cloud Shell**.
1. From the Bash session in the Cloud Shell pane, run the following command to trigger the deprovisioning of the Azure HPC OnDemand Platform infrastructure you created and evaluated in this lab :

   ```bash
   az group delete --name azhop --yes
   ```

   > **Note**: Wait for the resource deprovisioning to complete. This should take about 20 minutes.
