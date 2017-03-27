## Azure Step-by-Step Setup
### Key-Points

- This guide will walk you through running your code on GPUs in Azure.
Before we start, it cannot be stressed enough: do not leave the VM running when you are not using it.
- The expected time from start to finish is 1-2 hours. The most time consuming part will be downloading and installing NVIDIA drivers, CUDA and Tensorflow.

## FAQ
- If your teams have changed and you need to grant/remove access for an individual, follow the directions here: https://docs.microsoft.com/en-us/azure/billing/billing-add-change-azure-subscription-administrator
- Do not install updates using: sudo apt-get install --upgrade
- This might break the CUDA driver installation if the kernel is updated.
- Out of disk space error when unzipping or downloading your dataset. 
- Attach a larger disk to your VM. https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-linux-classic-attach-disk
- Run df -h to see which disks have free space. 
- Store your data to the attached disk. There may be a temporary disk as well; do not store persistent data to it!
- Problems connecting (e.g., using SSH) to the VM
- Try ping <vm’s ip address>
- Try ssh to the VM
- Try restarting the VM and/or your local machine
- If all of the previous steps fail, file an Azure support ticket

## Creating a Microsoft account
- You should have received an email to your inbox with an invitation to join the Azure subscription from your Azure Administrator. 
- Please follow the instructions using the email address that received this invitation.
 
## Getting started
### Logging into Azure portal
- Once you have created your account, log in to Azure at: portal.azure.com
- After logging in, you should reach the dashboard page.
- If you have multiple subscriptions (e.g., you previously signed up for a free one), you must select the name of your institution. by clicking in the top right quarter. If no such option appears please contact your Azure Admin. 
 

## Create a VM
- Once you are logged in, click on the + on the left. Select Ubuntu Server 16.04 LTS.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure1.JPG)
- You will be presented withe VM Image detauls simply Click Create.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure2.JPG)
- Fill in the name, user, etc for your VM. You must change the storage type from SSD to HDD. Also, you must use the region that you have been allotted. 
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure3.JPG)
- View all (click the button) of the options and select NV6 by scrolling through the list. If NV6 does not show up, then you probably chose the wrong region or chose SSD in the previous page. Also, if you do not select NV6 (or any of the NV/NC options), then you are not using a GPU instance and the setup scripts later will fail.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure4.JPG)
- Select the appropiate VM Size and Click OK.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure5.JPG)
- Wait for the configuration to validate and then click OK.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure6.JPG)
 

## Using the VM
### Finding your VM
Login to http://portal.azure.com Click all resources and select your VM. Our subscription has many, but yours will only have one if you just followed the setup instructions.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure7.JPG)
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure8.JPG)
 
 
## Spinning up your VM
If you just completed the previous part and the VM has finished deploying, then your VM should be running already. 
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure9.JPG) 

## Connecting (SSH) to your VM
Once your VM is started (it may take a few minutes). Click connect and follow the instructions.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure10.JPG)
 
## Stopping your VM
- Once you are done working, stop your VM. You must do this or your credits will disappear at a rate of $1+ every hour the VM is on.
- Make sure your VM is fully stopped. If you see “stopped still incurring compute charges”, you must hit stop again.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure11.JPG)
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure12.JPG)
 
## Completing CUDA/Tensorflow setup
- You will need to SSH into your VM. 

##Installing CUDA and Tensorflow dependencies. 

There are two scripts that you will need to run and your VM will need to reboot in the between running them.

##[Step 1]
- First, in your VM do:
- git clone https://github.com/leestott/AzureGPU.git
- cd azure-gpu-setup
- You should see the following if you use 
```ls -all```
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure13.JPG)
 
- Run gpu-setup-part1.sh using the following command:

```./gpu-setup-part1.sh```

This will install some libraries, fetch and install NVIDIA drivers, and trigger a reboot. (The command will take some time to run.)

Once your VM has finished restarting. 

## [Step 2]

SSH into the VM again. Navigate to the azure-gpu-setup directory again. Run the command:
	
```./gpu-setup-part2.sh```

This script installs the CUDA toolkit, CUDNN, and Tensorflow. It also sets the required environment varibales. Once the script finishes, we must do:
	
```source ~/.bashrc```

This ensures that the shell will use the updated environment variables. Now, to test that Tensorflow and the GPU is properly configured, run the gpu test script by executing:

```python gpu-test.py```
 
 - ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure14.JPG)

## Filing a support ticket
- Click on the help icon in the left sidebar and select new support request.
- ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure15.JPG)
- Follow the on screen instructions.
 - ![Azure Portal VM Selection](https://github.com/leestott/AzureGPU/blob/master/Images/Azure16.JPG)

## General recommendations
We highly suggest the following for using the GPU instances:
- Develop and debug your code locally and use scp to copy your code to the VM to run for the long training steps.
- Save your work often and keep a local copy.
- Be mindful of when your instance is running and shut it off when you are not actively using it.


