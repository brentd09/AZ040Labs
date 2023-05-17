# Demo code

## Module 07

### Foreach Demo

```PowerShell
# Report Which Computers Respond
$Computers = 'LON-Cl1','LON-DC1','LON-SVR1','LON-SVR2'
foreach  ($Computer in $Computers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer -InformationLevel Quiet -WarningAction SilentlyContinue
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### If Demo

```PowerShell
# MasterMind Game
Clear-Host
$HiddenNumbers = 1..6 | Get-Random -Count 4
do {
  $WrongPos = 0
  $RightPos = 0
  do {
    [int[]]$Guess = (Read-Host -Prompt 'enter 4 numbers 1-6 with commas to separate').split(',')
  } until ($Guess.count -eq 4)
  foreach ($Index in 0..3) {
    if ($Guess[$Index] -eq $HiddenNumbers[$Index]) {$RightPos++}
    elseif ($Guess[$Index] -in $HiddenNumbers) {$WrongPos++}
  }
  Write-Host -ForegroundColor Cyan "$Guess - RightPosition=$RightPos WrongPosition=$WrongPos"
} until ($RightPos -eq 4)
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Switch Demo

```PowerShell
# Menu Script
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

```PowerShell
# Divide by a series of numbers
$Number = 345
for ($Count = 1; $Count -le 10; $Count++) {
  $Div = $Number / $Count
  Write-Host "$Number / $Count = $Div"
}
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### While Demo

```PowerShell
# Guessing Game
Clear-Host
$HiddenNumber = 1..100 | Get-Random
do {
  [int]$Guess = Read-Host -Prompt 'Enter a number from 1 to 100'
  if ($Guess -gt $HiddenNumber) {Write-Host 'Your Guess was too high'}
  elseif ($Guess -lt $HiddenNumber) {Write-Host 'Your Guess was too low'}
  else {Write-Host 'You guessed the right number'}
} while ($HiddenNumber -ne $Guess)
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Until Demo

```PowerShell
# Looping Menu Script
Clear-Host
do {
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
} until ($Choice -eq 3)  
```


### Break and Continue Demo

```PowerShell
# Find Prime Numbers
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

