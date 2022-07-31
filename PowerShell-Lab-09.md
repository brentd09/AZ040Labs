---
lab:
    title: 'Lab: Azure resource management with PowerShell'
    module: 'Module 9: Managing Azure resources with PowerShell'
---

<!--
    <details><summary>Click for hint</summary><Strong> 

    ``` 
    HINT
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```
    ANSWER
    ```
    </Strong></details> 
-->

###### _

# Lab: Azure resource management with PowerShell

## Scenario

You're a system administrator for the London branch office of Adatum Corporation. You need to evaluate the Microsoft Azure platform to run virtual machines (VMs) and other resources for your company. As part of your evaluation, you also want to test PowerShell administration of Azure-based resources.

# Objectives

After completing this lab, you'll be able to:

- Install the Az module for Windows PowerShell.
- Run and use the Azure Cloud Shell environment.
- Manage Azure VMs and disks by using PowerShell.

## Estimated time: 60 minutes

## Lab setup

Virtual machines: **LON-DC1** and **LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1** and sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-CL1**.

## Exercise 1: Signing into the Azure cloud slicing subscription and Installing the PowerShell Az module

### Scenario 1

The main tasks for this exercise are:

1. Sign in to the Azure portal.
1. Install the Azure Az module for PowerShell. 

### Task 1: Sign into Azure portal and activate the cloud slicing subscription

1. On **LON-CL1**, open Microsoft Edge browser.
3. In the address bar type the following:

    ```PowerShell
    https://portal.azure.com
    ```
    
3. Use the Username and Password in the Resources tab of the lab instructions
   - The username will be very long and be similar format to: User1-21764458@LODSPRODMCA.onmicrosoft.com
4. In the Azure portal choose "Maybe Later" on the "Welcome to Microsoft Azure dialog box"
5. In the search bar at the top of the portal type "Subscriptions"
6. Choose Subscriptions from the drop down menu
7. Check to see if the MOC Subscription has been activated yet (This subscription takes some time to be created and activated)

 > Do not wait for this to appear, continue on with the rest of the lab and the subscription will appear later.


### Task 2: Install the Azure Az module for PowerShell

1. On **LON-CL1**, start the PowerShell 7.1 environment.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # this is powershell 7 not Windows PowerShell
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    pwsh.exe
    ```
    </Strong></details> 
3. Check your version of PowerShell by using `$PSVersionTable.PSVersion`.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    $PSVersionTable
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $PSVersionTable.PSVersion
    ```
    </Strong></details> 
5. Set the execution policy to **RemoteSigned** for the current user.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Set-ExecutionPolicy -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
    ```
    </Strong></details> 
7. From the PowerShell Gallery, install the Az module for the current user by using the **Install-Module** command.
    > This will take some time to install, wait until the installation of all of the AZ modules have fininshed
   
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Install-Module -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Install-Module AZ -Verbose -Force
    ```
    </Strong></details> 
9. Use **Connect-AzAccount** to sign in to your Azure subscription.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Connect-AzAccount -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    # When you run this command it will show website asking you to sign in
    # Sign in with your Username you found in the Resourses tab of the lab instructions
    Connect-AzAccount
    ```
    </Strong></details> 

## Exercise 2: Using Azure Cloud Shell

### Scenario 2

To work with other Azure resources such as Azure VMs, you need to create an Azure resource group. You decide to use Azure Cloud Shell to perform this task.

The main task for this exercise is:

- Use Azure Cloud Shell to create a resource group.

    > You could use the modules you have downloaded in the previous task <br>
    > However this next exercise give you experience in running the  <br>
    > PowerShell commands from the Clous Shell, meaning you do not have  <br>
    > to install the Az Modules <br>

### Task 1: Use Azure Cloud Shell to create a resource group

1. On the AZ-040T00A-LON-CL1 computer, open the Edge browser window and go to 
  
  ```PowerShell
  portal.azure.com
  ```

1. On the Microsoft Azure portal click the hamburger menu on the top left of the portal to find the following:
2. On the menu choose "Virtual Machines" and ensure that no virtual machines (VMs) are created. 
3. Again from the menu select "Storage Accounts". Ensure that no storage accounts are created. 
3. On the Microsoft Azure top blue bar, select the Cloud Shell icon, (hover over each icon with the mounse to find the Cloud Shell Icon >_ )
3. In the Welcome to Azure Cloud Shell window, select "PowerShell".
3. On the You have no storage mounted page, review the note about the missing storage account that's needed for Cloud Shell to run. Verify that in the Subscription field, your MOC subscription is selected, and then select "Create storage". Wait until the storage account is created.

   > When your storage account is created, the Cloud Shell console should open, and you should get a prompt in the format PS /home/user1-21764973>.

3. At the PowerShell prompt, Enter 
```PowerShell
Get-AzSubscription

```
5. Enter 
```PowerShell
Get-AzResourceGroup to review the resource group information.
```
7. On the Cloud Shell window Use the drop-down list to switch from PowerShell to the "Bash shell" and confirm your choice.
8. At the Bash shell prompt, Enter 
  ```PowerShell
  az account list
  
  ```
  
3. Enter az resource list to review the resource group information.

    > Switch back to the PowerShell interface.

3. In the PowerShell console, enter the following command to create a new Resource Group
  ```PowerShell
  New-AzResourceGroup -Name ResourceGroup1 -Location eastus
  
  ```

### Task 1: Use Azure Cloud Shell to create a Virtual Machine

1. In the Windows PowerShell window, enter the following command to define the admin credentials for the VM you want to create in Azure:

```PowerShell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

```

> When prompted, choose the username and password that you want to use as admin credentials for the new VM. Do not use name Admin for administrator.

2. In the Windows PowerShell window, enter the following command to define the VM parameters, and then select Enter. Replace yourname with your real name:

```PowerShell
$vmParams = @{
 ResourceGroupName = 'ResourceGroup1'
 Name = 'TestVM1'
 Location = 'eastus'
 ImageName = 'Win2016Datacenter'
 PublicIpAddressName = 'TestPublicIp'
 Credential = $cred
 OpenPorts = 3389
}

```

3. To create a new VM based on these parameters and store it in the newVM1 variable, enter the following command, and then select Enter:

```PowerShell
$newVM1 = New-AzVM @vmParams

```

> Note: Wait until the Azure VM is created.

4. To check the parameters on the new VM, enter the following commands, and then select Enter:

```PowerShell
$NewVM1

```
```PowerShell
$newVM1.OSProfile | Select-Object ComputerName,AdminUserName

```
```PowerShell
$newVM1 | Get-AzNetworkInterface | Select-Object -ExpandProperty IpConfigurations | Select-Object Name,PrivateIpAddress

```

5. To find a public IP address on the Azure VM you created so you can connect to it, enter the following commands, and then select Enter:
> **Do steps 5 & 6 on the lab machine's PowerShell 7 window**
```PowerShell
$publicIp = Get-AzPublicIpAddress -Name TestPublicIp -ResourceGroupName ResourceGroup1
$publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}

```

> Note the value of IPAddress in the table output.

6. Enter the following command, and then select Enter to connect to the VM:

```PowerShell
mstsc.exe /v $PublicIP.IPAddress

```

> When prompted, sign in with the credentials you provided for this VM. Ensure that you're connected to the Windows Server 2016 VM. Check the VM functionality and then shut it down.

### Task 2: Add a disk to the Azure VM by using PowerShell
1. Switch to the Microsoft Edge window, where you have the Azure portal open. 
2. From the Search bar type Virtual Machines, and select Virtual Machines from the menu.
3. Ensure that TestVM1 is listed. If not refresh the Virtual Machines page
4. Select the virtual machine listed.
5. On the Overview page, check the parameters of the VM you created. Select Disk. Ensure that only one disk is created (Os disk).
6. To create a data disk for the existing VM, in the Windows PowerShell window, enter the following commands, and select Enter after each:

```powershell
$VirtualMachine = Get-AzVM -ResourceGroupName "ResourceGroup1" -Name "TestVM1"

Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty

Update-AzVM -ResourceGroupName "ResourceGroup1" -VM $VirtualMachine

```
> This will take some time to complete and update the Azure virtual machine
5. Switch to the Azure portal and refresh the Disks page. You should be able to notice a new disk called disk1 in the Data disks section.

## You have successfully completed this Lab. Click End to end the lab.

[Go to next lab](AZ-040-Lab-10.md#_)
