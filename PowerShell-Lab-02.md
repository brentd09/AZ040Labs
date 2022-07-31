---
lab:
  title: 'Lab: Performing local system administration with PowerShell'
  module: 'Module 2: Windows PowerShell for local systems administration'
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

# Lab: Performing local system administration with PowerShell

## Scenario

You work for Adatum Corporation on the server support team. One of your first assignments is to configure the infrastructure service for a new branch office. You decide to complete the tasks by using Windows PowerShell.

## Objectives

After completing this lab, you'll be able to:

- Create and manage Active Directory objects by using Windows PowerShell.
- Configure network settings on Windows Server by using Windows PowerShell.
- Create an IIS website by using Windows PowerShell.

## Estimated time: 60 minutes

## Lab setup

Virtual machines: **AZ-040T00A-LON-DC1**, **AZ-040T00A-LON-SVR1**, and **AZ-040T00A-LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

## Lab Startup

1. Select **LON-DC1**.
1. Sign in by using the following credentials:

   - Username: **Administrator**
   - Password: **Pa55w.rd**
   - Domain: **Adatum**

1. Repeat these steps for **LON-CL1** and **LON-SVR1**.

## Exercise 1: Creating and managing Active Directory objects

### Exercise scenario 1

In this exercise, you'll create and manage Active Directory objects to create an organizational unit (OU) for a branch office, along with groups for OU administrators. You'll create accounts for a user and computer in the branch office, in the default OU, and add the user to the administrators group. You'll later move the user and computer to the OU that you created for the branch office. You'll use individual Windows PowerShell commands to accomplish these tasks from a client computer.

The main tasks for this exercise are:

1. Create a new OU for a branch office.
1. Create a group for branch office administrators.
1. Create a user and computer account for the branch office.
1. Move the group, user, and computer accounts to the branch office OU.

### Task 1: Create a new OU for a branch office

- From **LON-CL1**, use Windows PowerShell to create a new OU named **London**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help New-ADOrganizationalUnit -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-ADOrganizationalUnit -Name London
    ```
    </Strong></details> 

### Task 2: Create a group for branch office administrators

- In the **PowerShell** console, create the **London Admins** global security group.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help New-ADGroup -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-ADGroup "London Admins" -GroupScope Global
    ```
    </Strong></details> 

### Task 3: Create a user and computer account for the branch office

1. In the **PowerShell** console, create a user account for the user **Ty Carlson**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help New-ADUser -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-ADUser -Name Ty -DisplayName "Ty Carlson" 
    ```
    </Strong></details> 
3. Add the user to the **London Admins** group.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Add-ADGroupMember -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Add-ADGroupMember "London Admins" -Members Ty
    ```
    </Strong></details> 
5. Create a computer account for the **LON-CL2** computer.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help New-ADComputer -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-ADComputer LON-CL2
    ```
    </Strong></details> 

### Task 4: Move the group, user, and computer accounts to the branch office OU

- Use Windows PowerShell to move the following group, user, and computer accounts to the London OU:

  - **London Admins**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Move-ADObject -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Move-ADObject -Identity "CN=London Admins,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
    ```
    </Strong></details> 
  - **Ty Carlson**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Move-ADObject -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Move-ADObject -Identity "CN=Ty,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
    ```
    </Strong></details> 
  - **LON-CL2**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Move-ADObject -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Move-ADObject -Identity "CN=LON-CL2,CN=Computers,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
    ```
    </Strong></details> 

### Exercise 1 results

After completing this exercise, you'll have successfully identified and used commands for managing Active Directory objects in the Windows PowerShell command-line interface.

## Exercise 2: Configuring network settings on Windows Server

### Exercise scenario 2

In this exercise, you'll configure network settings on Windows Server. To review the effect, you'll test network connectivity before and after making changes. You'll use individual Windows PowerShell commands to accomplish these tasks on the server.

The main tasks for this exercise are:

1. Test the network connection and review the configuration.
3. Change the server IP address.
5. Change the DNS settings and default gateway for the server.
7. Verify and test the changes.

### Task 1: Test the network connection and review the configuration

1. Switch to **LON-SVR1**.
2. Open **Windows PowerShell** with administrative permissions.
3. Test the connection to **LON-DC1** and note the speed of the test.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Test-NetConnection LON-DC1
    ```
    </Strong></details> 
5. Review the network configuration for **LON-SVR1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *IPConfig*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-NetIPConfiguration
    ```
    </Strong></details> 
7. Note the IP address, default gateway, and DNS server.

### Task 2: Change the server IP address

- Use Windows PowerShell to change the IP address for the **Ethernet** network interface to **172.16.0.15/16**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *IPAddress*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.15 -PrefixLength 16
    ```
    
    ```PowerShell
    Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11
    ```
    </Strong></details> 

### Task 3: Change the DNS settings and default gateway for the server

1. Change the DNS settings of the **Ethernet** network interface to point at **172.16.0.12**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *DNSClient*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 172.16.0.12
    ```
    </Strong></details> 
3. Change the default gateway for the **Ethernet** network interface to **172.16.0.2**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Route*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false
    New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2
    ```
    </Strong></details> 

### Task 4: Verify and test the changes

1. On **LON-SVR1**, verify the changes to the network configuration.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *IPConfig*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-NetIPConfiguration
    ```
    </Strong></details> 
3. Test the connection to **LON-DC1**, and then note the difference in the test speed.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command -Verb Test
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Test-NetConnection LON-DC1
    ```
    </Strong></details> 
    
### Exercise 2 results

After completing this exercise, you'll have successfully identified and used Windows PowerShell commands for managing network configuration.

## Exercise 3: Creating a website

### Exercise scenario 3

In this exercise, you'll install the Web Server (IIS) server role and create a new internal website for the London branch. You'll use individual Windows PowerShell commands to accomplish these tasks on the server.

The main tasks for this exercise are:

1. Install the Web Server (IIS) role on the server.
1. Create a folder on the server for the website files.
1. Create the IIS website.

### Task 1: Install the Web Server (IIS) role on the server

- On **LON-SVR1**, use Windows PowerShell to install the Web server (IIS) role on **LON-SVR1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *feature*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Install-WindowsFeature Web-Server
    ```
    </Strong></details> 

### Task 2: Create a folder on the server for the website files

- On **LON-SVR1**, use PowerShell to create a folder named **London** under **C:\inetpub\wwwroot** for the website files.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Item*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-Item C:\inetpub\wwwroot\London -Type directory
    ```
    </Strong></details> 

### Task 3: Create the IIS website

1. On **LON-SVR1**, use PowerShell to create the IIS website by using the following configuration:

    - Name: **London**
    - Physical path: The folder that you previously created
    - Binding information: The current IP address of **LON-SVR1** using port **8080**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *IIS*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-IISSite London -PhysicalPath C:\inetpub\wwwroot\london -BindingInformation "172.16.0.15:8080:"
    ```
    </Strong></details> 

1. Open the website in Internet Explorer by using the IP address and port **8080**, and then verify that the site is using the provided settings. Internet Explorer will display an error message that a document hasn't been configured for the URL. The error message details give the physical path of the site, which should be **C:\\inetpub\\wwwroot\\london**.

### Exercise 3 results

After completing this exercise, you'll have successfully identified and used Windows PowerShell commands that would be used as part of a standardized web server configuration.


