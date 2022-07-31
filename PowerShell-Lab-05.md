---
lab:
    title: 'Lab: Querying information by using WMI and CIM'
    module: 'Module 5: Querying management information by using CIM and WMI'
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

# Lab: Querying information by using WMI and CIM

## Scenario

You have to query management information from several computers. You start by querying the information from your local computer and from one test computer in your environment.

## Objectives

After completing this lab, you'll be able to:

- Query information by using Windows Management Instrumentation (WMI) commands.
- Query information by using Common Information Model (CIM) commands.
- Invoke methods by using WMI and CIM commands.

## Estimated time: 45 minutes

## Lab Setup

Virtual machines: **AZ-040T00A-LON-DC1** and **AZ-040T00A-LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1** and sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-CL1**.

## Exercise 1: Querying information by using WMI

### Scenario 1

In this exercise, you'll discover repository classes and then use WMI commands to query them.

The main tasks for this exercise are as follows:

1. Query IP addresses.
1. Query operating system version information.
1. Query computer system hardware information.
1. Query service information.

### Task 1: Query IP addresses

1. On **LON-CL1**, start Windows PowerShell as an administrator.
1. An IP address is part of a network adapter’s configuration. Using the keyword **configuration**, find a repository class that lists IP addresses being used by the local computer.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *WMI*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Namespace root\cimv2 -List | Where-Object { $_.Name -like '*configuration*'} | Sort-Object -Property Name
    ```
    </Strong></details> 
1. Using a WMI command and the class that you discovered in the previous step, display a list of all the statically configured IP addresses.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_NetworkAdapterConfiguration | Where-Object {$_.DHCPEnabled -eq $False} | Select-Object -Property IPAddress
    ```
    </Strong></details> 

### Task 2: Query operating system version information

1. Using the keyword **operating**, find a repository class that lists operating system version information. Sort it by the **name** property.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Namespace root\cimv2 -List | Where-Object {$_.Name -like '*operating*'} | Sort-Object -Property Name
    ```
    </Strong></details> 
1. Display a list of properties for the class that you discovered in the previous step.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # When using Get-WMIObject you can use Get-Member to see the Methods and the Properties of the WMI object
    # However with the Get-CimClass cmdlet you can only use Get-Member to view Properties but one of these properties contains the method names (CimClassMethods) 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_OperatingSystem | Get-Member
    ```
    </Strong></details> 
1. Make note of the properties that contain the operating system version, the service pack major version, and the operating system build number.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Look for the following properties: Version, ServicePackMajorVersion, BuildNumber
    ```
    </Strong></details> 
1. Using a WMI command and the class that you discovered in step 1, display the local operating system version, service pack major version, and operating system build number.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_OperatingSystem | Select-Object -Property Version,ServicePackMajorVersion,BuildNumber
    ```
    </Strong></details> 

### Task 3: Query computer system hardware information

1. Using the keyword **system**, find a repository class that contains computer system information.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Namespace root\cimv2 -List | Where-Object {$_.Name -like '*system*'} | Sort-Object -Property Name 
    ```
    </Strong></details> 
1. Display a list of properties and property values for the class that you discovered in the previous step.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_ComputerSystem | Select-Object -Property *
    ```
    </Strong></details> 
1. Using the list of properties and a WMI command, display the local computer’s manufacturer, model, and total physical memory. Label the column for total physical memory **RAM**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_ComputerSystem | Select-Object -Property Manufacturer,Model,@{n='RAM';e={$PSItem.TotalPhysicalMemory}}
    ```
    </Strong></details> 

### Task 4: Query service information

1. Using the keyword **service**, find a repository class that contains service information.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Namespace root\cimv2 -List | Where-Object {$_.Name -like '*service*'} | Sort-Object -Property Name 
    ```
    </Strong></details> 
1. Display a list of properties and property values for the class that you discovered in the previous step.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_Service | Select-Object -Property *
    ```
    </Strong></details> 
1. Using the list of properties and a WMI command, display the service name, status (Running or Stopped), and sign-in name for all services that have names starting with the letter **S**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_Service -Filter "Name LIKE 'S%'" | Select-Object -Property Name,State,StartName 
    ```
    </Strong></details> 

## Exercise 2: Querying information by using CIM

### Scenario 2

In this exercise, you'll discover new repository classes and query them by using CIM commands.

The main tasks for this exercise are as follows:

1. Query user accounts.
1. Query BIOS information.
1. Query network adapter configuration information.
1. Query user group information.

### Task 1: Query user accounts

1. Using a CIM command and the keyword **user**, find a repository class that lists user accounts.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *CIM*
    # Notice that there are multiple cmdlets for CIM, CimInstance is referring to an actual instance of the class
    # where CIMClass cmdlets are used to find informationabout the class itself.
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimClass -ClassName *user* 
    # Win32_UserAccount is the one
    ```
    </Strong></details> 
1. Using a CIM command, display a list of properties for the class that you discovered in the previous step.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -Class Win32_UserAccount | Get-Member
    ```
    </Strong></details> 
1. Using a CIM command and the property list, display a list of user accounts in a table. Include columns for the account caption, domain, security ID, full name, and name. The full name column might be blank for some or all accounts.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Generally by default PowerShell will display 4 or less properties in a table and 5 or more as a list
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -Class Win32_UserAccount | Format-Table -Property Caption,Domain,SID,FullName,Name 
    ```
    </Strong></details> 

### Task 2: Query BIOS information

1. Using the keyword **bios** and a CIM command, find a repository class that contains BIOS information.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimClass -ClassName *bios*
    ```
    </Strong></details>    
1. Using a CIM command and the class that you discovered in the previous step, display a list of all available BIOS information.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -Class Win32_BIOS
    ```
    </Strong></details> 

### Task 3: Query network adapter configuration information

1. Use a CIM command to display all the local instances of the `Win32_NetworkAdapterConfiguration` class.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -Classname Win32_NetworkAdapterConfiguration 
    ```
    </Strong></details> 
1. Use a CIM command to display all instances of the `Win32_NetworkAdapterConfiguration` class that exist on **LON-DC1**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -Classname Win32_NetworkAdapterConfiguration -ComputerName LON-DC1 
    ```
    </Strong></details> 

### Task 4: Query user group information

1. Using a CIM command and the keyword **group**, find a class that lists user groups.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimClass -ClassName *group*
    # Win32_Group is the one we want
    ```
    </Strong></details> 
1. Using a CIM command, display a list of the user groups that exist on **LON-DC1**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -ClassName Win32_Group -ComputerName LON-DC1
    ```
    </Strong></details> 

## Exercise 3: Invoking methods

### Scenario 3

In this exercise, you'll use WMI and CIM commands to invoke methods of repository objects.

The main tasks for this exercise are as follows:

1. Invoke a CIM method.
1. Invoke a WMI method.

### Task 1: Invoke a CIM method

- Using a CIM command stop the Spooler service on the **LON-DC1** machine by running the command on **LON-CL1**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Invoke-CimMethod -ShowWindow
    # It can be easier to find the Instance of the class first and then pipe it into the Invoke-CimMethod cmdlet.
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -ClassName Win32_Service -ComputerName LON-DC1 | 
    Where-Object {$_.Name -eq 'Spooler'} |
    Invoke-CimMethod -MethodName StartService
    ```
    </Strong></details> 

### Task 2: Invoke a WMI method

1. Use the `Get-Service` cmdlet to review the **StartType** property of the **WinRM** service.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Service -Name WinRM | Select-Object -Property * 
    ```
    </Strong></details> 
1. Using WMI commands and the `ChangeStartMode` method of `Win32_Service`, change the start mode of the **WinRM** service to **Automatic**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # It is useful to know that the cimv2 namespace is very well documented online
    # you can search online for Win32_Service class in docs.micrososoft.com 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-WmiObject -Class Win32_Service -Filter "Name='WinRM'" | 
    Invoke-WmiMethod -Name ChangeStartMode -Argument 'Automatic' 
    ```
    </Strong></details> 
1. Use the `Get-Service` cmdlet to verify that the **StartType** property of the **WinRM** service has been updated to **Automatic**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Service WinRM | Select-Object -Property *
    ```
    </Strong></details> 

[Go to next lab](AZ-040-Lab-06.md#_)
