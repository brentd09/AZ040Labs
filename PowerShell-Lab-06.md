---
lab:
    title: 'Lab: Using variables, arrays, and hash tables in PowerShell'
    module: 'Module 6: Working with variables, arrays, and hash tables'
---


<!--
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # HINT
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    # ANSWER
    ```
    </Strong></details> 
-->

###### _

# Lab: Using variables, arrays, and hash tables in PowerShell

## Scenario

You're preparing to create scripts to automate server administration in your organization. Before you begin, you want to practice working with variables, arrays, and hash tables.

## Objectives

After completing this lab, you'll be able to:

- Work with variable types.
- Use arrays.
- Use hash tables.

## Estimated time: 45 minutes

## Lab setup

Virtual machines: **AZ-040T00A-LON-DC1**, **AZ-040T00A-LON-SVR1**, and **AZ-040T00A-LON-CL1**

Username: **Adatum\\Administrator**

Password: **Pa55w.rd**

For this lab, you'll use the available virtual machine environment. Before you begin the lab, complete the following steps:

1. Open **LON-DC1** and sign in as **Adatum\\Administrator** with the password **Pa55w.rd**.
1. Repeat step 1 for **LON-SVR1** and **LON-CL1**.

## Exercise 1: Working with variable types

### Exercise scenario 1

You first plan to practice working with different types of variables.

The main tasks for this exercise are:

1. Use string variables.
1. Use DateTime variables.

### Task 1: Use string variables

1. On **LON-CL1**, open Windows PowerShell.
1. Create a variable `$logPath` that contains **C:\logs**\.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help about_Variables -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logPath = "C:\Logs\"
    ```
    </Strong></details> 

3. For `$logPath`, identify the type of variable and the available properties and methods.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logPath | Get-Member
    ```
    </Strong></details> 

4. Create a variable `$logFile` that contains **log.txt**.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logFile = "log.txt"
    ```
    </Strong></details> 

5. Update `$logPath` to include the contents of `$logFile`.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logPath = $logPath + $logFile
    # Or you could write this in a shorthand version:
    # $logPath += $logFile
    ```
    </Strong></details> 
    <details><summary>Click to see an advanced answer</summary><Strong> 
    
    ```PowerShell
    $logPath = $logPath.TrimEnd('\') + '\' + $logFile
    # This solution provides a way to fix the proble of not including a '\' backslash 
    # when defining the original $LogPath, for example:
    # $LogPath = "C:\Logs"
    ```
    </Strong></details> 

6. Update the path stored in `$logPath` to use drive **D** instead of drive **C**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    $logPath | Get-Member
    # look for methods that will help
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logPath.Replace("C:","D:")
    # The ':' is included here so that the C: will be replaced with D:
    # If $logPath.Replace("C","D") was used this would replace every 
    # occurrence of C with D not just at the start.
    ```
    </Strong></details> 

7. Verify the variable has been modified.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Command *Date*
    # look for a command that will get the current date
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
     $logPath
    ```
    </Strong></details> 
8. Leave the PowerShell window open 

### Task 2: Use DateTime variables

1. At the Windows PowerShell prompt, create a variable `$today` that contains today’s date.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $today = Get-Date
    ```
    </Strong></details> 
2. For the variable `$today`, identify the type of variable and the available properties and methods.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $today | Get-Member
    ```
    </Strong></details> 
3. Use the properties of `$today` to create a string in the format **Year-Month-Day-Hour-Minute.txt**, and store the value in `$logFile`.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # When adding objects together the first object type (left most object) 
    # is what all of the other objects are converted to, if possible.
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $logFile = "" + $today.Year + "-" + $today.Month + "-" + $today.Day + "-" + $today.Hour + "-" + $today.Minute + ".txt"
    # OR 
    # $logFile = [string]$today.Year + "-" + $today.Month + "-" + $today.Day + "-" + $today.Hour + "-" + $today.Minute + ".txt"
    ```
    </Strong></details> 
4. Create a variable `$cutOffDay` that contains the date **30** days before today.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    $today | Get-Member
    # Check for methods that would help
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $cutOffDate = $today.AddDays(-30)
    ```
    </Strong></details> 
5. Use **Get-ADUser** to query user accounts that have signed in since `$cutOffDay`. Filter by using the **LastLogonDate** property.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-ADUser -Filter * -Properties * | Get-Member
    # The -Properties parameter is required as ActiveDirectory PowerShell cmdlets only return 
    # a very limited number of properties (this is very different to most other PS cmdlets)
    #
    # look for a property that would show that last time a logon occurred
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-ADUser -Filter * -Properties LastLogonDate | Where-Object {$_.LastLogonDate -gt $cutOffDate}
    ```
    </Strong></details> 
6. Leave the Windows PowerShell prompt open for the next exercise.

## Exercise 2: Using arrays

### Exercise scenario 2

Now that you've practiced using different types of variables, you want to work with arrays.

The main tasks for this exercise are:

1. Use an array to update the department for users.
1. Use an array list.

### Task 1: Use an array to update the department for users

1. Query all Active Directory Domain Services (AD DS) users in the **Marketing** department and place them in a variable named `$mktgUsers`. Include the **Department** property in the results.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Get-ADUser -ShowWindow
    # Read the help section that refers to the -Filter parameter
    # Learning how to filter from the Get-ADUser cmdlet will speed up your scripts
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mktgUsers = Get-ADUser -Filter {Department -eq "Marketing"} -Properties Department
    ```
    </Strong></details> 
1. Use `$mktgUsers` to identify the number of users in the Marketing department.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # All arrays automatically get a Count property as part of the collection
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mktgUsers.Count
    ```
    </Strong></details> 
1. Display the first user in `$mktgUsers` and verify that the **Department** property is listed.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help about_Arrays -ShowWindow
    # this will show how to use an "index" to get specific members of the array
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mktgUsers[0]
    ```
    </Strong></details> 
1. Pipe the users in `$mktgUsers` to **Set-ADUser** and update the department to **Business Development**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Set-ADUser -ShowWindow
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mktgUsers | Set-ADUser -Department "Business Development"
    ```
    </Strong></details> 
1. Review the **Department** attribute in `$mktgUsers` to check if it has been updated.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mktgUsers | Select-Object Name,Department
    ```
    </Strong></details> 
1. Query all AD DS users in the **Marketing** department to verify that there are none.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Help Get-ADUser -ShowWindow
    # Search for the -Filter parameter to show how to construct filters for Get-ADUser
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-ADUser -Filter {Department -eq "Marketing"}
    ```
    </Strong></details> 
1. Query all AD DS users in the **Business Development** department and verify that it matches the previous count from the Marketing department.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    Get-ADUser -Filter {Department -eq "Business Development"}
    ```
    </Strong></details> 
1. Leave the Windows PowerShell prompt open for the next task.

### Task 2: Use an array list

1. Create an arraylist named `$computers` with the values **LON-SRV1**, **LON-SRV2**, and **LON-DC1**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # User tab completion when creating an array list:
    # by typing [ArrayList] and then hitting the tab key it will 
    # automatically complete the full name [System.Collections.ArrayList]
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    [System.Collections.ArrayList]$computers="LON-SRV1","LON-SRV2","LON-DC1"
    ```
    </Strong></details> 
2. Verify that `$computers` doesn't have a fixed size.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    $Computers | Get-Member
    # There is a property that will show if this is a fixed size, ArrayLists are not
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers.IsFixedSize
    ```
    </Strong></details> 
3. Add **LON-DC2** to `$computers`.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Member -InputObject $Computers
    # Shows how you can use ArrayList methods
    #
    # when working with unfamiliar methods you can run the method without the brackets
    # this will show the overloads for this method instructing you what types of data
    # needs to be typed in to the brackets and how many bits of data need to be entered
    # Example: $Computers.Add
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers.Add("LON-DC2")
    ```
    </Strong></details> 
4. Remove **LON-SRV2** from `$computers`.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    Get-Member -InputObject $Computers
    # Shows how you can use ArrayList methods
    #
    # when working with unfamiliar methods you can run the method without the brackets
    # this will show the overloads for this method instructing you what types of data
    # needs to be typed in to the brackets and how many bits of data need to be entered
    # Example: $Computers.Remove
    ```
    </Strong></details>  
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers.Remove("LON-SRV2")
    ```
    </Strong></details> 
5. Display the contents of `$computers`.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $computers
    ```
    </Strong></details> 
6. Leave the Windows PowerShell prompt open for the next exercise.

## Exercise 3: Using hash tables

### Exercise scenario 3

After using variables and arrays, you plan to practice working with hash tables. You want to learn how working with hash tables differs from arrays and array lists.

The main task for this exercise is:

- Use a hash table.

### Task 1: Use a hash table

1. Create a hash table named `$mailList` with the following users and email addresses:

   - User **Frank** with the email address **Frank@fabrikam.com**
   - User **Libby** with the email address **LHayward@contoso.com**
   - User **Matej** with the email address **MStojanov@tailspintoys.com**
   <br>
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Hash tables are created with this construct @{Key = Value;...}: 
    # For Example
    # @{
    #    Name1 = 'Value'
    #    Name2 = 'Value'
    # }
    #
    # Hashtables must have unique key names
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList = @{
      Frank="Frank@fabriakm.com"
      Libby="LHayward@contso.com"
      Matej="MSTaojanov@tailspintoys.com"
    }
    ```
    
    ```PowerShell
    $mailList = [ordered]@{
      Frank="Frank@fabriakm.com"
      Libby="LHayward@contso.com"
      Matej="MSTaojanov@tailspintoys.com"
    }
    # The difference is this creates an ordered set of values, whereas the previous may be reordered by PowerShell 
    ```
    
    </Strong></details> 

2. Display the contents of `$mailList`.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList
    ```
    </Strong></details> 
3. Display the email address for **Libby**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # you can access the values by treating the key-names as properties
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList.Libby
    ```
    </Strong></details> 
4. Update the email address for **Libby** to **Libby.Hayward@contoso.com**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Assinging a new value to an existing hashtable key is just like assigning a value to a variable
    Get-Help About_Hash_Tables
    ```
    </Strong></details>
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList.Libby="Libby.Hayward@contoso.com"
    ```
    </Strong></details> 
5. Add a new email address for **Stela**: **Stela.Sahiti@treyresearch.net**.
    <details><summary>Click for hint</summary><Strong> 

    ```PowerShell
    # Use ArrayList methods to manipulate the ArrayList
    ```
    </Strong></details> 
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList.Add("Stela","Stela.Sahiti")
    ```
    </Strong></details> 
7. Remove **Frank** from `$mailList`.
    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList.Remove("Frank")
    ```
    </Strong></details> 
8. Verify that **Frank** is removed.

    <details><summary>Click to see the answer</summary><Strong> 
    
    ```PowerShell
    $mailList
    ```
    </Strong></details> 
9. Close the Windows PowerShell prompt.

[Go to next lab](AZ-040-Lab-07.md#_)
