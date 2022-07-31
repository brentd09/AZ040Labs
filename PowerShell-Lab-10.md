---
lab:
    title: 'Lab: Managing Microsoft 365 with PowerShell'
    module: 'Module 10: Managing Microsoft 365 services with PowerShell'
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


# Lab: Managing Microsoft 365 with PowerShell

## Exercise 1: Managing users and groups in Azure AD

### Task 1: Connect to Azure AD

1. On **LON-CL1**, right click **Start**, and then choose **Windows PowerShell (Admin)**.
1. Install the **AzureAD** module from PowerShell Gallery:
   
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *install*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
   ```PowerShell
   Install-Module AzureAD -Verbose -Force
   ```
    </Strong></details> 
   

1. Type "Y"  for yes on the "NuGet Provider is required to continue" dialog box and hit ENTER
2. Import the AzureAD module 

    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Import-Module AzureAD
    ```
    </Strong></details> 


4. Find and execute a PowerShell cmdlet that will connect to AzureAD and sign in

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command Connect*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Connect-AzureAD
    ```
    </Strong></details> 


1. In the **Sign in to your account** window, enter your username that is included in the Resources tab of the lab instructions, and then select **Next**.
1. At the **Enter password** prompt, enter the password provided in the Resource tab, and then select **Sign in**.
1. Find and execute a commant to find the users in AzureAD:

    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command Get*AzureAD*
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-AzureADUser
    ```
    </Strong></details>


### Task 2: Create a new administrative user

1. Create a **PasswordProfile** object:

    <details><summary>Click for hint</summary><Strong> 
    
    ```PowerShell
    Get-Help New-AzureADUser -Online
    # In the help document look for PasswordProfile
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```
    </Strong></details>


1. Think of a **Strong** password that you want to use for a new user and make a note of it so that you don't forget. In the next step, replace `<password>` with the password you thought of.
1.Set the password property of the password profile object:

   <details><summary>Click to see the answer</summary><Strong> 
    
   ```PowerShell
   $PasswordProfile.Password = "<password>"
   ```
   </Strong></details>


1. Create a new AzureAD User using the following command

    
   ```PowerShell
   $AzureADDomain = Get-AzureADDomain
   New-AzureADUser -DisplayName "Noreen Riggs" -UserPrincipalName Noreen@$($AzureADDomain.Name) -AccountEnabled $true -PasswordProfile $PasswordProfile -MailNickName Noreen
   ```



1. Place the new user in a variable, using the following command:


    
   ```PowerShell
   $user = Get-AzureADUser -ObjectID Noreen@$($AzureADDomain.Name)
   ```
  


1. Place the Global Administrator role in a variable, using the following command:

    
   ```PowerShell
   $role = Get-AzureADDirectoryRole | Where {$_.displayName -eq 'Global Administrator'}
   ```


1. Assign Noreen to the global administrator role, using the following command:


    
   ```PowerShell
   Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $user.ObjectID
   ```



1. To verify that Noreen is added to the global administrator role, enter the following command:


    
   ```PowerShell
   Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
   ```



### Task 3: Create and license a new user

1. Create a new AzureAD user, using the following command:

    
   ```PowerShell
   New-AzureADUser -DisplayName "Allan Yoo" -UserPrincipalName Allan@$($AzureADDomain.Name) -AccountEnabled $true -PasswordProfile $PasswordProfile -MailNickName Allan
   ```


1. Set the location for the user, using the following command:


    
   ```PowerShell
   Set-AzureADUser -ObjectId Allan@$($AzureADDomain.Name) -UsageLocation US
   ```



1. To review the available licenses in the tenant, enter the following command:

    
   ```PowerShell
   Get-AzureADSubscribedSku | Format-List
   ```


1. To place the SKU ID for the desired license in a variable, use the following command:

    
   ```PowerShell
   $SkuId = (Get-AzureADSubscribedSku | Where SkuPartNumber -eq "ENTERPRISEPREMIUM").SkuID
   ```



1. Create an **AssignedLicense** object, using the following command:


    
   ```PowerShell
   $License = New-Object -TypeName Microsoft.Open.AzureAD.Model.AssignedLicense
   ```


1. Add the SKU ID to the license object, using the following command:
    ong> 
    
   ```PowerShell
   $License.SkuId = $SkuId
   ```

1. Create an **AssignedLicenses** object, using the following command:


    
   ```PowerShell
   $LicensesToAssign = New-Object -TypeName Microsoft.Open.AzureAD.Model.AssignedLicenses
   ```


1. Add the **AssignedLicense** object to the **AddLicenses** property, using the following command:

 
    
   ```PowerShell
   $LicensesToAssign.AddLicenses = $License
   ```


1. To assign licences to Allan using the **AssignedLicenses** object, use the following command:

    
   ```PowerShell
   Set-AzureADUserLicense -ObjectId Allan@$($AzureADDomain.Name) -AssignedLicenses $LicensesToAssign
   ```


### Task 4: Create and populate a group

1. To review the existing groups, use the following command:


    
   ```PowerShell
   Get-AzureADGroup
   ```


1. Create a new security group, using the following command:
    

    
   ```PowerShell
   New-AzureADGroup -DisplayName "Sales Security Group" -SecurityEnabled $true -MailEnabled $false -MailNickName "SalesSecurityGroup"
   ```


1. Put the Sales Security Group in a variable, using the following command:

   
    
   ```PowerShell
   $group = Get-AzureAdGroup -SearchString "Sales Security"
   ```



1. Put Allan Yoo in a variable, in the console, using the following command:


    
   ```PowerShell
   $user = Get-AzureADUser -ObjectId Allan@$($AzureADDomain.Name)
   ```


1. Add "Allan Yoo" as a member of Sales Security Group, using the following command:

 
    
   ```PowerShell
   Add-AzureADGroupMember -ObjectId $group.ObjectId -RefObjectId $user.ObjectId
   ```



1. To verify Allan Yoo is a member of Sales Security Group, use the following command:


    
   ```PowerShell
   Get-AzureADGroupMember -ObjectId $group.ObjectId
   ```
   


## Exercise 2: Managing Exchange Online

### Task 1: Connect to Exchange Online
1. To install the **ExchangeOnlineManagement** module, use the following command:

   
    
   ```PowerShell
   Install-Module ExchangeOnlineManagement -Verbose -Force
   ```
   

1. Import the **ExchangeOnlineManagement** module, using the following command:

   
    
   ```PowerShell
   Import-Module ExchangeOnlineManagement
   ```
  
    
1. Connect to Exchange Online, using the following command:


    
   ```PowerShell
   Connect-ExchangeOnline
   ```
    


1. In the **Sign in to your account** window, 
    - Choose the Admin Login if it is offered as a sign-in option or
    - Enter the username provided in the lab instructions under the Resources tab, and then select **Next**.
1. If the **Enter password** prompt appears, enter your password, and then select **Sign in**.
1. To review a list of mailboxes in Exchange Online, use the following command:


    
   ```PowerShell
   Get-EXOMailbox
   ```



### Task 2: Create a room mailbox

1. To create a new room mailbox, use the following command:


    
   ```PowerShell
   New-Mailbox -Room -Name BoardRoom
   ```
 


1. To configure the new room to accept meeting requests, use the following command:

  
    
   ```PowerShell
   Set-CalendarProcessing BoardRoom -AutomateProcessing AutoAccept
   ```


### Task 3: Verify room resource booking

1. On **LON-CL1**, on the taskbar, select **Microsoft Edge**.
1. In Microsoft Edge, in the address bar, enter 
```
https://outlook.office.com
```
1. Sign in as Allan Woo 
```
allan@<Enter Your Tenant Name Here>  
```    
  > The Tenant name is provided in the instructions for the lab under the Resources tab Allan's singin name should be in this format **allan@LODSA682165.onmicrosoft.com**
  > change your password as instructed. 
  >  Be sure to note the password so that you can remember it for later exercises.

1. If prompted to stay signed in, select **No**.
1. From the menu bar, select **Calendar**, and then select **New event**.
1. In the **Add a title** box, enter **Staff Meeting**.
1. In the **Invite attendees** box, enter **BoardRoom**, select **BoardRoom**, select the first available time, and then select **Send**.
1. From the menu, select **Mail**.
1. Verify that Allan has received a response from **BoardRoom** that the meeting request was accepted.
1. Close Microsoft Edge.

## Exercise 3: Managing SharePoint Online

### Task 1: Connect to SharePoint Online

1. To install the SharePoint Online Management Shell, use the following command:

    
   ```PowerShell
   Install-Module -Name Microsoft.Online.SharePoint.PowerShell -Force -Verbose
   ```

1. Import the SharePoint Online Management module, using the following command:


    
   ```PowerShell
   Import-Module -Name Microsoft.Online.SharePoint.PowerShell 
   ```
 

1. To connect to SharePoint Online, use the following command:

   
   ```PowerShell
   $SPODomain = $AzureADDomain.Name -Replace '\..*$','-admin.sharepoint.com'    
   Connect-SPOService -Url https://$($SPODomain)
   ```

1. Sign in as Noreen Riggs and change your password as instructed. Be sure to note the password so that you can remember it for later exercises.
1. To review the existing sites, use the following command:

    
   ```PowerShell
   Get-SPOSite
   ```

### Task 2: Create a new site

1. To review the available templates, use the following command:
    
   ```PowerShell
   Get-SPOWebTemplate
   ```

1. To create a new site, in the console, use the following command:

    
   ```powershell
   $SPODomainForNewSite = $AzureADDomain.Name -Replace '\..*$','.sharepoint.com'    
   New-SPOSite -Url https://$($SPODomainForNewSite)/sites/Sales -Owner noreen@$($AzureADDomain.Name) -StorageQuota 256 -Template EHS#1 -NoWait
   ```

    > **Note:** It might take a few minutes for this command to complete processing.

1. To verify the status of the SharePoint site, in the console, use the following command:
    
  
    
   ```PowerShell
   Get-SPOSite | Format-List Url,Status
   ```

   > **Note:** Creating the site can take 10 minutes or longer. Don't wait for site creation to complete.

1. To disconnect from SharePoint Online, use the following command:
    
     
   ```PowerShell
   Disconnect-SPOService
   ```
  
## Exercise 4: Managing Microsoft Teams

### Task 1: Connect to Microsoft Teams

1. To install the SharePoint Online Management Shell, use the following command:


    
   ```PowerShell
   Install-Module MicrosoftTeams -Force -Verbose
   ```
 

1. Import the SharePoint Online module, use the following command::

    
   ```PowerShell
   Import-Module MicrosoftTeams 
   ```

1. To connect to Microsoft Teams, use the following command:


    
   ```PowerShell
   Connect-MicrosoftTeams
   ```



1. Sign in as your admin user. Notice that you can't use Noreen for this activity, because you need a Microsoft Teams license to create teams.
1. To verify that there are no existing teams, use the following command:

      
   ```PowerShell
   Get-Team
   ```
 
### Task 2: Create a new team

1. To create a Sales team, use the following command:

       
   ```PowerShell
   New-Team -DisplayName "Sales Team" -MailNickName "SalesTeam"
   ```
    > This can take some time to create the team
    
1. To place the team information in a variable, use the following command:

 
    
   ```PowerShell
   $team = Get-Team -DisplayName "Sales Team"
   ```
  
1. To review the information about your team, use the following command:

  
    
   ```PowerShell
   $team | Format-List
   ```
 
    > If you get no results from this command wait a minute and try steps 2 and 3 again

1. Review the information about the Sales Team. Notice that **GroupId** is a unique identifier.
1. To add a user to the team, in the console, use the following command:

  
    
   ```PowerShell
   Add-TeamUser -GroupId $team.GroupId -User Allan@$($AzureADDomain.Name) -Role Member
   ```
  


1. To review the team users, use the following command:

   
    
   ```PowerShell
   Get-TeamUser -GroupId $team.GroupId
   ```
 
   > **Note:** Notice that the user that created the team is an owner.

## Task 3: Verify access to the team

1. On **LON-CL1**, on the taskbar, select **Microsoft Edge**.
1. In Microsoft Edge, in the address bar, enter 
```
https://teams.microsoft.com
```    
1. Sign in as Allan Yoo.
1. If prompted to stay signed in, select **No**.
1. Close the **Bring your team together** window, and then verify that **Sales Team** is listed.
1. Select **New conversation**, enter **Prices are increasing 10% at month end**, and then select Enter.
1. Close Microsoft Edge.

## Congratulations you have finished this lab    

[Go to next lab](AZ-040-Lab-11.md#_)
