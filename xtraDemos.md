# Demo code

## Module 01

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 02

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 03

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 04

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 05

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 06

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
## Module 07

### Foreach Demo

```PowerShell
$Computers = 'LON-Cl1','LON-DC1','LON-SVR1','LON-SVR2'
foreach  ($Computer in $Computers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### If Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### SWitch Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### For Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### While Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Until Demo


## Module 08

## Module 09

## Module 11
