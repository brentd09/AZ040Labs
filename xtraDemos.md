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
  $ComputerResponded = Test-NetConnection -ComputerName $Computer -InformationLevel Quiet -WarningAction SilentlyContinue
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### If Demo

```PowerShell
$Numbers = 1..40
foreach  ($Number in $Numbers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer -InformationLevel Quiet -WarningAction SilentlyContinue
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Switch Demo

```PowerShell
Clear-Host
Write-Host Menu
Write-Host ----
Write-Host 
Write-Host 1...Add two numbers 
Write-Host 2...Multiply two numbers
Write-Host 3...Exit
Write-Host 
$Choice = Read-Host -Prompt 'Choose a menu number'
switch ($Choice) {
    1 {
      [double]$num1 = Read-Host -Prompt 'Enter the first number'
      [double]$num2 = Read-Host -Prompt 'Enter the second number'
      $num1 + $num2 
    }
    2 {
      [double]$num1 = Read-Host -Prompt 'Enter the first number'
      [double]$num2 = Read-Host -Prompt 'Enter the second number'
      $num1 * $num2 
    
    }
    3 {break}
    Default {Write-Host 'This was not a valid choice'}
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### For Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### While Demo

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Until Demo

### Break and Continue Demo

```PowerShell
$Numbers = 1..40
$MaxNumber = 140
$Numbers = 1..$MaxNumber
$DivideBys = 2..$MaxNumber
foreach  ($Number in $Numbers) {
  $IsPrime = $true
  if ($Number -eq 1 ) {continue}
  foreach ($DivideBy in $DivideBys) {
    $Remainder = $Number % $DivideBy
    if ($Remainder -eq 0 -and $Number -ne $DivideBy) {
      $IsPrime = $false
      break
    }
  }
  if ($IsPrime -eq $true) {$Number}
}
```
## Module 08

## Module 09

## Module 11
