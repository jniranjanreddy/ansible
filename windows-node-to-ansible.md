## How to add windows node to Ansible
```
https://www.techbeatly.com/configure-your-windows-host-to-manage-by-ansible/
https://www.techbeatly.com/how-to-open-winrm-ports-in-the-windows-firewall/
```

## Prerequisite
```
 1. Powershell
 2. .Net
 3. winrm
```
## How to check .net Version.
```
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse | Get-ItemProperty -Name version -EA 0 | Where { $_.PSChildName -Match '^(?!S)\p{L}'} | Select PSChildName, version

```
## How to check PowerShell version
```
$PSVERSIONTABLE
```
## How to check winrm
```
```
## create vars file..
```
mkdir /etc/ansible/group_vars
vi /etc/ansible/group_vars
root@awxdev:/etc/ansible/group_vars# cat win.yaml
---
ansible_user: "ansible"
ansible_password: "ansible"
ansible_connection: "winrm"
ansible_winrm_server_cert_validation: "ignore"
ansible_winrm_transport: "basic"
ansible_ssh_port: "5985"

```

Executing winrm from ubuntu through pythn
```
>>> import winrm
>>> session = winrm.Session('192.168.9.125', auth=('windevops','password'), transport='ntlm')
>>> result = session.run_ps("hostname")
>>> print(result.std_out)
b'DESKTOP-BVQIU5U\r\n'
```
# verify winrm port from linux machine
```
root@awxdev:/etc/ansible/group_vars# nc -vz 192.168.9.125 5985
Connection to 192.168.9.125 5985 port [tcp/*] succeeded!
```

```
PS C:\Users\Administrator> (Get-Host).Version

Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  693

```
##If PowerShell isn’t using TLSv1.2, configure it to use it by default by running:
[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12




```
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

PS C:\Users\Administrator> $url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
PS C:\Users\Administrator> $file = "$env:temp\ConfigureRemotingForAnsible.ps1"
PS C:\Users\Administrator> (New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
PS C:\Users\Administrator> powershell.exe -ExecutionPolicy ByPass -File $file

$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file

```
NOTE: For win2019 Execute the Command -  winrm set winrm/config/service '@{AllowUnencrypted="true"}'
```
 # Debugging winrm
 
 # Enable winrm service
 ```
 Enable-PSRemoting -Force
 ```
 # Verify the network configuration of the WinRM service.
 ```
 winrm enumerate winrm/config/listener
 ```
#  In our example, the WinRM service is allowing HTTP connections.
# Verify the WinRM service configuration
```
winrm get winrm/config/service
```
# Test the local connection to the WinRM service.
```
Test-WsMan 127.0.0.1
```
# Test the connection to the remote computer on the TCP port 5985.
```
(New-Object System.Net.Sockets.TcpClient).ConnectAsync('52.37.189.81', 5985).Wait(1000)
```
# Test the connection to the remote WinRM service.
```
Test-WSMan -ComputerName 52.37.189.81
```
# Execute a remote command using WinRM and Powershell.
```
Invoke-Command -ComputerName 52.37.189.81 -ScriptBlock { ipconfig } -credential administrator
```
# Test the connection to the remote WinRM service.
```
Test-WSMan -ComputerName 52.37.189.81
```

# Start a remote session using Powershell and WinRM.
 ```
 Enter-PSSession -ComputerName 52.37.189.81 -Credential administrator
 ```
 
 
 
 
 ``` 
 From CMD, start the WinRM service and load the default WinRM configuration.

 PS C:\Windows\system32>  winrm quickconfig
WinRM service is already running on this machine.
WSManFault
    Message
        ProviderFault
            WSManFault
                Message = WinRM firewall exception will not work since one of the network connection types on this machine is set to Public. Change the network connection type to either Domain or Private and try again.

Error number:  -2144108183 0x80338169
WinRM firewall exception will not work since one of the network connection types on this machine is set to Public. Change the network connection type to either Domain or Private and try again.
PS C:\Windows\system32>

PS C:\Windows\system32> winrm enumerate winrm/config/listener
Listener
    Address = *
    Transport = HTTP
    Port = 5985
    Hostname
    Enabled = true
    URLPrefix = wsman
    CertificateThumbprint
    ListeningOn = 127.0.0.1, 169.254.3.85, 169.254.62.191, 169.254.94.172, 169.254.108.220, 169.254.185.55, 169.254.217.165, 192.168.9.253, 192.168.56.1, ::1, 2405:201:c03d:405d:5150:e106:c947:54da, 2405:201:c03d:405d:d5ec:249d:857d:cce4, fe80::5c3:9b03:7083:341e%13, fe80::8820:6500:f15b:85af%2, fe80::bc24:d327:79d9:82e9%8, fe80::bf05:4369:9dcd:9303%7, fe80::c175:c827:26fe:dac5%21, fe80::e5e2:14e8:12ae:37f1%22, fe80::eb23:2eb7:8a4a:2918%4, fe80::f3b8:a247:f3b0:bdbd%5
       
  
    
 ```
 ## Check Connectvity
 winrm identify -r:http://127.0.0.1:5985 -auth:basic -u:Devops -p:password -encoding:utf-8
```
Set-ExecutionPolicy Unrestricted
```
