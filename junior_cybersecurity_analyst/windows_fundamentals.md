### NFTS vs  Share Permissions

```bash
smbclient -L SERVER_IP -U htb-student
smbclient '\\SERVER_IP\Company Data' -U htb-student

sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
sudo apt-get install cifs-utils

net share
```

### Windows Services & Processes

```bash
Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
Get-Service | ? {$_.Status -eq "Running"} | FL
Get-Service '*Reader*' | Where-Object {$_.Status -eq "Running"} | FL

\\live.sysinternals.com\tools\procdump.exe -accepteula
```


```bash
sc qc wuauserv
sc //hostname or ip of box query ServiceName
sc stop wuauserv
sc config wuauserv
sc sdshow wuauserv

Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List


```

### Interacting with the Windows Operating System

```bash
help schtasks
ipconfig /?

get-alias
New-Alias -Name "Show-Files" Get-ChildItem
Get-Alias -Name "Show-Files"

Get-Help Get-AppPackage
.\PowerView.ps1;Get-LocalGroup |fl
Get-Module | select Name,ExportedCommands | fl
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
Get-ExecutionPolicy -List
```

### WMI

```bash
wmic /?
Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft
Invoke-WmiMethod -Path "CIM_DataFile.Name='C:\users\public\spns.csv'" -Name Rename -ArgumentList "C:\Users\Public\kerberoasted_users.csv"

```


### Windows Security

```bash
whoami /user
gci -Hidden
Get-WmiObject -Class Win32_Account -Filter "Name='bob.smith'"

Reg Query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run"
```

### Assesment

```bash

```
