# Azure Container Service with DC / OS Lab Setup

> LAST UPDATED: 5/6/2018

This document describes all of the preparation you should take care of prior to the workshop.
If you do not have time, you may work on this during the workshop but may not complete as many lab steps
during lab times and can work on your own time to complete the lab at your leisure.

## Prerequisites

The lab will make use of several tools and services.  Here is a list of requirements that you will need to successfully complete the lab.  If you are missing any of these requirements, we will cover the steps to satisfy them in this document.

* An internet connection.
* Microsoft Azure subscription must be pay-as-you-go or MSDN.
  * Trial subscriptions will not work.
  * You must have enough cores available in your subscription to create the build agent and Azure Container Service cluster in Task 5: Create a build agent VM and Task 9: Create an Azure Container Service cluster. You’ll need eight cores if following the exact instructions in the lab, more if you choose additional agents or larger VM sizes. If you execute the steps required before the lab, you will be able to see if you need to request more cores in your sub.
* Local machine or a virtual machine configured with:
  * A browser, preferably Chrome for consistency with the lab implementation tests
  * Command prompt
    * On Windows, you will be using Bash on Ubuntu on Windows, hereon referred to as WSL.
    * On Mac, all instructions should be executed using bash in Terminal.

> VERY IMPORTANT: You should be typing all of the commands as they appear in this document, except where explicitly stated. Do not try to copy and paste as there can be issues that result in errors, execution of instructions, or creation of file content.

## Preparation Steps in this Document

**Create your Azure Account**

**Create a cluster and supporting tooling / setup**

* Task 1: Create a new Resource Group
* Task 2: Create a Windows 10 Development VM (optional)
* Task 3: Install WSL (Bash on Ubuntu on Windows)
* Task 4: Create an SSH key
* Task 5: Create a build agent VM
* Task 6: Connect securely to the build agent
* Task 7: Complete the build agent setup
* Task 8: Create a Docker Hub account
* Task 9: Create an Azure Container Service cluster
* Task 10: Download the sample source

## Create Azure Account

1. Microsoft Azure subscription must be pay-as-you-go or MSDN. **(Trial subscriptions will not work)**

2. First login with your Microsoft Account then go to this link [https://azure.microsoft.com/en-us/pricing/purchase-options](https://azure.microsoft.com/en-us/pricing/purchase-options)

    ![Azure](images/image_001.png)

3. Then you will be directed to Azure sign up page where you have to provide your details

    ![Azure](images/image_002.png)

4. Once you fill your details and push purchase you will be directed to this page

    ![Azure](images/image_003.png)

5. Click on Get Started With your Azure subscription.

6. You are redirected to Azure portal

    ![Azure](images/image_004.png)

## Before the Workshop Setup

**Duration:** 1 hour ***(possibly additional time if Azure provisioning is slower)***

You should follow all of the steps provided in Exercise 0 before attending the lab.

### Task 1: Create a new Resource Group

You will create an Azure Resource Group to hold most of the resources that you create in this lab.
This approach will make it easier to clean up later. You will be instructed to create new resources in this Resource Group during the remaining exercises.

1. In your browser, navigate to the Azure Portal ([https://portal.azure.com](https://portal.azure.com)).

2. Select + Create a resource in the navigation bar at the left.

    ![Microsoft Azure](images/ex0-task1-image_01.png)

3. In the Search the Marketplace search box, type "Resource group" and press Enter.

    ![Microsoft Azure](images/ex0-task1-image_02.png)

4. Select Resource group on the Everything blade and select Create.

    ![Microsoft Azure](images/ex0-task1-image_03.png)

5. On the new Resource group blade, set the following:

    * Resource group name: Enter something like “fabmedical-SUFFIX”, as shown in the following screenshot.
    * Subscription: Select the subscription you will use for all the steps during the lab.
    * Resource group location: Choose a region where all Azure Container Registry SKUs are available, which is currently East US, West Central US, or West Europe, and remember this for future steps so that the resources you create in Azure are all kept within the same region.

        ![Microsoft Azure](images/ex0-task1-image_04.png)

    * Select Create.

6. When this completes, your Resource Group will be listed in the Azure Portal.

    ![Microsoft Azure](images/ex0-task1-image_05.png)

### Task 2: Create a Windows 10 Development VM (optional)

You will follow these steps to create a development VM (machine) for the following reasons:

* If your operating system is earlier than Windows 10 Anniversary Update, you will need it to work with WSL as instructed in the lab.
* If you are not sure if you set up WSL correctly, given there are a few ways to do this, it may be easier to create the development machine for a predictable experience.

> NOTE: Setting up the development machine is optional for Mac OS since you will use Terminal for commands. Setting up the development machine is also optional if you are certain you have a working installation of WSL on your current Windows 10 VM.

In this section, you will create a Windows 10 VM to act as your development machine. You will install the required components to complete the lab using this machine. You will use this machine instead of your local machine to carry out the instructions during the lab.

1. From the Azure Portal, select + Create a resource, type “Windows 10” in the Search the marketplace text box and press Enter.

    ![Microsoft Azure](images/ex0-task2-image_01.png)

2. Select Windows 10 Pro N, Version 1709 and select Create.
3. On the Basics blade of the Virtual Machine setup, set the following:

    * Name: Provide a unique name, such as “fabmedicald-SUFFIX” as shown in the following screenshot.
    * VM disk type: Leave as SSD.
    * User name: Provide a user name, such as “adminfabmedical”.
    * Password: Provide a password, such as “Password$123”.
    * Confirm password: Confirm the previously entered password.
    * Subscription: Choose the same subscription you are using for all your work.
    * Resource group: Choose Use existing and select the resource group you created previously.
    * Location: Choose the same region that you did before.
    * Select OK to complete the Basics blade.

        ![Microsoft Azure](images/ex0-task2-image_02.png)

4. From the Size blade, choose D2S_V2 Standard and Select.

    ![Microsoft Azure](images/ex0-task2-image_03.png)

5. From the Settings blade, accept the default values for all settings and select OK.
6. From the Create blade, you should see that validation passed and select Create.

    ![Microsoft Azure](images/ex0-task2-image_04.png)

7. The VM will begin deployment to your Azure subscription.

    ![Microsoft Azure](images/ex0-task2-image_05.png)

8. Once provisioned, you will see the VM in your list of resources belonging to the resource group you created previously and select the new VM.

    ![Microsoft Azure](images/ex0-task2-image_06.png)

9. In the Overview area for the VM, select Connect to establish a Remote Desktop Connection (RDP) for the VM.

    ![Microsoft Azure](images/ex0-task2-image_07.png)

10. Complete the steps to establish the RDP session and ensure that you are connected to the new VM.

### Task 3: Install WSL (Bash on Ubuntu on Windows)

> NOTE: If you are using a Windows 10 development machine, follow these steps. For Mac OS you can ignore this step since you will be using Terminal for all commands.

You will need WSL to complete various steps. A complete list of instructions for supported Windows 10 versions is available on this page: [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### Task 4: Create an SSH key

In this section, you will create an SSH key to securely access the VMs you create during the upcoming exercises.

1. Open a WSL command window.

    ![WSL](images/ex0-task4-image_01.png)

    or

    ![WSL](images/ex0-task4-image_02.png)

2. From the command line, enter the following command to ensure that a directory for the SSH keys is created. You can ignore any errors you see in the output.

    `mkdir .ssh`

3. From the command line, enter the following command to generate an SSH key pair. You can replace “admin” with your preferred name or handle.

    `ssh-keygen -t RSA -b 2048 -C admin@fabmedical`

4. You will be asked to save the generated key to a file. Enter ".ssh/fabmedical" for the name.
5. Enter a passphrase when prompted, and don’t forget it!
6. Because you entered “.ssh/fabmedical”, the file will be generated in the “.ssh” folder in your user folder, where WSL opens by default.
7. Keep this WSL window open and remain in the default directory for Task 5: Create a build agent VM.

    ![WSL](images/ex0-task4-image_03.png)

### Task 5: Create a build agent VM

In this section, you will create a Linux VM to act as your build agent. You will install Docker to this VM once it is set up, and you will use this VM during the lab to develop and deploy.

> NOTE: You can set up your local machine with Docker however the setup varies for different versions of Windows. For this lab, the build agent approach simply allows for predictable setup.

1. From the Azure Portal, select + Create a resource, type “Ubuntu” in the Search the marketplace text box and press Enter.

    ![Microsoft Azure](images/ex0-task5-image_01.png)

2. Select Ubuntu Server 16.04 LTS and select Create.
3. On the Basics blade of the Virtual Machine setup, set the following:

    * Name: Provide a unique name, such as “fabmedical-SUFFIX” as shown in the following screenshot.
    * VM disk type: Leave as SSD.
    * User name: Provide a user name, such as “adminfabmedical”.
    * Authentication type: Leave as SSH public key.
    * SSH public key: From your local machine, copy the public key portion of the SSH key pair you created previously, to the clipboard.

        * From WSL, verify you are in your user directory shown as “~”. This command will take you there:

            `cd ~`

        * Type the following command at the prompt to display the public key that you generated.

            `cat .ssh/fabmedical.pub`

        * Copy the entire contents of the file to the clipboard.

            ![WSL](images/ex0-task5-image_02.png)

        * Paste this value in the SSH public key textbox of the blade.

    * Subscription: Choose the same subscription you are using for all your work.
    * Resource group: Choose Use existing and select the resource group you created previously.
    * Location: Choose the same region that you did before.
    * Select OK to complete the Basics blade.

        ![Microsoft Azure](images/ex0-task5-image_03.png)

4. From the Size blade, choose D2S_V3 Standard and Select.

    ![Microsoft Azure](images/ex0-task5-image_04.png)

5. From the Settings blade, accept the default values for all settings and select OK.
6. From the Create blade, you should see that validation passed and select Create.

    ![Microsoft Azure](images/ex0-task5-image_05.png)

7. The VM will begin deployment to your Azure subscription.

    ![Microsoft Azure](images/ex0-task5-image_06.png)

8. Once provisioned, you will see the VM in your list of resources belonging to the resource group you created previously.

    ![Microsoft Azure](images/ex0-task5-image_07.png)

### Task 6: Connect securely to the build agent

In this section, you will validate that you can connect to the new build agent VM.

1. From the Azure portal, navigate to the Resource Group you created previously and select the new VM, fabmedical-SUFFIX.
2. In the Overview area for the VM, take note of the public IP address for the VM.

    ![Microsoft Azure](images/ex0-task6-image_01.png)

3. From your local machine, return to your open WSL window and make sure you are in your user directory ~ where the key pair was previously created. This command will take you there:

    `cd ~`

4. Connect to the new VM you created by typing the following command.

    `ssh -i [PRIVATEKEYNAME] [BUILDAGENTUSERNAME]@[BUILDAGENTIP]`

    Replace the bracketed values in the command as follows:

        • [PRIVATEKEYNAME]: Use the private key name “.ssh/fabmedical,” created above.

        • [BUILDAGENTUSERNAME]: Use the username for the VM, such as adminfabmedical.

        • [BUILDAGENTIP]: The IP address for the build agent VM, retrieved from the VM Overview blade in the Azure Portal.

    `ssh -i .ssh/fabmedical adminfabmedical@52.174.141.11`

5. When asked to confirm if you want to connect, as the authenticity of the connection cannot be validated, type “yes”.
6. When asked for the passphrase for the private key you created previously, enter this value.
7. You will connect to the VM with a command prompt such as the following. Keep this command prompt open for the next step.

    `adminfabmedical@fabmedical-SUFFIX:~$`
    ![WSL](images/ex0-task6-image_02.png)

> NOTE: If you have issues connecting, you may have pasted the SSH public key incorrectly. Unfortunately, if this is the case, you will have to recreate the VM and try again.

### Task 7: Complete the build agent setup

In this task, you will update the packages and install Docker engine.

1. Go to the WSL window that has the SSH connection open to the build agent VM.
2. Update the Ubuntu packages and install curl and support for repositories over HTTPS in a single step by typing the following in a single line command. When asked if you would like to proceed, respond by typing “y” and pressing enter.

    `sudo apt-get update && sudo apt install apt-transport-https ca-certificates curl software-properties-common`

3. Add Docker’s official GPG key by typing the following in a single line command.

    `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

4. Add Docker’s stable repository to Ubuntu packages list by typing the following in a single line command.

    `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

5. Update the Ubuntu packages and install Docker engine, node.js and the node package manager in a single step by typing the following in a single line command. When asked if you would like to proceed, respond by typing “y” and pressing enter.

    `sudo apt-get update && sudo apt install docker-ce nodejs npm`

6. Now, upgrade the Ubuntu packages to the latest version by typing the following in a single line command. When asked if you would like to proceed, respond by typing “y” and pressing enter.

    `sudo apt-get upgrade`

7. When the command has completed, check the Docker version installed by executing this command. The output may look something like that shown in the following screen shot. Note that the server version is not shown yet, because you didn’t run the command with elevated privileges (to be addressed shortly).

    `docker version`
    ![WSL](images/ex0-task7-image_01.png)

8. You may check the versions of node.js and npm as well, just for information purposes, using these commands.

    `nodejs --version`

    `npm -version`

9. Add your user to the Docker group so that you do not have to elevate privileges with sudo for every command. You can ignore any errors you see in the output.

    `sudo usermod -aG docker $USER`
    ![WSL](images/ex0-task7-image_02.png)

10. In order for the user permission changes to take effect, exit the SSH session by typing ‘exit’, then press \<Enter>. Repeat the commands in Task 6: Connect securely to the build agent from step 4 to establish the SSH session again.
11. Run the Docker version command again, and note the output now shows the server version as well.

    ![WSL](images/ex0-task7-image_03.png)

12. Run a few Docker commands.

    * One to see if there are any containers presently running

        `docker container ls`

    * One to see if any containers exist whether running or not

        `docker container ls -a`

13. In both cases, you will have an empty list but no errors running the command. Your build agent is ready with Docker engine running properly.

    ![WSL](images/ex0-task7-image_04.png)

### Task 8: Create a Docker Hub account

> NOTE: this is an alternative to using Azure Container Registry, since there are quite a few steps required to deploy container images to DCOS from a secure registry

Docker images are deployed from a Docker Registry.
To complete the lab, you will need access to a registry that is publicly accessible to the DC/OS Cloud cluster you are creating.
In this task, you will create a free Docker Hub account for this purpose, where you push images for deployment.

> NOTE: If you already have a public Docker Hub account you do not need to complete this task – you will need the name of the account for the exercises that rely on it.

1. Navigate to Docker Hub at [https://hub.docker.com](https://hub.docker.com).
2. Create a new Docker Hub account by providing a unique name for your Docker Hub ID, your email address for confirmation of the account, and a password.

    ![New to Docker](images/ex0-task8-image_01.png)

3. Once you confirm your email address, you can sign in and confirm your Docker Hub account is ready.

    ![Confirm email address](images/ex0-task8-image_02.png)

4. Login to your Docker Hub account. If it is a new account, you will not see any repositories yet. You will create these during the lab.

    ![Welcome to DockerHub](images/ex0-task8-image_03.png)

### Task 9: Create an Azure Container Service cluster (DC/OS)

In this task you will create your Azure Container Service cluster based on Apache Mesos. You will use the same SSH key you created previously to connect to this cluster in the next task.

> NOTE: In this step, you will be asked to create a second Resource Group as part of the template for creating the Azure Container Service cluster. This is due to the current template not providing the option to select an existing Resource Group, and this may change in the future.

1. From the Azure Portal, select New -> Containers and select Azure Container Service.

    ![Microsoft Azure](images/ex0-task9-image_01.png)

2. From the Azure Container Service blade click Create.
3. In the Basics blade provide the information shown in the screen shot that follows:

    * Enter a username such as “adminfabmedical”.
    * Paste the same SSH public key you used for the agent VM previously.
    * Choose the same subscription and location as you are using for other resources.
    * Since the template does not support using an existing Resource Group, provide a new name such adding a “2” to the suffix, such as fabmedical-SUFFIX2.

        ![Microsoft Azure](images/ex0-task9-image_02.png)

4. From the Framework configuration blade, choose DC/OS for the Orchestrator configuration, then click OK.

    ![Microsoft Azure](images/ex0-task9-image_03.png)

5. From the Azure Container service settings blade, enter the settings shown in the screen shot that follows:

    * Set the Agent count to 2.
    * Choose Standard DS1 for the Agent virtual machine size.
    * Set the Master count to 1.
    * Set the DNS prefix to something like “fabmedical-SUFFIX”.
    * Click OK.

        ![Microsoft Azure](images/ex0-task9-image_04.png)

6. Review the Summary blade and click OK.

    ![Microsoft Azure](images/ex0-task9-image_05.png)

7. From the Purchase blade, click Purchase.
8. You should see a successful deployment notification when the cluster is ready. It can take up to 30 minutes or more before your Azure Container Service cluster is listed in the Azure Portal.

    ![Microsoft Azure](images/ex0-task9-image_06.png)

> Note: If you experience errors related to lack of available cores, you may have to delete some other compute resources or request additional cores be added to your subscription and then try this again.

### Task 10: Download the sample source

FabMedical has provided starter files for you. They have taken a copy of one of their web sites, for their customer Contoso Neuro, and refactored it from a single node.js site into a web site with a content API that serves up the speakers and sessions. This is a starting point to validate the containerization of their web sites. They have asked you to use this as the starting point for helping them complete a POC that helps to validate the development workflow for running the web site and API as Docker containers, and managing them within container platform.

1. From WSL, connect to the build agent VM as you did previously in Task 6.

2. Download the starter files to the build agent by typing the following curl instruction (case sensitive): 

    ```bash
    curl -L -o FabMedical.tgz https://bit.ly/2FSDtOz
    ```

3. Create a new directory named FabMedical by typing in the following command:

    ```bash
    mkdir FabMedical
    ```

4. Unpack the archive with the following command. This will extract the files from the archive to the FabMedical directory you created. The directory is case sensitive when you navigate to it.

    ```bash
    tar -C FabMedical -xzf FabMedical.tgz
    ```

    NOTE: Keep this WSL window open as your build agent SSH connection. During the lab you will later open new WSL sessions to other machines.

5. Navigate to FabMedical folder and list the contents.

    ```bash
    cd FabMedical
    ll
    ```

6. You’ll see the listing includes two folders, one for the web site and another for the content API.

    ```bash
    content-api/
    content-web/
    ```
