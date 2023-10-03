# Demo code
<h2>Module 01</h2>

<h2>Module 02</h2>

<h2>Module 03</h2>   




<details><summary><h2>Module 04</h2></summary><Strong> 

  ```PowerShell
  Install-Module -Name PipelineDemo -Force                     # Install this before trying any of these examples
  ```
  ### Get-OpenTCPPortByVal
  #### Try ByValue pipeline

  ```PowerShell
  # ByValue Pipeline
  Get-ADComputer -Filter *          |        Get-OpenTCPPortByVal

  #
  # Get-Member shows type                    Get-Help shows:   
  # of [ADComputer] --------------> |----->  -Computer [ADComputer]
  #                                 |         Pipeline=True (ByValue)
  #                                 |
  #                                 |        -TcpPort [int]
  #                                 |         Pipeline=False

  # We can pipe the entire [ADComputer] object to Get-OpenTCPPortByVal
  # because of these two reasons:
  #   1. The -Computer parameter (from Get-OpenTCPPortByVal) can accept pipeline using ByValue {pipeline=True  ByValue}
  #   2. The type for the parameter -Computer matches the object produced by
  #      the "Get-ADComputer -Filter *" command {[ADComputer] = [ADComputer]} 
  ```
  #### ByValue pipeline succeeds
  
  
  ---
  
  ### Get-OpenTCPPortByPN
  #### Always try ByValue pipeline first
  
  ```PowerShell
  Get-ADComputer -Filter *          |        Get-OpenTCPPortByPN

  # This command produces           |        This command does NOT accept 
  # an [ADComputer] object          |        [ADComputer] objects ByValue
  
  # Get-Member shows type                    Get-Help shows:
  # of [ADComputer] ------------> X | X      -Name [String]
  #                                 |         Pipeline=True (ByPropertyName)
  #                                 |
  #                                 |        -TcpPort [int]
  #                                 |         Pipeline=False

  # This fails because there are no parameters in the Get-OpenTCPPortByPN command
  # that accept pipeline using ByValue that also have the type [ADComputer]
  ```
  #### ByValue pipeline failed --> PowerShell now tries the ByPropertyName pipeline

  ---
  
  ### Get-OpenTCPPortByPN  
  #### Resorting to ByPropertyName pipeline
  
  ```PowerShell
  Get-ADComputer -Filter *          |        Get-OpenTCPPortByPN

  # When Unpacking the [ADComputer] |        This command has the following
  # object we find these            |        parameters:
  # properties:                     |
  
  # Get-Member shows:                        Get-Help shows:
  #  Name              [String] --->|----->  -Name [string]  
  #  DNSHostName       [String]     |         pipeline=True  ByPropertyName
  #  Enabled           [Boolean]    |  
  #  DistinguishedName [String]     |        -TcpPort <Int32>
  #  ObjectClass       [String]     |         Pipeline=False
  #  ObjectGUID        [Guid]       |
  #  SamAccountName    [String]     |
  #  SID               [SID]        |
  #  UserPrincipalName [String]     |

  # We can pipe the value of the contents of the Name property to Get-OpenTCPPortByPN
  # because of these three reasons:
  #   1. The -Name parameter (from Get-OpenTCPPortByPN) can accept pipeline using ByPropertyName {pipeline=True  ByPropertyName}
  #   2. The property and parameter and property names are spelt exactly the same {Name = Name}
  #   3. The types for both of the property and parameter are the same {[string] = [string]} 
  ```
  #### ByPropertyName succeeds

  <br>

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
  
</Strong></details> 

<h2>Module 05</h2>

<h2>Module 06</h2> 

<details><summary><h2>Module 07</h2></summary><Strong> 


### Demo: Foreach 

```PowerShell
# Report Which Computers Respond
$Computers = 'LON-Cl1','LON-DC1','LON-SVR1','LON-SVR2'
foreach  ($Computer in $Computers) {
  $ComputerResponded = Test-NetConnection -ComputerName $Computer -InformationLevel Quiet -WarningAction SilentlyContinue
  Write-Host "Computer $Computer responded: $ComputerResponded" 
}
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### Demo: if, do..until, nested loops

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
### Demo: Switch

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
### Demo: For

```PowerShell
# Divide by a series of numbers
$Number = 345
for ($Count = 1; $Count -le 10; $Count++) {
  $Div = $Number / $Count
  Write-Host "$Number / $Count = $Div"
}
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo: Do..While, If 

```PowerShell
# Guessing Game
Clear-Host
$Turns = 0
$HiddenNumber = 1..100 | Get-Random
do {
  [int]$Guess = Read-Host -Prompt 'Enter a number from 1 to 100'
  $Turns++
  if ($Guess -gt $HiddenNumber) {Write-Host 'Your Guess was too high'}
  elseif ($Guess -lt $HiddenNumber) {Write-Host 'Your Guess was too low'}
  else {Write-Host "You Guessed the number correctly, it took you $Turns turns"}
  }
} while ($HiddenNumber -ne $Guess)
```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)
### Demo: Do..While, Switch, Break

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
### Demo: Break, Continue, Foreach, ArrayList 

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
      break # We already know this is not a prime now so no use going through the rest of the loop
    }
  }
  if ($IsPrime -eq $true) {$Primes += $Number}
}
$Primes
```
[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)


</Strong></details> 

<h2>Module 08</h2>

<h2>Module 09</h2>

<h2>Module 10</h2>

<h2>Module 11</h2>

<details><summary><h2>Advanced Concepts</h2></summary><Strong> 
### Demo: Function, Param, PSCustomObject 
 
```PowerShell
function Get-IpConfig {
  [cmdletbinding()]
  Param(
    [switch]$All
  )
  $LiveAdapter = Get-NetAdapter -Physical | Where-Object {$_.status -eq 'up'}
  $IP4 = Get-NetIPAddress -AddressFamily IPv4 | where-Object {$_.ifIndex -eq $LiveAdapter.ifIndex}
  $DNSAddr = Get-DnsClientServerAddress -AddressFamily IPv4| where-Object {$_.InterfaceIndex -eq $LiveAdapter.ifIndex}
  $IPConfig = Get-NetIPInterface -InterfaceIndex 5 -AddressFamily IPv4
  $Routes = Get-NetRoute -AddressFamily IPv4
  if ($All -eq $true){
    return [PSCustomObject][ordered]@{
      InterfaceAlias = $LiveAdapter.ifAlias
      InterfaceIndex = $LiveAdapter.ifIndex
      MACAddress     = $LiveAdapter.LinkLayerAddress
      IPV4Address    = $IP4.IPAddress
      IPV4MaskLength = $IP4.PrefixLength
      DNSServer      = $DNSAddr.Address
      DHCPEnabled    = $IPConfig.Dhcp
      DefaultGateway = ($Routes | Where-Object {$_.DestinationPrefix -match '0.0.0.0' -and $_.IfIndex -eq $LiveAdapter.ifIndex}).NextHop
    }
  }
  else { 
    return [PSCustomObject][ordered]@{
      InterfaceAlias = $LiveAdapter.ifAlias
      IPV4Address    = $IP4.IPAddress
      IPV4MaskLength = $IP4.PrefixLength
    }
  }
}

Get-IpConfig
# Get-IpConfig -All

```

[Back to labs](https://github.com/brentd09/AZ040Labs/blob/main/README.md#powershell-labs)

### Demo: Break, Continue, Foreach, ArrayList, Function, Param, PSCustomObject

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
