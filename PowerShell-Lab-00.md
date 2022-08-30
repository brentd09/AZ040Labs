# Fix lab machines before starting labs

## Run this command on the LON-CL1 machine before starting the labs  

- Copy the PowerShell Command below
- Click the Start menu, and then just type ISE
- Start the "PowerShell ISE" console
- From the ISE's File menu choose New
- In the Untitled window pane of the PowerShell ISE editor paste the copied command, and then hit the green 'run script' icon in the toolbar


```PowerShell 


Invoke-Command -ComputerName LON-SVR1 -ScriptBlock {Set-NetFirewallProfile -All -Enabled false}


```
