# AZ-040Labs

## Rearranged the lab order to help your learning of the topics.
- Lab 2 has been placed toward the end of the week as the commands that you will see here will mostly be covered by other modules
- Labs 9 and 10 are relating to Microsoft 365 and Azure and are best dealt with after all of the on-prem coding is finished 

## Prepare labs before starting
- Run the fix commands below on LON-CL1 before running the labs<br> 
   ```PowerShell 
   Invoke-Command -ComputerName LON-SVR1,LON-DC1 -ScriptBlock {Set-NetFirewallProfile -All -Enabled false}
   Set-NetFirewallProfile -All -Enabled false
   ```

## PowerShell Labs

- Labs for learning PowerShell  
  [PowerShell Lab  1](PowerShell-Lab-01.md)<br>
  [PowerShell Lab  3](PowerShell-Lab-03.md)<br>
  [PowerShell Lab  4](PowerShell-Lab-04.md)<br>
  [PowerShell Lab  5](PowerShell-Lab-05.md)<br>
  [PowerShell Lab  6](PowerShell-Lab-06.md)<br>
  [PowerShell Lab  7](PowerShell-Lab-07.md)<br>
  [PowerShell Lab  8](PowerShell-Lab-08.md)<br>
  [PowerShell Lab  2](PowerShell-Lab-02.md)<br>
  [PowerShell Lab 11](PowerShell-Lab-11.md)<br>

- Labs for Microsoft 365 and Azure automation<br>
  [PowerShell Lab  9](PowerShell-Lab-09.md)<br>
  [PowerShell Lab 10](PowerShell-Lab-10.md)<br>
  
- Demo Code<br>
  [Demos](xtraDemos.md#demo-code) <br>  
