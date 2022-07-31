---
lab:
    title: 'Lab: Performing remote administration with PowerShell'
    module: 'Module 8: Administering remote computers with Windows PowerShell'
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

# Lab: Performing remote administration with PowerShell

## Scenario

You're an administrator for Adatum Corporation and must perform maintenance tasks on a server running Windows Server 2019. You don't have physical access to the server, and instead plan to perform the tasks using Windows PowerShell remoting. You also have some tasks to perform on both a server and another client computer that runs Windows 10. In your environment, communication protocols such as remote procedure call (RPC) are blocked between your local computer and the servers. You plan to use Windows PowerShell remoting, and want to use sessions to provide persistence and reduce the setup and cleanup overhead that improvised remoting connections will impose.

## Objectives

After completing this lab, you'll be able to:

- Enable remoting on a client computer.
- Run a task on a remote computer by using one-to-one remoting.
- Run a task on two computers by using one-to-many remoting.
- Create and manage PSSessions.
- Send commands to multiple computers in parallel.

## Estimated time: 60 minutes

## Lab Setup

Virtual machines:

- **LON-DC1**
- **LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1**, and then sign in as **Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-CL1**.

## Exercise 1: Enabling remoting on the local computer

### Scenario 1

In this exercise, you'll enable remoting on the client computer.

The main task for this exercise is:

- Enable remoting for incoming connections.

### Task 1: Enable remoting for incoming connections

1. Ensure that you're signed in to **LON-CL1** as **Adatum\\Administrator**.
1. Enable remoting for incoming connections on the **LON-CL1** client computer.
<!--
    <details><summary>Click for hint</summary><Strong> 

    ``` 
    Get-Command *Remoting*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```
    Enable-PSRemoting -Force
    # The -Force parameter will not prompt for questions
    ```
    </Strong></details> 
-->
3. Verify that Windows PowerShell has created several session configurations. You have to find a command that can produce this list.
<!--
    <details><summary>Click for hint</summary><Strong> 

    ``` 
    Get-Help about_Session_Configurations
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```
    Get-PSSessionConfiguration
    ```
    </Strong></details> 
-->

## Exercise 2: Performing one-to-one remoting

### Scenario 2

In this exercise, you'll connect to a remote computer and perform maintenance tasks.

The main tasks for this exercise are:

1. Connect to the remote computer and install an operating system feature on it.
1. Test multi-hop remoting.
1. Observe remoting limitations.


### Task 1: Connect to the remote computer and install an operating system feature on it

1. Ensure that you're still signed in to **LON-CL1**.
1. On **LON-CL1**, in the Windows PowerShell console, use remoting to establish a one-to-one connection to **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Session*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Enter-PSSession -ComputerName LON-DC1
    ```
    </Strong></details> 
3. Install the Network Load Balancing (NLB) feature on **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Feature*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Install-WindowsFeature NLB
    ```
    </Strong></details> 
4. Disconnect from **LON-DC1**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Exit-PSSession
    ```
    </Strong></details> 

### Task 2: Test multi-hop remoting is disabled

1. Establish a one-to-one remoting connection with **LON-DC1**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Enter-PSSession -ComputerName LON-DC1
    ```
    </Strong></details> 
1. From this connection try to establish another connection to **LON-SVR1**. What happens and why?
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Enter-PSSession -ComputerName LON-SVR1
    # this connection will fail because of the muti-hop rule in PowerShell
    ```
    </Strong></details> 
1. Close the connection.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Exit-PSSession
    ```
    </Strong></details> 

### Task 3: Observe remoting limitations

1. Establish a one-to-one remoting connection to **LON-CL1** using the computer name **localhost**. This is your local computer, but this new connection creates a second user session for you on the computer.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Enter-PSSession -ComputerName localhost
    ```
    </Strong></details> 
1. Use Windows PowerShell to start a new instance of Notepad. What happens, and why?
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Notepad.exe
    ```
    </Strong></details> 
1. Close Notepad.exe using Ctrl-C. This may take some time to quit.   

1. Close the connection.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Exit-PSSession
    ```
    </Strong></details> 

## Exercise 3: Performing one-to-many remoting

In this exercise, you'll run commands against multiple computers. One of those will be the client computer, although you'll be establishing a second sign-in to it for the duration of each command.

The main tasks for this exercise are:

1. Retrieve a list of physical network adapters from two computers.
1. Compare the output of a local command to that of a remote command.

### Task 1: Retrieve a list of physical network adapters from two computers

1. On **LON-CL1**, using a keyword such as **adapter**, find a command that can list network adapters.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *adapter*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-NetAdapter
    ```
    </Strong></details> 
1. Review the Help for the command, then find a switch parameter that will limit output to physical adapters.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Get-NetAdapter -ShowWindow 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-NetAdapter -Physical
    ```
    </Strong></details> 
1. Use one-to-many remoting to run the command on both **LON-SVR1** and **LON-DC1** at the same time.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Run this help command and search in the help for "HOW TO RUN A REMOTE COMMAND"
    Get-Help About_Remote -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -ComputerName LON-SVR1,LON-DC1 -ScriptBlock { Get-NetAdapter -Physical }
    ```
    </Strong></details> 

### Task 2: Compare the output of a local command to that of a remote command

1. Pipe a collection of **Process** objects to **Get-Member**.
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Process | Get-Member
    ```
    </Strong></details> 
2. Use one-to-many remoting to retrieve **Process** objects from **LON-DC1**, and then pipe them to **Get-Member**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -ComputerName LON-DC1 -ScriptBlock { Get-Process } | Get-Member
    ```
    </Strong></details> 
3. Compare the output of the two **Get-Member** results.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The remote command will have the follow differences:
    # The TypeName will specify that it is Deserialized
    # The methods that appeared in step 1 will be missing from the remote command in step 2
    ```
    </Strong></details> 


## Exercise 4: Using implicit remoting

In this exercise, you'll use implicit remoting to import and run commands from a remote computer.

The main tasks for this exercise are as follows:

1. Create a persistent remoting connection to a server.
1. Import and use a module from a server.
1. Close all open remoting connections.

### Task 1: Create a persistent remoting connection to a server

1. On **LON-CL1**, sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
2. Select the **Start** menu, and then enter **powersh**.
3. In the results list, right-click **Windows PowerShell** and then select **Run as administrator**.
4. In the Windows PowerShell command window, create a persistent remoting connection to **LON-DC1**, and then save the resulting PSSession object in the variable `$dc`.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Run this help command and search for "HOW TO CREATE A PERSISTENT CONNECTION"
    Get-Help About_Remote -ShowWindow 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $dc = New-PSSession -ComputerName LON-DC1
    ```
    </Strong></details> 
5. Display a list of PSSessions in `$dc` and verify their availability.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $dc
    ```
    </Strong></details> 

### Task 2: Import and use a module from a server

1. From **LON-CL1**, display a list of Windows PowerShell modules on **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *module*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Module -ListAvailable -PSSession $dc
    ```
    </Strong></details> 
2. Locate a module on **LON-DC1** that can work with Server Message Block (SMB) shares.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Run this help command and search for "available"
    # Then search for "session", to find the PowerShell Session parameter 
    Get-Help Get-Module -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Module -ListAvailable -PSSession $dc | Where-Object { $_.Name -Like '*share*' }
    ```
    </Strong></details> 
3. Import the SMBShare module via the $DC session, and then add the prefix **DC** to the nouns for each imported command.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Module Import-Module -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Import-Module -PSSession $dc -Name SMBShare -Prefix DC
    ```
    </Strong></details> 
4. Display a list of SMB shares on **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The Prefix DC is added to the front of the noun
    # Remember to use this prefix when running the remotely imported module
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-DCSMBShare
    ```
    </Strong></details> 
5. Display a list of SMB shares on the client computer.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The local command is still available without the prefix
    # Running the normal SMBShare cammands will autoload the module locally
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-SMBShare
    ```
    </Strong></details> 

### Task 3: Close all open remoting connections

- In the Windows PowerShell console, close all open remoting connections.

## Exercise 5: Managing multiple computers

In this exercise, you'll perform several management tasks against multiple computers, relying on PSSessions to provide persistence.

The main tasks for this exercise are:

1. Create PSSessions to two computers.
1. Create a report that displays Windows Firewall rules from two computers.
1. Create and display an HTML report that displays local disk information from two computers.
1. Close all open PSSessions.

### Task 1: Create PSSessions to two computers

1. In the **Windows PowerShell** console, create PSSessions to both **LON-SVR1** and **LON-DC1** and Save both PSSession objects in the variable $computers.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers = New-PSSession -ComputerName LON-SVR1,LON-DC1
    ```
    </Strong></details> 
1. Verify that the connections in $computers are open and available.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers
    ```
    </Strong></details> 

### Task 2: Create a report that displays Windows Firewall rules from two computers

1. Find a Windows PowerShell module capable of working with Network Security.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Module *security* -ListAvailable
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    # NetSecurity is the required module
    ```
    </Strong></details> 
1. Find a command that can display Windows Firewall rules.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Firewall* -Module NetSecurity
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-NetFirewallRule
    ```
    </Strong></details> 
1. Use a single command line to list all enabled firewall rules on both **LON-SVR1** and **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Use one-to-many remoting to find these firewall rules
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-NetFirewallRule -Enabled True } 
    ```
    </Strong></details> 
1. Display only the rule names and the computer name each rule came from.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-NetFirewallRule -Enabled True } | Get-Member
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-NetFirewallRule -Enabled True } | Select Name,PSComputerName
    ```
    </Strong></details> 

### Task 3: Create and display an HTML report that displays local disk information from the two computers

1. As a test, use **Get-WmiObject** to display a list of local hard drives (the **Win32_LogicalDisk** class, filtered to include only those drives with a drive type of **3**).
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3"
    ```
    </Strong></details> 
3. Use remoting to run the **Get-WmiObject** command against both **LON-DC1** and **LON-SVR1**. Don't add a **–ComputerName** parameter to the **Get-WmiObject** command. Your HTML report must include each computer’s name, each drive’s letter, and its free space and total size in bytes.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" } | Get-Member
    ```
    <br>
    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" } | Select-Object
    # Compare the two results to discover which properties you will need for this report
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Invoke-Command -Session $computers -ScriptBlock { Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" } | Select-Object PSComputerName,DeviceID,FreeSpace | ConvertTo-Html | Out-File e:\DiskReport.html
    ```
    </Strong></details> 


### Task 4: Test the HTML report

- Open Internet Explorer and type the following into the address bar
```PowerShell
e:\DiskReport.html
```

## Congratulations you have fininshed the lab

[Go to next lab](AZ-040-Lab-09.md#_)
