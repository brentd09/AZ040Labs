# AZ-040Labs

## Rearranged the lab order to help your learning of the topics.
- Lab 2 has been placed toward the end of the week as it is a much better placement for this module
- Labs 9 and 10 are related to Microsoft 365 and Azure and are best dealt with after all of the non-cloud modules are completed. 

## Prepare labs before starting 
- A Firewall issue in the lab machines prevents some labs from running correctly
- Run this fix script below from the LON-CL1 lab machine, before running the labs.<br> 
   ```PowerShell 
   Invoke-Command -ComputerName LON-SVR1 -ScriptBlock {
     $Params = @{
       Name='FixForPSLab'
       DisplayName = 'FixForPSLab' 
       Enabled = 'True' 
       Direction = 'Inbound' 
       Action = 'Allow'
       RemoteAddress = '172.16.0.0/16'
     }
     New-NetFirewallRule @Params
   }
   ```
- if using PowerShell 7.x, **Get-EventLog** command is a now a legacy command
  - use **Get-WinEvent** instead when accessing event log information, especially on remote machines

## PowerShell Labs

- Labs for learning PowerShell  
  [Lab instructions for Module 1](PowerShell-Lab-01.md)<br>
  [Lab instructions for Module 3](PowerShell-Lab-03.md)<br>
  [Lab instructions for Module 4](PowerShell-Lab-04.md)<br>
  [Lab instructions for Module 5](PowerShell-Lab-05.md)<br>
  [Lab instructions for Module 6](PowerShell-Lab-06.md)<br>
  [Lab instructions for Module 7](PowerShell-Lab-07.md)<br>
  [Lab instructions for Module 8](PowerShell-Lab-08.md)<br>
  [Lab instructions for Module 2](PowerShell-Lab-02.md)<br>
  [Lab instructions for Module 11](PowerShell-Lab-11.md)<br>

- Labs for Microsoft 365 and Azure automation<br>
  [Lab instructions for Module 9](PowerShell-Lab-09.md)<br>
  [Lab instructions for Module 10](PowerShell-Lab-10.md)<br>
  
- Extra Demo Code<br>
  [Demos](xtraDemos.md#demo-code) <br>  
