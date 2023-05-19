# Demo code
<details><summary><h2>Module 7</h2></summary><Strong> 

## Module 07

### Demo Foreach 

```PowerShell
# Report Which Computers Respond
$Computers = 'LON-Cl1','LON-DC1','LON-SVR1','LON-SVR2'
foreach  ($Computer in $Computers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer -InformationLevel Quiet -WarningAction SilentlyContinue
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### Demo if, do..until, nested loops

```PowerShell
# MasterMind Game
Clear-Host
$HiddenNumbers = 1..6 | Get-Random -Count 4
do {
  $WrongPos = 0
  $RightPos = 0
  do {
    [int[]]$Guess = (Read-Host -Prompt 'Enter 4 numbers 1-6 with commas to separate').split(',')
    $Guess = $Guess | Select-Object -Unique
    $HighestNumber = ($Guess | Sort-Object -Descending)[0] 
  } until ($Guess.count -eq 4 -and $HighestNumber -le 6 )
  foreach ($Index in 0..3) {
    if ($Guess[$Index] -eq $HiddenNumbers[$Index]) {$RightPos++}
    elseif ($Guess[$Index] -in $HiddenNumbers) {$WrongPos++}
  }
  Write-Host -ForegroundColor Yellow "$Guess -   RightPosition = $RightPos    WrongPosition = $WrongPos   " 
} until ($RightPos -eq 4)
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo Switch

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
### Demo For

```PowerShell
# Divide by a series of numbers
$Number = 345
for ($Count = 1; $Count -le 10; $Count++) {
  $Div = $Number / $Count
  Write-Host "$Number / $Count = $Div"
}
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo While, Switch 

```PowerShell
# Guessing Game
Clear-Host
$Turns = 0
$HiddenNumber = 1..100 | Get-Random
do {
  [int]$Guess = Read-Host -Prompt 'Enter a number from 1 to 100'
  $Turns++
  switch ($Guess) {
    {$_ -gt $HiddenNumber} {Write-Host 'Your Guess was too high'}
    {$_ -lt $HiddenNumber} {Write-Host 'Your Guess was too low'}
    default {Write-Host "You Guessed the number correctly, it took you $Turns turns"}
  }
} while ($HiddenNumber -ne $Guess)
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo Do..While, Switch, Break

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
} while ($Choice -ne 3)  
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo Break, Continue, Foreach, ArrayList 

```PowerShell
# Find Prime Numbers
[System.Collections.ArrayList]$Primes = @()
Write-Host "Prime Numbers"
$MaxNumber = 50
$Numbers = 1..$MaxNumber
foreach  ($Number in $Numbers) {
  if ($Number -eq 1 ) {continue}
  $DivideBys = 2..$Number
  $IsPrime = $true
  foreach ($DivideBy in $DivideBys) {
    $Remainder = $Number % $DivideBy
    if ($Remainder -eq 0 -and $Number -ne $DivideBy) {
      $IsPrime = $false
      break
    }
  }
  if ($IsPrime -eq $true) {$Primes += $Number}
}
$Primes
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo Break, Continue, Foreach, ArrayList, Function, Param, PSCustomObject

```PowerShell
function Get-Prime {
  [cmdletbinding()]
  param (
     [int]$MaxNumber = 50
  )
  # Find Prime Numbers
  [System.Collections.ArrayList]$Primes = @()
  $Numbers = 1..$MaxNumber
  foreach  ($Number in $Numbers) {
    if ($Number -eq 1 ) {continue}
    $DivideBys = 2..$Number
    $IsPrime = $true
    foreach ($DivideBy in $DivideBys) {
      $Remainder = $Number % $DivideBy
      if ($Remainder -eq 0 -and $Number -ne $DivideBy) {
        $IsPrime = $false
        break
      }
    }
    if ($IsPrime -eq $true) {$Primes += $Number}
  }
  return [PSCustomObject]@{
    Primes     = $Primes
    MaxPrime   = $Primes[-1]
    PrimeCount = $Primes.count
  }
}

$PrimeData = Get-Prime -MaxNumber 10
$PrimeData
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

</Strong></details> 
