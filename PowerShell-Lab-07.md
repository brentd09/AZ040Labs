---
lab:
    title: 'Lab: Using scripts with PowerShell'
    module: 'Module 4: Windows PowerShell scripting'
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

# Lab: Using scripts with PowerShell

## Scenario

You've started to develop Windows PowerShell scripts to simplify administration in your organization. There are multiple tasks to accomplish, and you'll create a Windows PowerShell script for each one.

## Objectives

After completing this lab, you'll be able to:

- Digitally sign a script.
- Process an array by using ForEach.
- Process items by using If statements.
- Create user accounts based on a CSV file.
- Query disk information from remote computers.
- Update a script to use alternate credentials.

## Estimated time: 150 minutes

## Lab setup

- Virtual machines: **LON-DC1**, **LON-SVR1**, and **LON-CL1**
- User name: **Adatum\\Administrator**
- Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1** and sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-SVR1** and **LON-CL1**.

## Exercise 1: Signing a script

### Exercise scenario 1

To enhance security, you're considering a requirement that all Windows PowerShell scripts in your environment be digitally signed. Before implementing this requirement, you want to test the process.

The main tasks for this exercise are:

1. Install a code signing certificate.
1. Digitally sign a script.
1. Set the execution policy.

### Task 1: Install a code signing certificate

1. On **LON-CL1**, open an **MMC** console, and then add the **Certificates** snap-in focused on **My user account**.
1. In the **MMC** console, browse to **Certificates - Current User\\Personal**.
1. Use the context menu of the **Personal** folder and select **Request New Certificate**.
1. Use the following settings in the **Certificate Enrollment** wizard:

   - **Active Directory Enrollment Policy**
   - **Adatum Code Signing** template

1. In the **MMC** console, verify that the new code-signing certificate is present.
1. Close the **MMC** console.

### Task 2: Digitally sign a script

1. Open a Windows PowerShell prompt.
1. Place the code-signing certificate in **Cert:\CurrentUser\My** into a variable.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $cert = Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
    ```
    </Strong></details> 
1. In **E:\Mod07\Labfiles**, rename **HelloWorld.txt** to **HelloWorld.ps1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command -noun Item
    # Look for a command to rename a file
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-Location E:\Mod07\Labfiles
    Rename-Item HelloWorld.txt HelloWorld.ps1
    ```
    </Strong></details> 
1. Use the **Set-Authenticode** cmdlet to apply a digital signature to **HelloWorld.ps1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Set-AuthenticodeSignature -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-AuthenticodeSignature -FilePath HelloWorld.ps1 -Certificate $cert
    ```
    </Strong></details> 

### Task 3: Set the execution policy

1. On **LON-CL1**, set the execution policy to allow only signed scripts.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Execution*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-ExecutionPolicy -ExecutionPolicy AllSigned
    ```
    </Strong></details> 
2. Verify that you can run **HelloWorld.ps1**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    .\HelloWorld.ps1
    ```
    </Strong></details> 
3. Make unathorized edits to the **HelloWorld.ps1** file.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Use Out-File with the Append parameter to modify the file
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    "Unauthorized edit" | Out-File -Append -FilePath .\HelloWorld.ps1
    ```
    </Strong></details> 
4. Attempt to run the unauthorized script.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # This will fail because the digital signature no longer matches the contents of the file
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    .\HelloWorld.ps1
    ```
    </Strong></details>     
6. Set the execution policy to **Unrestricted**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-ExecutionPolicy Unrestricted
    ```
    </Strong></details> 

## Exercise 2: Processing an array with a ForEach loop

### Scenario 2

Adatum Corporation is testing a new Voice over IP (VoIP) and video-conferencing system. To support this system, you must set the **ipPhone** attribute for a group of test users. The naming convention that's been selected for the **ipPhone** attribute is **FirstName.LastName@adatum.com**.

The main tasks for this exercise are:

1. Create a test group.
1. Create a script to configure the `ipPhone` attribute.

### Task 1: Create a test group

1. On **LON-CL1**, use PowerShell to create a new Active Directory Domain Services (AD DS) group named **IPPhoneTest** in the **IT** organizational unit.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Group* -Module ActiveDirectory
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    New-ADGroup -Name IPPhoneTest -GroupScope Universal -GroupCategory Security -Path "OU=IT,DC=Adatum,DC=com"
    # Either Universal or Global group scope would work here 
    ```
    </Strong></details> 
3. Add the following users as members in the **IPPhoneTest** group:

   - **Abbi Skinner**
   - **Ida Alksne**
   - **Parsa Schoonen**
   - **Tonia Guthrie**
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Add-ADGroupMember -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Add-ADGroupMember -Identity IPPhoneTest -Members Abbi,Ida,Parsa,Tonia
    ```
    </Strong></details> 

### Task 2: Create a script to configure the ipPhone attribute

1. Create a script named **E:\\Mod07\\Labfiles\\ipPhone.ps1**, and then open it in the Windows PowerShell ISE.
2. Use **Get-ADGroupMember** to create a query to obtain the membership of the **IPPhoneTest** group.
3. Create a **ForEach** loop that processes the users that are members of **IPPhoneTest**.
4. In the loop:
   - Use **Get-ADUser** to retrieve the first and last names for a user.
   - Calculate the required value for the **ipPhone** attribute.
   - Use **Set-ADUser** with the *-Replace* parameter to set the **ipPhone** attribute for the user.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The answer below will show all of the elements of this script
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $TestUsersFromGroup = Get-ADGroupMember -Identity IPPhoneTest
    # The group membership does not store all of the user information
    Foreach ($TestUser in $TestUsersFromGroup) {
      $ADUserObject = Get-ADUser -Identity $TestUser
      # This previous command gets all of the information we need 
      # to construct the ipPhone attribute 
      $IpPhoneAttrib = $ADUserObject.GivenName + '.' + $ADUserObject.Surname + '@adatum.com'
      Set-ADUser -Identity $TestUser -Replace @{ipPhone = $IpPhoneAttrib}
    }
    ```
    </Strong></details> 

5. Run the script and then verify that the **ipPhone** attribute is modified for the selected users.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-ADUser -Identity Ida -Properties ipPhone
    # Check to see if the ipPhone attibute is set for Ida
    ```
    </Strong></details> 


## Exercise 3: Processing items by using If statements

Some of the servers in your organization have services that don't start properly when the server is restarted. You want to create a script that can be used to start a specified list of services. When you've performed sufficient testing, you plan to configure a scheduled task that runs the script. During the testing phase, you'll work with the Windows Time and Print Spooler services.

The main tasks for this exercise are:

1. Create services.txt with service names.
1. Create a script that starts stopped services.

### Task 1: Create services.txt with service names

1. On **LON-CL1**, open Windows PowerShell.
2. Create a new file **E:\\Mod07\\Labfiles\\services.txt**.

    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Set-Location E:\Mod07\Labfiles
    New-Item services.txt -ItemType File
    ```
    </Strong></details> 
3. Identify the name for the **Print Spooler** service and add it to **services.txt**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Out-File -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Service "Print Spooler" | Select -ExpandProperty Name | Out-File services.txt -Append
    ```
    </Strong></details> 
4. Identify the correct name for the **Windows Time** service and add it to **services.txt**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Service "Windows Time" | Select -ExpandProperty Name | Out-File services.txt -Append
    ```
    </Strong></details> 

### Task 2: Create a script that starts stopped services

1. Create a new script **E:\\Mod07\\Labfiles\\StartServices.ps1**.
2. Retrieve the service names from **services.txt** and place them in a variable.
3. Use a **ForEach** loop to process each service:

   - If the service isn't running, start it, and then enter text to the screen indicating that the service was started.
   - If the service is running, do nothing, and then enter text to the screen indicating that no action was required.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The answer below shows the script
    # BEFORE running the script make sure that one of the services (Windows Time or Spooler) is stopped 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $ServiceNames = Get-Content E:\Mod07\Labfiles\services.txt
    foreach ($ServiceName in $ServiceNames) {
      $ServiceInfo = Get-Service -Name $ServiceName
      if ($ServiceInfo.Status -ne 'Running') {
        Start-Service -Name $ServiceName
        Write-Host -ForegroundColor Red "The service ($ServiceName) was not running, it is starting now"
      }
      else {
        Write-Host -ForegroundColor Green "The service ($ServiceName) was already running, nothing to do here!"
      }
    }
    ```
    </Strong></details> 

## Exercise 4: Creating users based on a CSV file

### Exercise scenario 4

The helpdesk for Adatum creates user accounts once per week based on data provided by the Human Resources department. This data is provided in a CSV file.

There have been multiple instances where the new user accounts weren't created with the correct information. The helpdesk has been using the CSV files as a reference and creating the user accounts by using graphical tools. You want to automate this process to avoid these errors.

The main task for this exercise is:

- Create AD DS users from a CSV file.

### Task 1: Create AD DS users from a CSV file

1. Create a new script named **E:\\Mod07\\Labfiles\\CreateUsers.ps1**.
1. Import **users.csv** and store the objects in a variable.
1. Create a **ForEach** loop that processes the data in the variable to create user accounts:

   - Create a variable that contains the Lightweight Directory Access Protocol (LDAP) name of the organizational unit for the user. For example: OU=IT,DC=Adatum,DC=com
   - Create a variable that contains the user principal name for the new user. This should be in the format **UserID@adatum.com**.
   - Create a variable that contains the display name for the new user. This should be in the format *FirstName LastName*.
   - Enter a status message to the screen indicating which user is being created and where.
   - Create the new user and be sure to set the following:

      - **GivenName**
      - **Surname**
      - **Name**
      - **DisplayName**
      - **SamAccountName**
      - **UserPrincipalName**
      - **Path**
      - **Department**

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help about_Splatting
    # Splatting allow you to collect all of the information for the parameters 
    # in a hash table and then use the hash table instead of individual parameters 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $UserInfoFromCSV = Import-Csv -Path E:\Mod07\Labfiles\users.csv
    foreach ($User in $UserInfoFromCSV) {
      $UserSplat = @{
        GivenName           = $User.First
        SurName             = $USer.Last
        Name                = $User.First + ' ' + $User.Last
        DisplayName         = $User.First + ' ' + $User.Last
        SamAccountname      = $User.UserID
        UserPrincipalName   = $User.UserID + '@adatum.com'
        Path                = 'OU=' + $User.Department + ',DC=adatum,DC=com'
        Department          = $User.Department
      }
      New-ADUser @UserSplat
    }
    ```
    </Strong></details> 

## Exercise 5: Querying disk information from remote computers

### Exercise scenario 5

Adatum hasn't documented the logical disk configuration for all of the computers. As part of gathering the information for your documentation, you're creating a script to gather logical disk information.

Your script for disk information has the following requirements:

- Accept the remote computer name as a parameter.
- If no computer name is provided as a parameter, the user should be prompted to enter a computer name.
- The query for information should use Web Services-Management (WS-MAN) rather than Distributed Component Object Model (DCOM).
- Display logical disk information (volumes) rather than physical disk information.
- Only information for local disks (hard drives) should be included.

The main task for this exercise is:

- Create a script that queries disk information with current credentials.


### Task 1: Create a script that queries disk information with current credentials

1. Perform all tasks using **LON-CL1**.
2. Using the WS-MAN protocol, What CIM cmdlet would query disk information
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *CIM* 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-CimInstance -ClassName Win32_LogicalDisk
    ```
    </Strong></details>
3. Identify the syntax required to query logical disk information and only include local disks (This means hard disks).
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Search the internet for Win32_logicalDisk
    # Find out which DriveType numbers relate to local disks
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-Ciminstance -ClassName Win32_LogicalDisk | Where-Object {$_.DriveType -eq 3}
    ```
    </Strong></details> 
5. Create a new script **E:\\Mod07\\QueryDisk.ps1**.
6. Add a **param()** block to the script that accepts a computer name and prompts for a computer name if one isn't provided.
7. Verify that the script correctly queries disk information from **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Parameters can have default values which are only used if the parameter is not used when running the script
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    param(
      [string]$ComputerName=(Read-Host "Enter computer name")
    )

    Get-CimInstance Win32_LogicalDisk -ComputerName $ComputerName | Where-Object {$_.DriveType -eq 3} 
    ```
    </Strong></details>

## Exercise 6: Updating the script to use alternate credentials

### Exercise scenario 6

You'd like to run a script that queries disk information from remote computers. To account for scenarios where the user doesn't have permission to query disk information on remote servers, you're updating the script to accept alternate credentials when specified.

The script needs to meet the following requirements:

- Accept a switch parameter to indicate whether alternate credentials are required.
- If alternate credentials are required, gather and use those credentials.

The main task for this exercise is:

- Update the script to use alternate credentials.

### Task 1: Update the script to use alternate credentials

1. Open **E:\\Mod07\\QueryDisk.ps1** for editing.
1. Update the **param()** block to include a switch that indicates alternate credentials will be used.
1. Add an **If** statement that evaluates the switch.

   - If the switch is true, run new code that gathers alternate credentials.
   - If the switch is false, run the existing query.

1. For new code, you must:
   - Get the credential from the user.
   - Open a remote session using that credential.
   - Send the query using that session.

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # The answer below show the script 
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    param(
      [string]$ComputerName=(Read-Host "Enter computer name"),
      [switch]$AlternateCredential
    )

    if ($AlternateCredential -eq $true) {
      $Cred = Get-Credential
      $Session = New-CimSession -ComputerName $ComputerName -Credential $Cred
      Get-CimInstance Win32_LogicalDisk -CimSession $Session | Where-Object {$_.DriveType -eq 3}
    } 
    else {
      Get-CimInstance Win32_LogicalDisk -ComputerName $ComputerName | Where-Object {$_.DriveType -eq 3}
    }
    ```
    </Strong></details>

[Go to next lab](AZ-040-Lab-08.md#_)
