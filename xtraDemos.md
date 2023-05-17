# Demo code

## Module 01

## Module 02

## Module 03

## Module 04

## Module 05

## Module 06

## Module 07

### Foreach Demo

```PowerShell
$Computers = 'LON-Cl1','LON-DC1','LON-SVR1','LON-SVR2'
foreach  ($Computer in $Computers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```


### If Demo

### SWitch Demo

### For Demo

### While Demo

### Until Demo

## Module 08

## Module 09

## Module 11
