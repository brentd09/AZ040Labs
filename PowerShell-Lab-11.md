---
lab:
    title: 'Lab: Jobs management with PowerShell'
    module: 'Module 11: Using background jobs and scheduled jobs'
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

# Lab: Jobs management with PowerShell

## Scenario

Background jobs provide a useful way to run multiple commands simultaneously and long-running commands in the background. In this lab, you'll learn to create and manage two of the three basic kinds of jobs.

You'll create and configure two scheduled jobs. You'll also create a scheduled task using a Windows PowerShell script that searches for and removes disabled accounts from a certain security group.

## Objectives

After completing this lab, you'll be able to:

- Start and manage jobs.
- Create a scheduled job.

### Estimated time: 30 minutes

## Lab Setup

Virtual machines: **LON-DC1**, **LON-SVR1**, and **LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine (VM) environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1** and sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-SVR1** and **LON-CL1**.

## Exercise 1: Starting and managing jobs

### Exercise scenario 1

In this exercise, you'll start jobs using two of the basic job types.

The main tasks for this exercise are:

1. Start a Windows PowerShell job.
1. Start a local job.
1. Review and manage job status.

### Task 1: Start a Windows PowerShell job

1. Ensure that you're signed in to **LON-CL1** as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Start a Windows PowerShell remoting job that retrieves a list of physical network adapters from **LON-DC1** and **LON-SVR1**. Name the job **RemoteNetAdapt**.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Some PowerShell commands have a parameter that allows the job to be run as a job (Background Job) 
    Get-Help -ShowWindow Get-NetAdapter
    ```

    ```PowerShell
    # However when you are asked to create a background job for the local computer it may be better to use the Start-Job cmdlet
    # Especially if the background job need a specific name
    Get-Help -ShowWindow Start-Job
    ```

    ```PowerShell
    # Another option is for when you need to run background jobs for multiple remote computers
    Get-Help -ShowWindow Invoke-Command 
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -AsJob -JobName RemoteNetAdapt -ComputerName LON-DC1,LON-SVR1 -ScriptBlock {Get-NetAdapter -Physical}  
    ```
    </Strong></details> 

3. Start a Windows PowerShell remote job that retrieves a list of Server Message Block (SMB) shares from **LON-DC1** and **LON-SVR1**. Name the job **RemoteShares**.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # When you need to run background jobs for multiple remote computers
    Get-Help -ShowWindow Invoke-Command 
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -AsJob -JobName RemoteShares -ComputerName LON-DC1,LON-SVR1 -ScriptBlock {Get-SmbShare}  
    ```
    </Strong></details> 
5. Start a Windows PowerShell remote job that retrieves all instances of the **Win32_Volume** Common Information Model (CIM) class from every computer in Active Directory Domain Services (AD DS). Name the job **RemoteDisks**. Because some domain computers might not start, some child jobs might fail.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # When you need to run background jobs for multiple remote computers
    Get-Help -ShowWindow Invoke-Command 
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -AsJob -JobName RemoteDisks -ComputerName (Get-ADComputer -Filter *).Name -ScriptBlock {Get-CimInstance -CimClass Win32_Volume}  
    ```
    </Strong></details> 

### Task 2: Start a local job

1. Start a local job that retrieves all entries from the **Security** event log. Name the job **LocalSecurity**.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Some PowerShell commands have a parameter that allows the job to be run as a job (Background Job) 
    Get-Help -ShowWindow Get-NetAdapter
    ```

    ```PowerShell
    # However when you are asked to create a background job for the local computer it may be better to use the Start-Job cmdlet
    # Especially if the background job need a specific name
    Get-Help -ShowWindow Start-Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Start-Job -Name LocalSecurity -ScriptBlock {Get-EventLog -Logname Secuity} 
    ```
    </Strong></details> 

### Task 3: Review and manage job status

1. Ensure that you're signed in to **LON-CL1** as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Display a list of running jobs.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Job -State Running
    ```
    </Strong></details> 
3. Display a list of running jobs whose names start with **remote**.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help -ShowWindow Get-Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Job -Name Remote*
    ```
    </Strong></details> 
5. Force the **LocalDir** job to stop.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Stop-Job -Name LocalDir
    ```
    </Strong></details>
7. Wait for all remaining jobs to finish successfully or fail.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Job | Wait-Job 
    ```
    </Strong></details>
9. Receive the results of the **RemoteNetAdapt** job.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Receive-Job -Name RemoteNetAdapt
    ```
    </Strong></details>
11. Use a single command line to receive the results from **LON-DC1** for the **RemoteDisks** job.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Job
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Job -Name RemoteDisks -IncludeChildJob | Where-Object {$_.Location -eq 'LON-DC1'} | Receive-Job 
    ```
    </Strong></details>

> **Note:** You must start by querying the parent job and include the child jobs . Filter the child jobs so that just the **LON-DC1** job remains, and then receive the results from that job. You'll use a total of three commands to complete this step.

## Exercise 2: Creating a scheduled job

### Exercise scenario 2

In this exercise, you'll create and run a scheduled job and retrieve its results. You'll then create and run a scheduled task from a Windows PowerShell script that removes a disabled user from a security group in AD DS.

The main tasks for this exercise are:

1. Create job options and job triggers.
1. Create a scheduled job and retrieve results.
1. Use a Windows PowerShell script as a scheduled task.

### Task 1: Create job options and job triggers

1. Ensure that you're signed into **LON-CL1** as **Adatum\\Administrator** with the password **Pa55w.rd**.
2. Click START button and type "ISE"
3. Select "Windows PowerShell ISE" from the menu
4. Create the scheduled job commands below within the ISE script pane
5. Create a job option object and store it in `$option`. Configure the job object so the job will:

    - Wake the computer to run.
    - Run under elevated permissions.
    
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *sched*job*
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
   $Option = New-ScheduledJobOption -WakeToRun -RunElevated 
    ```
    </Strong></details>    

1. Create a job trigger object and store it in `$trigger1`. Configure the trigger so the job will:

    - Run once in five minutes. Use **Get-Date** and a method of the resulting **DateTime** object to calculate five minutes from now.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell 
    # Discover which commands relate to the scheduling of jobs
    Get-Command *trigger*
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $Trigger1 = New-JobTrigger -Once -At (Get-Date).AddMinutes(5) 
    ```
    </Strong></details> 

### Task 2: Create a scheduled job and retrieve results

1. Using `$option` and `$trigger1`, create a new scheduled job that has the following attributes:

    - The job action retrieves all entries from the **Security** event log.
    - The job name is **LocalSecurityLog**.
    - The maximum number of job results is five.
   
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell 
    # Discover which commands relate to the scheduling of jobs
    Get-Command -Module PSScheduledJob
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Register-ScheduledJob -Name LocalSecurityLog -MaxResultCount 5 -Trigger $Trigger1 -ScheduledJobOption $Option -ScriptBlock {Get-EventLog -LogName Security} 
    ```
    </Strong></details> 

1. Display a list of job triggers, including time, for the **LocalSecurityLog** scheduled job.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Discover the properties of the Scheduled Job
    Get-ScheduledJob | Get-Member
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    (Get-ScheduledJob).JobTriggers 
    ```
    </Strong></details> 

3. Wait until the time displayed in step 2 has passed.
4. Display a list of jobs.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Scheduled Jobs create background jobs when they execute
    ```
    
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Job
    ```
    </Strong></details> 
6. Receive the job results for **LocalSecurityLog**.

    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Receive-Job -Name LocalSecurityLog
    ```
    </Strong></details> 

## Congratulations you have finished the lab
[Go back to first lab](AZ-040-Lab-01.md#_)
