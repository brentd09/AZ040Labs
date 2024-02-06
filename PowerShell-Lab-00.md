# Fix lab machines before starting labs

## Run this command on the LON-CL1 machine before starting the labs  

- Copy the PowerShell Command below
- Click the Start menu, and then just type ISE
- Start the "PowerShell ISE" console
- From the ISE's File menu choose New
- In the Untitled window pane of the PowerShell ISE editor paste the copied command, and then hit the green 'run script' icon in the toolbar


```PowerShell 

Invoke-Command -ComputerName LON-SVR1 -ScriptBlock {
  $Params = @{
    Name='FixForLab'
    DisplayName = 'FixForLab' 
    Enabled = 'True' 
    Direction = 'Inbound' 
    Action = 'Allow' 
    Protocol = 'TCP' 
    LocalPort = 49670 
    Profile = 'Any'
  }
  New-NetFirewallRule @Params
}

```
